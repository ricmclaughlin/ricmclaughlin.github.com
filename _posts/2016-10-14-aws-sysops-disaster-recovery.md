---
layout: post
title: "AWS SysOps - Disaster Recovery"
description: ""
category: posts
tags: [aws, sysopscert]
---
{% include JB/setup %}

A disaster is anything that has a negative impact on business continuity. 

## RTO &amp; RPO
Recovery Time Objective (RTO) - From disaster all the way back to normal

Recovery Point Objective (RPO) - how much data you can loose

# Disaster Recovery - Onsite -> AWS
AWS makes diaster recover way, way less expensive and easier to manage. One big way scenario is backing up your on-premise capability with AWS. Import/Export Snowball is great idea.

There are numerous ways approaching diaster recover:

- **Backup and Restore** - long RTO; use Direct Connect, Snowball and EC2 backup => S3 

- A **pilot light** is a small environment on AWS that can be scale quickly if there is a diaster recovery need. RDS is a key component so that DBs can come up quickly along with bundled AMIs for EC2. Preallocated ELB and ENI (with a pre-allocated mac address) can also useful for apps that require a dedicated IP or MAC address. Upon disaster, using Route53 you can redirect traffic to your AWS resources.

- A **warm standby**, which feels like a low cost multi-site setup, is when all resources, but likely smaller instances and version, are ready for use at a moments notice. This is expensive.

- **Multi-Site** - On the most expensive end but very low RTO and RPO you can use Route53 weighted routed between multiple regions. 

## Storage Gateway
Storage Gateways help extend local disaster recover to the cloud and augment local IT capabilities. There are three types of storage gateways:

1. Gateway Stored Volumes - The entire dataset is stored onsite and async'd backed to S3

2. Gateway Cached Volumes - The entire dataset is stored on S3 and frequently access materials are cached onsite.

3. Gateway Virtual Tape Library - Offsite backup gateway to backup stuff offsite directly.


# Diaster Recover on AWS
Tons of learning in this areas... the big issues surrounding disaster recover ON AWS include: 

- AMI are region specific

- EC2 key pairs are region specific

- Read replicas can have delays

## Resources
[Disaster Recovery](http://media.amazonwebservices.com/AWS_Disaster_Recovery.pdf)