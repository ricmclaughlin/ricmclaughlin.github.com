---
layout: post
title: "AWS - Storage Gateway"
description: ""
category: posts
tags: [aws, storage-gateway, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

The AWS Storage Gateway is a service connecting an on-premises software appliance with cloud-based storage to provide seamless and secure integration between an organization’s on-premises IT environment and AWS’s storage infrastructure. The software application is a VM and host local, can use Storage Gateway and DOES compress data at rest and in flight and supports bandwidth throttling. The service actives on port 80, uses port 3260 for iSCSI and needs port 53 and port 443 as well. AWS Storage Gateway can be deployed to an EC2 instance. One gateway per site... because it is an local appliance sort of thing which runes on VMware ESXi or Hyper-V

[File Interface](http://docs.aws.amazon.com/storagegateway/latest/userguide/create-file-gateway.html) - you store and retrieve objects from a local cache which syncs the file to Amazon S3

Volume Gateway are local vm based servers that enable file access with backup that can be mounted by and iSCSI device. Can be configured to use  point-in-time snapshots that are EBS volume capable (consistency = offline the volume to flush data to disk);

- Cached (Gateway-cached) - Basically a SAN with primary data written to S3; cached local for low latency; 32 volumes of 32TB each = 1 PM total storage; can snapshot to create a point in time version

- Stored (Gateway-stored) - store it local then async backed; 32 volumes of 16TB each = 512 TB; stores backups as EBS snapshots 

Tape gateway - (Gateway-Virtual Tape Library or Shelf) - Each Gateway-VTL presents a backup application with an industry-standard iSCSI-based Virtual Tape Library (VTL) consisting of a virtual media changer and tape drives that can hold up to 1PB. 

- Virtual tape libraries - S3 backed; 1500 tapes; instantaneous retrieval; 1 PB

- Virtual tape shelf - Glacier backed; unlimited tapes; 24 hour retrieval


