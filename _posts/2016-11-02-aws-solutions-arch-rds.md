---
layout: post
title: "AWS Solutions Arch - RDS"
description: ""
category: posts
tags: [aws, rds, soluarch]
---
{% include JB/setup %}


## RDS
Transactional Storage Engines are recommended for durability. RDS instances without Multi-AZ don't perform as well as those that are - backups, restores and all housekeeping activities are performed on the secondary instance.  Backups are stored on S3. Restoration only works for the default DB parameter and security groups are associated with the instance you will likely need to setup the DB default parameters and security groups. You maybe can change storage engine - if they are related.

Deleting an RDS instance deletes ALL the automated backups... but not the manual ones. =>>"I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available."

A backup retention period of zero days will disable automated backups for this DB Instance. Restores manifest themselves as a new RDS instance with a new endpoint. Encryption uses KMS. "Restore to point in Time" option allows you to pinpoint a time to restore from. RDS security groups do not require a port number or protocol.

### Multi-AZ failover
A multi-AZ failover process is key in the event of an AZ failure but is NOT a scaling solution.  Multi-AZ systems are setup within the same region - that would be obvious from the name.

For MySQL, Oracle and PostgreSQL replication from one zone to another causes a lot of higher write and commit latency - this is a synchronous physical replication - provisioned IOPS is recommended. Native replication, called Mirroring, is used for MS SQLServer.

Patching is another benefit of standy instances - patch the standby, failover, then patch the primary. Backups can be created from the standy instance and that can eliminate I/O locking and latency spikes from backups on the primary.

Failover is automatically triggers when the AZ or the underlying hardware fails or you can force a fail over using a *Reboot with failover* using the console or with `RebootDBInstance` API call given that failover is initialized. A slow server and corrupt data will NOT lead to a failover. Getting notification about a failover as you would expect - RDS events.

Failovers are implemented as a DNS change so the application servers have to re-establish connections with the failover instance.

Automated backup is done from the backup instances NOT the primary instance so there is no performance hit during backups.

### RDS Read Replicas
Read Replicas are used to scale RDS by creating a READ ONLY copy of your database in a single AZ. Possible use cases include:

- Scale beyond single instance I/O capabilities

- Serve read-only traffic during times the primary is unavailable (hard to visualize a scenarios where this is useful)

- Business reporting against almost live data without affecting the performance of the primary instance

A transactional DB engine must be used to support read replicas - so Aurora/MySQL must have InnoDB engine installed. Needs MySQL version 5.6 or later; PostgresSQL requires 9.3.5. In this configuration, the native asynchronous engine is used. There can be a total of 5 read replica per primary instance. MySQL allows multi-region read replicas. DB Snapshots and backups can NOT be taken from read replicas. If not Multi-AZ on setup, expect I/O suspension; Multi-AZ = snapshot from secondary DB. Read-replicas can be promoted. Oracle and SQL Server can't do read replicas.

Replica lag is a key metric - keeping the read replica on a similiar, in fact, exact same configuration of instances can help keep this metric inline. 

#### Read Replica Setup
There are several ways to create Read Replica.

1. Restore a snapshot and select Multi-AZ deployment.

2. Auto backup must be enabled (this is done by turning the backup retention period to greater than 0) then RDS takes a backup copy of the database then enables replication. Without multi-AZ operations, IO is suspended while RDS snapshots the database; with Multi-AZ operations a snapshot is used. This can be enabled from the console OR using the `CreateDBInstanceReadReplica` call from the API. A backup or snapshot must have been created to setup a read replica.

Replicas can be promoted to a primary but this breaks the replication link.

## Aurora
MySQL compatible, relational DB that starts with 10Gb and scales in 10Gb increments to 64 Tb and up to 32 vCPUs and 244 Gb of RAM. It has amazing HA capabilities with 2 copies in 3 AZ so you get 6 copies of your data and it is self-healing through disk and data block data scanning and errors are fixed. You can have up to 15 Aurora read replicas and 5 MySQL read replicas.

#Labs
[Introduction to Amazon Relational Database Service (RDS) (Linux)](https://qwiklabs.com/focuses/2926)