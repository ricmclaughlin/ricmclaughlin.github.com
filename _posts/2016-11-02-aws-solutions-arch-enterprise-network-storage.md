---
layout: post
title: "AWS Solutions Arch - Enterprise Network & Storage"
description: ""
category: posts
tags: [aws, soluarch, snowball, directconnect, storagegate, vpn]
---
{% include JB/setup %}

# What && Why && A Bit of Detail

# Direct Connect
[Direct Connect](https://aws.amazon.com/directconnect/) makes it easy to connect an on-premise data center with AWS. Instead of running AWS bound traffic out your regular Intenet connection, Direct Connect enables a 1 or 10 GB dedicated connections to be setup using industry standard 802.1 VLANS to your VPC - provided you remembered to address your VPC correctly! There are lower bandwidths of connection available but no SLA available. You can use BGP to have multiple direct connects in either an active-active or active-failover setup as direct connect is not redundunt. Overall Direct Connect gives you some great benefits:

1. Reduce your Internet Bandwidth cost - you are probably paying a lot of money to an ISP and could get a cheaper connection directly to AWS

1. Consistently higher bandwidth and lower latency 

1. Possibly higher 

[http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/images/basic-cust-gateway-diagram.png]
other direct connect thing as a gateway 

Customer Gateway - this is the customer side of the connection. 

Virtual Interfaces (VIF) - the connection. They come in two flavors:

- private VIF - used to access VPC using internal IP addresses or private VPC

- public VIF - allows public connections to EC2 or S3 or public VPC 

Virtual Private Gateways are the VPC end of the direct connect setup

In the US a single direct connect connections can access all 4 US regions.

# Import/Export Snowball
AWS used to offer the ability to send them a disk and have them import the disk to EBS, S3 or Glacier then to possibily export the data to S3 (only most recent version is exported if versioned.) This had to be totally painful so now they are a specially created device called an import/export Snowball - a portable NAS you can ship back to AWS and they import the data to S3. Import encryption is optional; exported data is encrypted.

# Storage Gateway
The AWS Storage Gateway is a service connecting an on-premises software appliance with cloud-based storage to provide seamless and secure integration between an organization’s on-premises IT environment and AWS’s storage infrastructure. The software application is a VM and host local, can use Storage Gateway and DOES compress data at rest and in flight and supports bandwidth throttling. The service actives on port 80, uses port 3260 for iSCSI and needs port 53 as well. Can be deployed to an EC2 instance. 

Gateway-cached - store in cloud; cache local for low latency; 32TB each * 32 volumes supported = 1PB

Gateway-stored - store it local; async back up to____ ; 16TB each * 12 volumes supported = 192TB; mounted as iSCSI

Gateway-Virtual Tape Library or Shelf - Each Gateway-VTL presents a backup application with an industry-standard iSCSI-based Virtual Tape Library (VTL) consisting of a virtual media changer and tape drives that can hold up to 1PB. Virtual tape libraries are backed by S3 with 1500 tapes = instantaneous access; virtual tape shelves are backed by Glacier with unlimited tapes = up to 24 hour access

Can be snapshot'ed (adhoc or scheduled) and used as an EBS volume; one gateway per site... because it is an local appliance sort of thing

##  When to do what...

### Direct connect vs  VPN Capability

VPN = quick to bring up and easy; slower and sucks Internet bandwidth

Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost

### Gateway Cached vs Gateway Stored vs Virtual Tape Library vs Virtual Tape Shelf vs Import/Export Snowball
Cached = less to maintain local, frequently access to small amount of data

Stored = low latency for ALL data

Virtual Tape Shelf - unlimited storage and OK with a 24 hour turn around time.

Virtual Tape Library - fast access but a little but more management

Snowball - lots of data with a small pipe

### Backup
Low end = s3 directly

## Resources
[BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol#Requirements_of_a_router_for_use_of_BGP_for_Internet_and_backbone-of-backbones_purposes)

[Import/Export Snowball](https://aws.amazon.com/importexport/)


