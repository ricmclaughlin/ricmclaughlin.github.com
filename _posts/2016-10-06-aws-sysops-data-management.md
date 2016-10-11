---
layout: post
title: "AWS SysOps - Data Management"
description: ""
category: posts
tags: [aws, sysopscert]
---
{% include JB/setup %}

## Services WITHOUT Automated Backup

EC2 does not have automated backup... snapshots of EBS volumes are incremental but when restored snapshots are complete.

### Automating EC2 Backup 
A python or bash script is required.

# Services with Automatic Backup


## ElastiCache
ElastiCache only really backs up Redis clusters. Snapshots backup the data for the entire cluster at a specific time and probably cause a performance degradation. Try to backup read replicas!

## Redshift
Free backup storage up to the size of the cluster! Backup can be automatic or manual and is incremental.

## RDS
Transactional Storage Engines are recommended for durability. RDS instances without Multi-AZ don't perform as well as those that are. Deleting an RDS instance deletes ALL the automated backups... but not the manual ones. Backups are stored on S3. Restoration only works for the default DB parameter and security groups are associated with the instance you will likely need to setup the DB default parameters and security groups. You maybe can change storage engine - if they are related.

### RDS Read Replicas
To setup a RDA read replica, auto backup must be enabled (this is done by turning the backup retention period to greater than 0) then RDS takes a backup copy of the database then enable replication. Without multi-AZ operations, IO is suspended while RDS snapshots the database. A transactional DB engine must be used to support read replicas - so Aurora/MySQL must have InnoDB engine installed. Needs MySQL version 5.6 or later.

Replica lag is a key metric - keeping the read replica on a similiar, in fact, exact same configuration of instances can help keep this metric inline. You can't enable "multi-az" on read replica - single AZ only.


### Multi-AZ failover
A multi-AZ failover process is key in the event of an AZ failure. Failover can be enabled from the Console or via the API. Replication from one zone to another causes a lot of higher write and commit latency - this is a syncronous process - provisioned IOPS is recommended. Patching is another benefit of standy instances - patch the standby, failover, then patch the primary. Backups can be created from the standy instance and that can eliminate I/O locking and latency spikes from backups on the primary.

Failover is automatically triggers when the AZ or the underlying hardware fails or there is a manual reboot with failover is initialized. A slow server and corrupt data will NOT lead to a failover. Getting notification about a failover as you would expect - RDS events.

Failovers are implemented as a DNS change so the application servers have to re-establish connections with the failover instance.

## Multi-Region Read Replica
Pretty much the same process as Multi-AZ.

# Disaster Recovery - Onsite -> AWS
A disaster is anything that has a negative impact on business continuity. AWS makes diaster recover way, way less expensive and easier to manage. One big way scenario is backing up your on-premise capability with AWS. Import/Export Snowball is great idea.

There are numerous ways approaching diaster recover:

- A **pilot light** is a small environment on AWS that can be scale quickly if there is a diaster recovery need.

- A **hot standby**, also known as multi-site, is when all resources are ready for use at a moments notice -> This is expensive.

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

#Labs
[Introduction to Amazon Relational Database Service (RDS) (Linux)](https://qwiklabs.com/focuses/2926)
