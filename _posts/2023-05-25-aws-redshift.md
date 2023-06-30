---
layout: post
title: "AWS - RedShift"
description: ""
category: posts
tags: [analytics, aws-solutions-arch-pro, aws-ml-spec]
---
{% include JB/setup %}

Redshift is a data warehouse product that costs about $1000 per terabyte per year which is about 1/10 the cost of most data warehousing solutions. Instead of storing data in a row, data is stored in columns which makes it ideal for aggregating data. Data is stored sequentially and compressed and when combined with Massive Parallel Processing (MPP) the system is quite peppy. 

## Configurations
Only in 2 AZ; so limited HA. Redshift does not use indexes because the data is stored in columns.

- Small configuration is a single node and up to 160Gb.
- Multi-Node - includes a leader node and up to 128 compute nodes.

In general, you can optimize on compute density using SSD storage (362 TB) or storage density using magnetic storage (2Pb). This service attempts to maintain at least three copies of your data (original, recompute nodes, S3).

Similar to RDS, Redshift uses parameter groups to standardize the configuration of a cluster. Workload Management groups are a big part of the standardization of a cluster. By default the WLM group contains one queue that can run 5 queries concurrently. More queues can be added; the last queue in the list of queues is the default queue. Additionally, users, query groups, and user groups can be defined to allow tailored access to the cluster.

### Encryption
Data is encrypted in transit and at rest using Redshift managed keys. Key Management through HSM or KMS is possible. Encryption is an immutable property of the cluster. The only way to switch from an encrypted cluster to a non-encrypted cluster is to unload the data and reload it into a new cluster. Encryption applies to the cluster and any backups. When you restore a cluster from an encrypted snapshot, the new cluster is encrypted as well.

## Operations
Data load - single source per load; generally compressed from S3 but others as well

Redshift Enhanced VPC Routing enables faster COPY and UNLOAD through the VPC.

Any SQL client that works with PostgreSQL works with Redshift.

### Resizing
0. Connections are terminated, cluster is restarted in readonly mode, all transactions in flight are rolled back
0. The service starts a new cluster and copies data from the only cluster and operates in a read-only mode
0. The endpoint is updated and the old cluster terminates all connections

A better way is probably to snapshot the cluster, load it on a new resized cluster, and then switch the connection endpoint to the resized cluster when the resize is complete

### Cluster Termination
Shutting the cluster down snap shots the cluster for use at a later time but delete all automated snapshots.

Deleting a cluster does not create a final snapshot and deletes all the automated snapshots.

## Redshift Backups
Redshift includes free automatic storage for snapshots with a 35 day retention period and backups up the amount of storage in the cluster.  

Snapshots are point-in-time backups. Redshift nodes are continuously backed up to S3.

Automatic snap shotting happens every 8 hours, every 5 GB or on a schedule with configurable retention period (default of 1 days); these can not be manually deleted but are automatically deleted after the retention period is up. Manual snapshots are retained until you delete them. The _Redshift Automated Backups_ feature copies snapshots from one region to another manually or automatically BUT does incur data transfer costs. To transfer encrypted snapshots cross region, a snapshot copy grant must be preformed so that Redshift access the KMS key.

### Restoring Backups
Restoring data from a snapshot by launching a new cluster and importing the data from the snapshot. The snapshot contains the number of nodes, type of nodes, the cluster configuration and the data included in the nodes.

## Costing
Price = Compute Node Hours + backup costs + data transfer (within the VPC) 

Notice that the leader node is not a cost! Storage is provisioned as part of the node as long as the cluster is running so spot instances are not an option. On-demand instances can be added for scaling or temporary clusters. Reserved instances, given they are the right instance type and in the right AZ, can be used.

## Redshift Spectrum
Redshift Spectrum enables a cluster to query data stored in S3 by spinning up thousands of Redshift Spectrum nodes to do the query. Then the results are transmitted to the compute nodes for aggregation. 

## Redshift Workload Management (WLM)
Amazon Redshift WLM creates query queues at runtime to keep short, fast queries from getting stuck behind long running queries. There are two modes: Automatic, where the queues and resources as managed by Redshift, and Manual, where you manage queues and resources.

## Redshift Concurrency Scaling
With the Concurrency Scaling feature, there is support for virtually unlimited concurrent users and concurrent queries, with consistently fast query performance by adding additional cluster capacity to process an increase in both read and write queries. This feature users the WLM feature to prioritize queries. This service charges by the second.

## Redshift ML
Redshift ML enables the creation, training, and deployment of machine learning models directly from a Redshift cluster. The process works like:
1. Automaticall export the results of a query to a bucket
2. Calls SageMaker AutoPilot to generate the model
3. Uses SageMaker Neo to optimize the model then makes it available as a SQL function
4. SQl function is exposed via a SageMaker endpoint.