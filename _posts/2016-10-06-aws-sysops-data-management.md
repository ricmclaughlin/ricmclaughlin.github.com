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
ElastiCache only backs up Redis clusters. Snapshots backup the data for the entire cluster at a specific time and probably cause a performance degradation. Try to backup read replicas!

## Redshift
Free backup storage up to the size of the cluster! Backup can be automatic or manual and is incremental. Only 1 day retention period by default.

## RDS
Transactional Storage Engines are recommended for durability. RDS instances without Multi-AZ don't perform as well as those that are - backups, restores and all housekeeping activities are performed on the secondary instance.  Backups are stored on S3. Restoration only works for the default DB parameter and security groups are associated with the instance you will likely need to setup the DB default parameters and security groups. You maybe can change storage engine - if they are related.

Deleting an RDS instance deletes ALL the automated backups... but not the manual ones. -=>>"I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available."

### Multi-AZ failover
A multi-AZ failover process is key in the event of an AZ failure but is NOT a scaling solution.  Multi-AZ systems are setup within the same region - that would be obvious from the name.

For MySQL, Oracle and PostgreSQL replication from one zone to another causes a lot of higher write and commit latency - this is a synchronous physical replication - provisioned IOPS is recommended. Native replication, called Mirroring, is used for MS SQLServer.

Patching is another benefit of standy instances - patch the standby, failover, then patch the primary. Backups can be created from the standy instance and that can eliminate I/O locking and latency spikes from backups on the primary.

Failover is automatically triggers when the AZ or the underlying hardware fails or you can force a fail over using a *manual reboot* using the console or with `RebootDBInstance` API call given that failover is initialized. A slow server and corrupt data will NOT lead to a failover. Getting notification about a failover as you would expect - RDS events.

Failovers are implemented as a DNS change so the application servers have to re-establish connections with the failover instance.

Automated backup is done from the backup instances NOT the primary instance so there is no performance hit during backups.

### RDS Read Replicas
Read Replicas are used to scale RDS by creating a READ ONLY copy of your database in a single AZ. Possible use cases include:

- Scale beyond single instance I/O capabilities

- Serve read-only traffic during times the primary is unavailable (hard to visualize a scenarios where this is useful)

- Business reporting against almost live data without affecting the performance of the primary instance

A transactional DB engine must be used to support read replicas - so Aurora/MySQL must have InnoDB engine installed. Needs MySQL version 5.6 or later; PostgresSQL requires 9.3.5. In this configuration, the native asynchronous engine is used. There can be a total of 5 read replica per primary instance. Read Replicas are single AZ. MySQL allows multi-region read replicas. DB Snapshots and backups can NOT be taken from read replicas. 

Replica lag is a key metric - keeping the read replica on a similiar, in fact, exact same configuration of instances can help keep this metric inline. 

There are several ways to create Read Replica.

1. Restore a snapshot and select Multi-AZ deployment.

2. Auto backup must be enabled (this is done by turning the backup retention period to greater than 0) then RDS takes a backup copy of the database then enables replication. Without multi-AZ operations, IO is suspended while RDS snapshots the database; with Multi-AZ operations a snapshot is used. This can be enabled from the console OR using the `CreateDBInstanceReadReplica` call from the API. A backup or snapshot must have been created to setup a read replica.

Replicas can be promoted to a primary but this breaks the replication link.



#Labs
[Introduction to Amazon Relational Database Service (RDS) (Linux)](https://qwiklabs.com/focuses/2926)
