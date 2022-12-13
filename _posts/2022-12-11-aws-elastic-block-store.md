---
layout: post
title: "AWS - Elastic Block Store"
description: ""
category: posts
tags: [elastic-block-store, aws, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

## EBS Volumes

Elastic Block Store is permanent storage with snapshot capability that can automatically be mounted to an EC2 instance. Optionally encrypted; can be resized. The type of volume can be changed as well. Pre-warming (using fio/dd) is no longer required for new volumes - only for restores from snapshots.

Generally mapped to /dev/sda1 and can be optionally encrypted. EBS can be sized up to 1 or 2 Tb depending on the OS and they boot faster than instance stores. Each EBS volume initially gets 5.4 million credits for burst performance (3000 IOPS for 30 minutes). 

There are many types of EBS:

1. General purpose SSD (gp2/gp3) - gp2 has 250 MiB/s while gp2 has 1,000 MiB/s; can not multi-attach

2. Provisioned IOPS (io1, io2, i02 block express) - For databases... supports multi-attach on Linux.

3. Throughput optimized (st1) - Cheap and slow EBS volumes. Can not specify IOPS with this storage type and it is not burstable; not recommended for smaller read sizes using random access. Good for big data and data warehouses.

4. Cold HDD (sc1) - very cheap with good sequential read throughput

## EBS Monitoring
CloudWatch does not have access to disk space available... this is an OS specific metric. There are four status check statuses coming from EBS volumes:

- `ok` - All good.

- `warning` = Degraded or Severely Degraded

- `impaired` = Stalled or Not Available 

- `insufficient-data` = not enough data!

## Resizing and Modifying an EBS Volume

It is possible to resize or modify an EBS volume while it is mounted.

To access an existing EBS from an instance first attach it, then create a file system, then mount it!

## EBS Root Devices &amp; Instance Termination

By default, EBS root volumes are lost when an instance is terminated. That said, there are numerous ways to preserve EBS root volumes after termination:

1. Take a snapshot of the EBS

2. Disable the "Delete on Termination" option

3. Attach EBS volumes after instance creation.... this is not exactly a root volume.

## Restoration

Restoration is a straight forward affair; simply, create a volume from the snapshot then mount it. From there you can also do a file level restore by copying the files to a regular production volume. 

EBS volumes restore from snapshots should be pre-warmed - use the Fast Snapshot Restore (FSR - a new feature) OR go old school and use the `fio` or `dd` utility which reads from every block on the volume - called pre-warming.

## Volumes &amp; Snapshots

Snapshots are incremental and stored on S3 but you don't have acces to them. Frequent snap shots improve RPO and RTO and don't cost much as they are deltas. Ideally you want to detach the volume.

You can copy snapshots across regions and make an AMI from a snapshot.

RAID array snapshots are a pain. You need to freeze the file system, unmount the RAID array; it is easiest is to stop the instance beforehand.

## Encryption

EBS Volumes are only encryptable at creation and can't be un-encrypted by any means. If a snapshot is unencrypted, it can be encrypted. There is an account level encryption flag but must be enabled on each region.

Now the root device on an instance can be encrypted by setting the encryption flag when you create the instance. This is supported by all current generation instance types.

## Instance Volumes

They are ephemeral and always get deleted when an EC2 instance. Can not be added to an instance AFTER creation. Can be stripped; can not be increased in size (because it's attached to the instance).

## Data Lifecycle Manager

Automates the creation, retention, and deletion of EBS snapshots and EBS-backed AMI. Can use EBS or EC2 tags to ID resources that should be backed up.

Can't manage any data created outside of DLM or instance-store backed AMI.

## EBS RAID

RAID can be implemented on EBS or Instance Store volumes.

- RAID 0 - striping; improves IOPS performance only; no fault tolerance; 128k or 256k block size preferred; EBS optimzed instance is preferrable

- RAID 1 - mirror; no additional IOPS or throughput; increased fault tolerance

- RAID 5 - 4 data; 1 chksum; NEVER on EBS

- RAID 10 - Mirror and stripe