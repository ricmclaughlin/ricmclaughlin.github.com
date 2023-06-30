---
layout: post
title: "AWS - Elastic Block Store"
description: ""
category: posts
tags: [storage, aws-solutions-arch-pro]
---
{% include JB/setup %}

## EBS Volumes
Elastic Block Store is permanent storage with snapshot capability that can be mounted to an EC2 instance. Volumes are characterized by size, throughput, and IOPS. Can be optionally encrypted, which can not be modified after creation. Volumes can be resized and the type of volume can be changed as well. EBS backed EC2 instances boot faster than instance stores backed EC2. Generally mapped to /dev/sda1.

There are many types of EBS:
0. General purpose SSD (gp2/gp3) - gp2 has 250 MiB/s while gp3 has 1,000 MiB/s; can not multi-attach; GP3 is cheaper at time of writing. Gets burst performance; larger sizes = more MiB/s. 
0. Provisioned IOPS (io1, io2, i02 block express) - For databases... supports multi-attach, within the same AZ, on Linux. Not burst-able because provisioned. 
0. Throughput optimized (st1) - Cheap and slow EBS volumes. Can not specify IOPS with this storage type and it is not burst-able; not recommended for smaller read sizes using random access. Good for big data and data warehouses.
0. Cold HDD (sc1) - very cheap with good sequential read throughput

## Volumes &amp; Snapshots
Snapshots are incremental and stored on S3 but can only access them through the EBS service - you don't have access to them. Frequent snapshots improve RPO and RTO and don't cost much because they are deltas. You can copy snapshots across regions and make an AMI from a snapshot.

Ideally you want to detach the volume before snap-shotting. RAID array snapshots are a pain. You need to freeze the file system, unmount the RAID array; it is easiest is to stop the instance beforehand.

## Restoration
Restoration is a straight forward affair; simply, create a volume from the snapshot then mount it. From there you can also do a file level restore by copying the files to a regular production volume. 

EBS volumes restore from snapshots should be pre-warmed - use the _Fast Snapshot Restore_ (FSR - a new feature) OR go old school by using the `fio` or `dd` utility which reads from every block on the volume. 

## Data Lifecycle Manager
_Data Lifecycle Manager_ automates the creation, retention, and deletion of EBS snapshots and EBS-backed AMI. Can use EBS or EC2 tags to ID resources that should be backed up. Can't manage any data created outside of DLM or instance-store backed AMI.

It's more likely you would want to use AWS Backup, a centralize service for backing up EVERYTHING, instead of Data Lifecycle Manager which only works on EBS volumes.

## Encryption
EBS Volumes are only encryptable at creation and can't be un-encrypted by any means. If a snapshot is unencrypted, it can be encrypted. There is an account level encryption flag but must be enabled on each region.

Now the root device on an instance can be encrypted by setting the encryption flag when you create the instance. This is supported by all current generation instance types.

## Instance Store
An _Instance Store_ is ephemeral storage directly attached to an instance and always get deleted when an EC2 instance. Can not be added to an instance AFTER creation. Can be stripped; can not be increased in size (because it's attached to the instance). Instance Store is a much better name for this feature than the old name of it ("Instance Volume").

## EBS Root Devices &amp; Instance Termination
By default, EBS root volumes are lost when an instance is terminated. That said, there are numerous ways to preserve EBS root volumes before termination:

0. Take a snapshot of the EBS
0. Disable the "Delete on Termination" option
0. Attach EBS volumes after instance creation.... this is not exactly a root volume.

## EBS RAID
RAID can be implemented on EBS or Instance Store volumes.

- RAID 0 - striping; improves IOPS performance only; no fault tolerance (in fact it is less fault tolerant); 128k or 256k block size preferred; EBS optimized instance is preferable
- RAID 1 - mirror; no additional IOPS or throughput; increased fault tolerance
- RAID 5 - 4 data; 1 chksum; NEVER on EBS
- RAID 10 - Mirror and stripe

## EBS Monitoring
CloudWatch does not have access to disk space available... this is an OS specific metric. There are four status check statuses coming from EBS volumes:

- `ok` - All good.
- `warning` = Degraded or Severely Degraded
- `impaired` = Stalled or Not Available 
- `insufficient-data` = not enough data!