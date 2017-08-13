---
layout: post
title: "AWS - Storage Gateway"
description: ""
category: posts
tags: [aws, storage-gateway, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

The AWS Storage Gateway is a service connecting an on-premises software appliance with cloud-based storage to provide seamless and secure integration between an organization’s on-premises IT environment and AWS’s storage infrastructure. The software application is a VM and host local, can use Storage Gateway and DOES compress data at rest and in flight and supports bandwidth throttling. The service actives on port 80, uses port 3260 for iSCSI and needs port 53 as well. Can be deployed to an EC2 instance. One gateway per site... because it is an local appliance sort of thing

[File gateway](http://docs.aws.amazon.com/storagegateway/latest/userguide/create-file-gateway.html) - you store and retrieve objects in Amazon S3 with a local cache for low latency access to your most recently used data.

Volume Gateway are local vm based servers that enable file access with backup that can be mounted by and iSCSI device.

- Cached (Gateway-cached) - cached local for low latency; 150TiB total storage

- Stored (Gateway-stored) - store it local then async back to s3; 12 TiB; point-in-time snapshots (consistency = offline the volume to flush data to disk); EBS volume capable

Tape gateway - (Gateway-Virtual Tape Library or Shelf) - Each Gateway-VTL presents a backup application with an industry-standard iSCSI-based Virtual Tape Library (VTL) consisting of a virtual media changer and tape drives that can hold up to 1PB. 

- Virtual tape libraries - S3 backed; 1500 tapes; instantaneous retrieval

- Virtual tape shelf - Glacier backed; unlimited tapes; 24 hour retrieval


