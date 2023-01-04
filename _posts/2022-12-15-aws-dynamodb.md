---
layout: post
title: "AWS - DynamoDB"
description: ""
category: posts
tags: [aws-services, aws, databases, aws-solutions-arch-pro]
---
{% include JB/setup %}


[DynamoDB](https://aws.amazon.com/dynamodb/) is a NoSQL database - but the concepts behind MongoDB have better names. It stores unlimited amounts of data in three separate data centers - not AZs.

By default, reads of a DynamoDB table are eventually consistent but also supports strongly consistent reads. DynamoDB uses optimistic concurrency control. Now DynamoDB support ACID transactions as well. **Eventually Consistent Reads** - By default, once there is a DynamoDB write you can access data with a second or so it is eventually consistent. **Strongly Consistent Reads** - a strongly consistent read reflects all the writes that have been received a successful response prior to the read. 

**DynamoDB limits** - 400kb per record; 2,500 tables per region, 10,000 writes/reads per second (soft limit). The maximum size of a DynamoDB range primary key attribute value is 1024 bytes. 

## Pricing
Pricing with DynamoDB is complicated; first there are two capacity modes: 
0. _Provisioned Mode_ where the ceiling and floor are enabled, WCU/RCU is managed manually or through Auto Scaling, and there is the ability to purchase capacity reservation to lower cost. With provisioned capacity mode, DynamoDB charges for the number of reads and writes per second. Predictable, gradual ramps, known traffic, and ongoing monitoring are great fits for this mode. 
0. _On-demand Mode_ which should be called "set and forget" mode. With on-demand capacity mode, DynamoDB charges for the data reads and writes your application performs on your tables. This mode is great for new/unpredictable workloads, frequently idle workloads, and events with unknown traffic. 

Then there are other charges for disk storage, DynamoDB streams, export and data transfer out, global tables, and backup. 

Provisioned mode results in cheaper bills much like a reserved instances do; uses pay-per-request pricing

# Features
## Tables
two classes: standard and IA

## Data types
DynamoDB is hybrid of document and key value pair noSQL database with the following mix of data types:
- Scalar types - String, Boolean, Binary, NULL, and Number
- Sets - which are typed, non-ordered arrays including string set, number set, and binary set.
- Document - list and map

### Partitions
Partitions hold 10GB (including LSI) and 3000 RCU &amp; 1000 WCU; when one of those limits is reached the data is then spread by partition key across many partitions. A single partition key is therefore limited to 10GB and 3000 RCU and 1000 WCU.

Figuring out the number of partitions is pretty easy:

numPartitions = MAX( RoundUp( Desired RCU / 3000 + Desired WCU / 1000), (Data / 10 GB)) 

Minimizing hot partitions is really an issue of proper provisioning and [partitian key design](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/). The important thing to do is to make sure the primary key is unique. 

### Keys and Indexes
Queries are restricted to the PK + sort key on the main table and indexes so defining keys and indexes is critical. There can be a total of 25 indexes per table - 20 global indexes &amp; 5 local 

Primary Key - Like all databases, a DynamoDB primary key is a unique identifier for a record in a table. The primary key uniquely identifies each item in the table, so that no two items can have the same key. The primary key can be the partition key or the primary key and sort key (composite)

Partition Keys - When DynamoDB stores data it uses the partition key to divide the table between partitions. The partition key should be a high-cardinality, or composite attribute. 

Sort Keys (range key) - The table can be sorted as well.. and it should be.

DynamoDB indexes are sparse so if a record does not have the value required to be indexed it will NOT appear in the index. 

### Local Secondary Indexes (LSI)
LSI use the table primary key but a different sort key with some attributes; consumes tables defined read capacity but for the size of the items in the projection NOT the size of the item in the table; created asynchronously at table creation ONLY.

If you scan or query for attributes NOT in the index projection that is bad... and results in a query to the index AND a query from the table. 

When you write a new item or update a NULL attribute projected into secondary index, that costs one write in the LSI. Change an indexed item? two LSI writes; delete an item? two LSI writes

### Global Secondary Indexes
GSI can use a different partition key and a different sort key and can be defined after table creation. A global secondary index is considered "global" because queries on the index can span all of the data in a table, across all partitions and are eventually consistent. 

Global indexes have completely different read/write capacity units that is calculated on the size of the projection NOT the underlying table. CRD operations consume write capacity. 

### Data access - Scans vs Queries
Queries - find items based on primary key attribute; optionally provide sort key and value; use a `ProjectionExpress` so the query only returns some of the attributes. Defaults to sorted Ascending by the sort key; use `ScanIndexForward` to false for Descending. 

Scan - examines every item in the table; avoid this. Supports eventually consistent and consistent reads. 1 Mg per scan max.

Overall, you want to avoid table scans therefore designing tables to use the `Query`, `Get` or `BatchGetItems` APIs. 

## DynamoDB Features

### Transactions
In the past DynamoDB had poor support for transactions but this has been improved to support ACID transactions.

### On-Demand Backup and Restore
Another new feature since last certification! Amazingly these full backups can be done at anytime without impact on performance or availability. Not cross region; backups not automatically deleted.

### Point-in-Time recovery (PITR)
Enables the ability to restore data at anypoint in the last 35 days and protects you against accidental writes or deletes. This is not enabled by default and the last restorable data is 5 minutes in the past.

### Global Tables
Global tables are a multi-master, multi-region replication solution to enable DR or HA. Requires streams to be active; replicates changes in less than one second.

### Encryption
Uses KMS keys for encryption of data at rest, can use VPN, DX to encrypt data in-flight; can also uses VPC endpoints.

### Time to Live (TTL)
The ability to expire a row of data after the TTL (in epoch time)

### Streams

#### DynamoDB Streams
Streams are ordered lists of record updates to a table that is stored for 24 hours with no duplicate entries in near real-time. Streams can be configured four ways:

- `KEYS_ONLY` - only the key attributes are written
- `NEW_IMAGE` - the entire post is written to the stream
- `OLD_IMAGE` - the entire pre update item is written to the stream
- `NEW_AND_OLD_IMAGES` - the pre and post item are written to the stream

There are two basic use cases for streams: replication and triggers. Cross region replication is enabled by a [CloudFormation template](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.CrossRegionRepl.html) that has been deprecated. 

#### Kinesis Streams 
Instead of routing the change to DynamoDB stream, now the stream can be routed to Kinesis directly. Kinesis has a longer data retention (365 days vs 24 hours in DynamoDB Streams) and better streams process tools (Kinesis Data Analytics &amp; Firehose )

### DAX
DAX is a caching layer that sits between an application and a DynamoDB table. It works for both reads and writes with a 5 minute TTL in cache. Has microsecond latency and can solve the hot key problem. Can be deployed Multi-AZ (recommended 3 nodes) with a max of 10 nodes. Integrates with the typical management services (IAM, VPC, KMS, CloudTrail). DAX is not serverless.

DAX doesn't work for generic caching needs; for instance, the query results can't be written to DAX but COULD be written to ElastiCache.

## Triage
- Record size < 400? DynamoDB
- Record Size > 400? Pointer to data in S3; OR vertically partition large data into separate table
- Pre-written App Tied to RDMBS? RDS
- Joins/Transactions? RDS
- BLOBS? store file metadata and pointer to data in S3
- Low I/O rate? better to store data in S3
- Write Heavy Partitions ( the [two candidates in voting app](https://youtu.be/bCW3lhsJKfw?t=31m25s) is a great example of this) - add random value to candidateID then create Lambda to aggregate
- Static Time Series Table - time series with no updates with a fixed "interest" TTL -> pipe data into hot table with lots of RCU and WCU; warm table with less RCU & WCU; cold table in glacier or S3 
- Dynamic Time Series Table - updates to TTL in a "delete after 30 days of last touch sort of scenario"; create write sharded GSI to query expired items; Lambda stored procedure to delete items; use streams to migrate data to cold storage; or use the new [TTL Timestamps](https://aws.amazon.com/blogs/aws/new-manage-dynamodb-items-using-time-to-live-ttl/) feature.
- Product Catalog Hot Item - cache hot items; pull through cache or move cache building to backend process using Lambda
- query filters vs composite key indexes - filtering only removes items from the found set AFTER they are read; instead, create a composite index (likely using a GSI) with the key and the filter attribute
- query for item that contains an attribute that does NOT exist in other items? Global secondary on attribute because of sparse indexes

## Conditional Updates vs Atomic counters
Because record locking isn't an option on a distributed database, app developers have to implement some sort of update concurrency model. There are two ways to approach this:

Conditional updates - When the system applies an update, the record is checked to make sure it has not changed since the record was read, and if it has not then it is updated. If the record has changed the update fails. This is an idempotent change so it can be made many times to see if it will work.

Atomic Counters - The other way to work this is to allows all write requests to be applied in the order they are received by incrementing or decrementing the attribute value. Challenging to figure out how this works. Overall, I'd say this is another case where you mark a record inactive, and insert it as a new record.

## Errors
All of these error return HTTP 400.
- `LimitExceededException` - There are too many concurrent control plane operations. The cumulative number of tables and indexes in the CREATING, DELETING or UPDATING state cannot exceed 10. You can create global and local secondary indexes at the same time you create a table, but you must wait for the first table with a secondary index to become active before creating the next one.

- `ProvisionedThroughputExceededException` - You exceeded your maximum allowed provisioned throughput for a table, partition, one or more global secondary indexes. With global secondary indexes, if one of the indexes runs low on write capacity, all of the tables indexes might get throttled, even if one or more of the indexes aren't affected. 

- `ItemCollectionSizeLimitExceededException` - When a single partition key (and LSI) are greater than 10GB 
