---
layout: post
title: "AWS - RedShift"
description: ""
category: posts
tags: [aws, redshift, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

Redshift is a data warehouse product that costs about $1000 per terabyte per year which is about 1/10 the cost of most data warehousing solutions. Instead of storing data in a row, data is stored in columns which makes it ideal for aggregating data. Data is stored squentially and compressed and along with Massive Parallel Processing make it really peppy. 

## Configurations

Small configuration is a single node and up to 160Gb.

Multi-Node - includes a leader node and up to 128 compute nodes.

In general, you can optimize on compute density using SSD storage (362 TB) or storage density using magnetic storage (2Pb).

Only in 1 AZ; no HA.

Redshift does not use indexes because the data is stored in columns.

Data is encrypted in transit and at rest using Redshift managed keys. Key Management through HSM or KMS is possible. Encryption is an immutable property of the cluster. The only way to switch from an encrypted cluster to a nonencrypted cluster is to unload the data and reload it into a new cluster. Encryption applies to the cluster and any backups. When you restore a cluster from an encrypted snapshot, the new cluster is encrypted as well.

## Operations

Data load - single source per load generally compressed from S3

Any SQL client that works with PostgreSQL works with Redshift.

The `EXPLAIN` command spits out the execution plan for the query.

### Resizing

0. Connections are terminated, cluster is restarted in readonly mode, all transactions in flight are rolled back

0. The service starts a new cluster and copies data from the only cluster and operates in a read-only mode

0. The endpoint is updated and the old cluster terminates all connections

### Cluster Termination

Shutting the cluster down snap shots the cluster for use at a later time but delete all automated snapshots.

Deleting a cluster does not create a final snapshot and deletes all the automated snapshots.

### Redshift Backups

Free storge for snapshots and backups up the amount of storage in the cluster. Automatic 

Snapshots are point-in-time backups. Redshift nodes are continuously backed up to S3

Restoring data from a snapshot by launching a new cluster and importing the data from the snapshot. The snapshot contains the number of nodes, type of nodes, the cluster configuration and the data included in the nodes.

Snapshots can be copied from one region to another manually and automatically. Automatic snap shotting can be configured with a retention period and does incur data transfer costs.

## Costing

Price = Compute Node Hours + backup costs + data transfer (within the VPC) 

Storage is provisioned as part of the node as long as the cluster is running so spot instances are not an option. On-demand instances can be added for scaling or temporary clusters. Reserved instances, given they are the right instance type and in the right AZ, can be used.

