---
layout: post
title: "AWS Solutions Arch - Enterprise Network & Storage"
description: ""
category: posts
tags: [aws, soluarch, snowball, directconnect, storagegate, vpn, soluarchpro]
---
{% include JB/setup %}

# Direct Connect
[Direct Connect](https://aws.amazon.com/directconnect/) makes it easy to connect an on-premise data center with AWS. Instead of running AWS bound traffic out your regular Intenet connection, Direct Connect enables a 1 or 10 GB dedicated connections to be setup using industry standard 802.1 VLANS to your VPC - provided you remembered to subnet and address your VPC correctly! There are lower bandwidths of connection available through partners but no SLA available. You can use BGP to have multiple direct connects in either an active-active or active-failover setup as direct connect is not redundunt by default. Overall, Direct Connect gives you some great benefits:

1. Reduce your Internet Bandwidth cost - you are probably paying a lot of money to an ISP and could get a cheaper connection directly to AWS

1. Consistently higher bandwidth and lower latency 

## Direct Connect Components
Customer Gateway - this is the customer side of the connection. 

Virtual Interfaces (VIF) - the connection. They come in two flavors:

- private VIF - used to access VPC using internal IP addresses or private VPC

- public VIF - allows public connections to EC2 or S3 or public VPC 

Virtual Private Gateways are the VPC end of the direct connect setup

In the US a single direct connect connections can access all 4 US regions.

# Import/Export Snowball
AWS used to offer the ability to send them a disk and have them import the disk to EBS, S3 or Glacier then to possibily export the data to S3 (only most recent version is exported if versioned.) This had to be totally painful so now they are a specially created device called an import/export Snowball - a portable NAS you can ship back to AWS and they import the data to S3. Import encryption is optional; exported data is encrypted. Data can only be exported from S3.

# Storage Gateway
The AWS Storage Gateway is a service connecting an on-premises software appliance with cloud-based storage to provide seamless and secure integration between an organization’s on-premises IT environment and AWS’s storage infrastructure. The software application is a VM and host local, can use Storage Gateway and DOES compress data at rest and in flight and supports bandwidth throttling. The service actives on port 80, uses port 3260 for iSCSI and needs port 53 as well. Can be deployed to an EC2 instance. One gateway per site... because it is an local appliance sort of thing

Gateway-cached - store in cloud; cache local for low latency; 32TB each * 32 volumes supported = 1PB

Gateway-stored - store it local; 512TB; mounted as iSCSI; point-in-time snapshots; EBS volume capable

Gateway-Virtual Tape Library or Shelf - Each Gateway-VTL presents a backup application with an industry-standard iSCSI-based Virtual Tape Library (VTL) consisting of a virtual media changer and tape drives that can hold up to 1PB. 

- Virtual tape libraries - S3 backed; 1500 tapes; instantaneous retrieval; 

- Virtual tape shelves - Glacier backed; unlimited tapes; 24 hour retrieval

##  Use Cases

### Storage

S3 protect from delete or overwrite - enable versioning

On certain folks can delete and overwrite? enable MFA

Cross region replication? Requires versioning on source bucket

### Storage Tiers & Lifecycle Policies

Rebuildable? S3-SSR

Infrequently access? S3-IA

greater than 4 hours retrieval, cheap? Glacier

### Storage Rules

- Can't move from SA-IA -> RRS or S3

- Move to S3-IA? 30 days in current storage class

- Move to Glacier? if 128k (directlyAfterCreation == true || daysInS3-IA > 30 days) 

- Move from Glacier? nope.

### Direct connect vs VPN Capability

- VPN = quick to bring up and easy; slower and sucks Internet bandwidth

- Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost

### Gateway Cached vs Gateway Stored vs Virtual Tape Library vs Virtual Tape Shelf vs Import/Export Snowball

- Low end = S3 directly

- Gateway Cached = less to maintain local so cheaper outlay, frequently access to small amount of data

- Gateway Stored = low latency for ALL data

- Virtual Tape Shelf - unlimited storage and OK with a 24 hour turn around time.

- Virtual Tape Library - fast access. 1500 tape..

- Snowball - lots of data with a small pipe

## Resources
[BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol#Requirements_of_a_router_for_use_of_BGP_for_Internet_and_backbone-of-backbones_purposes)

[Network Admin Guide to VPC](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Introduction.html)

[Import/Export Snowball](https://aws.amazon.com/importexport/)


