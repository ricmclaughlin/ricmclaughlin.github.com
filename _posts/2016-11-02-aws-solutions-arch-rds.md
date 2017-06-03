---
layout: post
title: "AWS Solutions Arch - RDS"
description: ""
category: posts
tags: [aws, rds, soluarch]
---
{% include JB/setup %}
DB do not require a port number or protocol.

## RDS

Transactional Storage Engines are recommended for durability. RDS instances without Multi-AZ don't perform as well as those that are - backups, restores and all housekeeping activities are performed on the secondary instance.  Backups are stored on S3. Restoration only works for the default DB parameter and security groups are associated with the instance you will likely need to setup the DB default parameters and security groups. You maybe can change storage engine - if they are related.

Deleting an RDS instance deletes ALL the automated backups... but not the manual ones. =>>"I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available."

A backup retention period of zero days will disable automated backups for this DB Instance. Restores manifest themselves as a new RDS instance with a new endpoint. Encryption uses KMS. "Restore to point in Time" option allows you to pinpoint a time to restore from. 

### Security

Encryption at rest must be setup at instance create time and uses KMS keys. Once enabled, EVERYTHING is encrypted which might cause a problem with cross region replications and snapshots seeing that KMS keys are region specific. The keys can't be changed after installation. All RDS instance types support encryption at rest.

Oracle and MS SQL Server support Transparent Data Encryption. Oracle does NOT integrate with KMS but will work with CloudHSM. SQL Server requires a key but that key is managed by RDS after enabling TDE.

Encryption in Transit - every RDS instance includes a SSL Certificate.


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

A transactional DB engine must be used to support read replicas - so Aurora/MySQL/MariaDB must have InnoDB engine installed. Needs MySQL version 5.6 or later; PostgresSQL requires 9.3.5. In this configuration, the native asynchronous engine is used.  MySQL allows multi-region read replicas.   Read-replicas can be promoted. 

Limits:

- max 5 read replica per primary instance

- DB Snapshots and backups can NOT be taken from read replicas.

- no read replicas from Oracle and SQL Server

Replica lag is a key metric - keeping the read replica on a similiar, in fact, exact same configuration of instances can help keep this metric inline. 

#### Read Replica Setup

There are several ways to create Read Replica:

1. Restore a snapshot and select Multi-AZ deployment.

2. Auto backup must be enabled (this is done by turning the backup retention period to greater than 0) then RDS takes a backup copy of the database then enables replication. Without multi-AZ operations, IO is suspended while RDS snapshots the database; with Multi-AZ operations a snapshot is used. This can be enabled from the console OR using the `CreateDBInstanceReadReplica` call from the API. A backup or snapshot must have been created to setup a read replica.

Replicas can be promoted to a primary but this breaks the replication link.

Replication can also be used as a disaster recovery or data migration mechanism. Start by using mysqldump/mysqlimport and requires MySQL 5.6.13.

## Aurora

MySQL compatible, relational DB that starts with 10Gb and scales in 10Gb increments to 64 Tb and up to 32 vCPUs and 244 Gb of RAM. 

Security is a good thing; must exist in a VPC, AES-256 for data in transit, use KMS keys or other keys to  encrypt storage, snapshots, backup, and replicas. If a database is created unencrypted you have to create another database.

It has amazing HA capabilities with 2 copies in 3 AZ so you get 6 copies of your data and it is self-healing through disk and data block data scanning and errors are fixed. You can have up to 15 Aurora read replicas and up to 5 MySQL read replicas (large affect on the primary because the transaction log is played against the replica)

There is automatic, continous incremental backups with a point-in-time restore within a second and are stored for 35 days by default.

## Oracle on RDS

Version of Oracle available include Oracle EE, Oracle SE, and Oracle SE One. Support Multi-AZ & manual snapshots. Oracle does NOT support multi-region replication.

Oracle RAC is not supported on RDS but can be implemented using EC2 placement groups and using Ntop N2n VPN software to tunnel between nodes. In addition Oracle Data Guard can be implemented as well.

Importing uses two different tools... Oracle SQL Developer for small amounts and Oracle Data Pump for large amounts of data. 

Recovery Manager (RMAN) can be used for backup and recovery scenarios including hybrid on-prem/Cloud scenarios and Oracle EC2 instances. Use RDS snapshots for point-in-time snapshots in AWS only solutions.

## MS SQL on RDS

Supports point-in-time backups using snapshots, Multi-AZ deployments using the native MS Mirroring technology but read replicas are not supported. There is no support for multi-region or Cloud to on-prem replication in RDS based MS SQL but native tools can be used. 

On-prem to Cloud migration is super ugly... create RDS empty tables, disable backups/key contraints, import flat files. In addition, FILESTREAM functions are not supported and there is no ability to restore data from file. 

#Labs

[Introduction to Amazon Relational Database Service (RDS) (Linux)](https://qwiklabs.com/focuses/2926)