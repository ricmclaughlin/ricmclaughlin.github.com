---
layout: post
title: "AWS SysOps - Elastic Block Storage"
description: ""
category: posts
tags: [aws, sysopscert, ebs]
---
{% include JB/setup %}

## Instance Volumes
They are ephemeral and always get deleted when an EC2 instance is deleted. They can be up to 10Gb and boot slower than EBS volumes.

## EBS Volumes
Elastic Block Storage - permanent storage with snapshot capability automatically mounted. There are three different volume types including general purpose, provisioned IOPS SSD which can be striped together for faster performance and magnetic volumes. Generally mapped to /dev/sda1 and can be optionally encrypted. EBS can be sized up to 1 or 2 Tb depending on the OS and they boot faster than instance stores. Each EBS volume initially gets 5.4 million credits for burst performance (3000 IOPS for 30 minutes).

There are three types of EBS:

1. General purpose SSD (gp2) - Minimum of 100 IOPS then 3 IOPS per GB; can burst to 3000 IOPS if less than 1 TB. The max throughput is 160 MiB/s and this can end up being a bottleneck if read size is large. Even though you can boost performance, use if you are after less than 10K IOPS; 3Tb gets you there.

2. Provisioned IOPS (io1) - 30 IOPS per GB up to 20,000 IOPS; Steady consisten IOPS not really a burstable; The max throughput is 320 MiB/s; Use if you are after over 10k IOPS

3. Magnetic - Cheap and slow EBS volumes can also be Cold HDD (sc1) and Throughput Optimized HDD (st1). Can not specify IOPS with this storage type.


### Initializing EBS Volumes (pre-warm)
For peak performance from the EBS volume if you are mounting it for the first time you can do a `dd if={mount location} of=/dev/null bs=1M` which writes to every block. If creating from a snapshot use the `fio` utility which reads from every block on the volume.

### Resizing and Modifying an EBS Volume
It is not possible to resize or modify an EBS volume while it is mounted. Take a snapshot, modify as you would like then stop the running instance, attach, then restart the instance.

To access an existing EBS from an instance first attach it, then create a file system, then mount it!

## EBS Root Devices &amp; Instance Termination
By default, EBS root volumes are lost when an instance is terminated. That said, there are numerous ways to preserve EBS root volumes after termination:

1. Take a snapshot of the EBS

2. Disable the "Delete on Termination" option

3. Attach EBS volumes after instance creation.... this is not exactly a root volume.

