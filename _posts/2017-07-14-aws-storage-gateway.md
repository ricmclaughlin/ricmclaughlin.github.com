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

Volume Gateway are local vm based servers that enable file access with backup.

- Cached (Gateway-cached) - store in cloud; cache local for low latency; 32TB each * 32 volumes supported = 1PB

- Stored (Gateway-stored) - store it local; 512TB; mounted as iSCSI; point-in-time snapshots (consistency = offline the volume to flush data to disk) stored on S3; EBS volume capable

Tape gateway - (Gateway-Virtual Tape Library or Shelf) - Each Gateway-VTL presents a backup application with an industry-standard iSCSI-based Virtual Tape Library (VTL) consisting of a virtual media changer and tape drives that can hold up to 1PB. 

- Virtual tape libraries - S3 backed; 1500 tapes; instantaneous retrieval; 

- Virtual tape shelf - Glacier backed; unlimited tapes; 24 hour retrieval

### Gateway Cached vs Gateway Stored vs Virtual Tape Library vs Virtual Tape Shelf vs Import/Export Snowball

- Low end DR = S3 directly

- Gateway Cached = less to maintain local so cheaper outlay, frequently access to small amount of data

- Gateway Stored = low latency for ALL data; snapshots can be used as a migration tool 

- Virtual Tape Shelf - unlimited storage and OK with a 24 hour turn around time.

- Virtual Tape Library - fast access. 1500 tape..

- Snowball - lots of data with a small pipe
