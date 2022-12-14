---
layout: post
title: "AWS - Storage Gateway"
description: ""
category: posts
tags: [aws, storage-gateway, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

The AWS Storage Gateway is a service connecting an on-premises software appliance with cloud-based storage to provide seamless and secure integration between on-premises IT environment and AWS. The software application is a VM, can use Storage Gateway and DOES compress data at rest and in flight and supports bandwidth throttling. The service actives on port 80, uses port 3260 for iSCSI and needs port 53 and port 443 as well. AWS Storage Gateway is deployed as some sort of VM or as a hardware appliance (which works as a File/Volume/Tape Gateway). 

Storage Gateway doesn't support resource-based policies

[S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/what-is-file-s3.html) - files are stored on S3 (in all classes EXCEPT Glacier) and a NFS/SMB mount point exposing something like network attached storage. Most recent data is cached on the gateway. Gateways can point to a Bucket, a S3 Access point, or S3 Access Point Alias AND fully supports VPC endpoints, encryption (S3-SSE, KMS), and logging. SMB has integration with AD. NFS supports POSIX access control.

Each file share can only connect to one S3 bucket, but multiple file shares can connect to the same bucket. If you connect more than one file share to the same bucket, you must configure each file share to use a unique, non-overlapping prefix name to prevent read/write conflicts.

[FSx Gateway](https://docs.aws.amazon.com/filegateway/latest/filefsxw/what-is-file-fsxw.html) - FSx Gateway enables a caching solution a FSx for Windows File Server.

[Volume Gateway](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html) are on-prem vm based servers that enable file access with backup that can be mounted by as an iSCSI device. Can be configured to use point-in-time snapshots that are EBS volume capable (consistency = offline the volume to flush data to disk).

- Cached - Basically a SAN with primary data written to S3; cached local for low latency; can snapshot to create a point in time version

- Stored - store it local then async backed; 32 volumes of 16TB each = 512 TB; stores backups as EBS snapshots 

[Tape Gateway](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html) - Each Gateway-VTL presents a backup application with an industry-standard iSCSI-VTL (Virtual Tape Library) consisting of a virtual media changer and tape drives that can hold up to 1PB. 

- Virtual tape libraries - S3 backed; 1500 tapes; instantaneous retrieval; 1 PB

- Virtual tape shelf - Glacier backed; unlimited tapes; 24 hour retrieval


