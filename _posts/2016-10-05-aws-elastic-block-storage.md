---
layout: post
title: "AWS - Elastic Block Storage"
description: ""
category: posts
tags: [elastic-block-storage, aws, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

## Instance Volumes

They are ephemeral and always get deleted when an EC2 instance is deleted. They can be up to 10Gb and boot slower than EBS volumes.

## EBS Volumes

Elastic Block Storage is permanent storage with snapshot capability automatically mounted. There are three different volume types including general purpose, provisioned IOPS SSD which can be striped together for faster performance and magnetic volumes. Generally mapped to /dev/sda1 and can be optionally encrypted. EBS can be sized up to 1 or 2 Tb depending on the OS and they boot faster than instance stores. Each EBS volume initially gets 5.4 million credits for burst performance (3000 IOPS for 30 minutes).(Linux Academy say max 16TiB; AWS says max 20TiB)

There are three types of EBS:

1. General purpose SSD (gp2) - Minimum of 100 IOPS then 3 IOPS per GB; can burst to 3000 IOPS if less than 1 TB. The max throughput is 160 MiB/s and this can end up being a bottleneck if read size is large. These instances include a burst pool of 5.4 Millions IOPS which can be burned at 3000 MAX. Even though you can boost performance, use if you are after less than 10K IOPS; 3Tb gets you there. 

2. Provisioned IOPS (io1) - 30 IOPS per GB up to 20,000 IOPS to a maxiuim of 320 MiB/s; Steady consisten IOPS not really a burstable; Use if you are after over 10k IOPS

3. Magnetic - Cheap and slow EBS volumes can also be Cold HDD (sc1) and Throughput Optimized HDD (st1). Can not specify IOPS with this storage type and it is not burstable; not recommended for smaller read sizes using random access

### EBS Monitoring

There are four status check statuses coming from EBS volumes:

- `ok` - All good.

- `warning` = Degraded or Severely Degraded

- `impaired` = Stalled or Not Available 

- `insufficient-data` = not enough data!

### Initializing EBS Volumes (pre-warm)

This is no longer required for new volumes.

For peak performance from the EBS volume if you are mounting it for the first time you can do a `dd if={mount location} of=/dev/null bs=1M` which writes to every block. 

If creating from a snapshot use the `fio` utility which reads from every block on the volume - something like `sudo fio --filename=/dev/xvdf1 --rw=randread --bs=128k --iodepth=32 --ioengine=libaio --direct=1 --name=volume-initialize`


### Resizing and Modifying an EBS Volume

It is not possible to resize or modify an EBS volume while it is mounted. Take a snapshot, modify as you would like then stop the running instance, attach, then restart the instance.

To access an existing EBS from an instance first attach it, then create a file system, then mount it!

## EBS Root Devices &amp; Instance Termination

By default, EBS root volumes are lost when an instance is terminated. That said, there are numerous ways to preserve EBS root volumes after termination:

1. Take a snapshot of the EBS

2. Disable the "Delete on Termination" option

3. Attach EBS volumes after instance creation.... this is not exactly a root volume.

## Volumes &amp; Snapshots

Snapshots are incremental. Stop instances to snapshots the root devices else EC2 stops it for you. Frequent snap shots improve RPO and RTO and don't cost much as they are deltas.

RAID array snapshots are a pain. You need to freeze the file system, unmount the RAID array although it is easiest is to stop the instance.

### RAID

- RAID 0 - striping; good performance; 128k or 256k block size preferred

- RAID 1 - mirror

- RAID 5 - 4 data; 1 chksum; NEVER on EBS

- RAID 10 - Mirror and stripe

## Encryption

EBS Volumes are only encryptable at creation and can't be un-encrypted by any means. Encrypted volumes are snap-shotted encrypted automatically - and are restored encrypted. Unencrypted snapshots can be encrypted during copy using the any key you specify.

## Backup Strategies

There are several situations to be fimiliar with:

- Restoring by mounting volumes

- Backup using Snapshots (which are object level storage and incremental backups...)

- Application-consistent snapshots are super challenging because they include memory, in-process transations and the data on disk

- AMI "pre-baking" reduces the instance spin-up time and can also serve as a backup strategy

Hot backups are a big case of backup... you can:

- first pausing I/O using xfs_freeze or fsfreeze (ext based filesystems)

- unmount, snapshot and remount

- Use a Logical Volume Manager & backup the LVM snapshots inside the EBS snapshot... then delete the LVM snapshot

- LVM is also a good choice for RAID Volume Snapshots

## Use Cases

Long term data storage? EBS

data shared between instance fast? memecached or redis

persist data shared between instances? EFS

don't care? instance store

DB or INTENSE datawarehouse server = IOPS/io1

General purpose = gp2

Large I/O including EMR, Kafka, log processing and data warehouse ETL = st1 (sequential data reads)

Super high disk IO? either RAID 0 or RAID 10 EBS

## Monitoring

### gp2 volumes

`VolumeReadBytes` & `VolumeWriteBytes` - throughput measurements can be reported in SUM or more importantly AVERAGE. Average should tell you if you are ending up with a bottleneck or excess throughput.

`VolumeReadOps` & `VolumeWriteBytes` - This tells up the number of IOPS

`VolumeTotalReadTime` & `VolumeTotalWriteTime` - Looking a trend line of this metric can show what the future requirements might be.

`VolumeQueueLength` - How long the queue is can determine usage patterns. 

#### io1 volumes

The `VolumeThroughputPercentage` & `VolumeConsumedReadWriteOps` metrics show how much of the provisioned throughput you are using. 