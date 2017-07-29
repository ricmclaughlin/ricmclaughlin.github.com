---
layout: post
title: "AWS - DynamoDB"
description: ""
category: posts
tags: [dynamodb, aws, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}


# DyanamoDB Overall

Yes, [DynamoDB](https://aws.amazon.com/dynamodb/) is like MongoDB - but the concepts behind MongoDB have better names. Reads of a DynamoDB table, unless you specify otherwise, are eventually consistent. DynamoDB uses optimistic concurrency control. It stores data in three separate data centers - not AZs. 

**DynamoDB limits** - DynamoDB limits the tables per region to 256 but this can be increased by contacting AWS. The maximum size of a DynamoDB range primary key attribute value is 1024 bytes. If you need more than 10,000 writes/second or 10,000 reads/second you need to contact AWS directly. 


## Data types

DynamoDB is hybrid of document and key value pair noSQL database types and features the following mix of data types:

- Scalar types = String, Boolean, Binary, and Number

- Sets - which are typed, non-ordered arrays including string set, number set, and binary set.

- Document - basically json, much like how MongoDB store data

## Pricing 

Pricing with DyanamoDB is difficult to project. Instead of being priced on just disk storage, pricing is based on disk storage, provisioned throughput, DynamoDB streams and data transfer.

## Keys and Indexes

Primary Key - Like all databases, a DynamoDB primary key is a unique identifier for a record in a table. The primary key can be simple (use the partition key) or composite (the primary key and sort key)

Partition Keys (hash key) - When DynamoDB stores data it uses the partition key to divide the table between partitions. The partitian key should be a high-cardinality, or composite attribute. 

Sort Keys (range key) - The table can be sorted as well.. and it should be.

DynamoDB indexes are sparse so if a record does not have the value required to be indexed it will NOT appear in the index. 

10 indexes per table = 5 global indexes + 5 local 

### Local Secondary Indexes

Secondary Local Indexes are sparse indexes including the table primary key & sort key, a different sort key and any additional projected attributes; consumes tables defined read capacity but for the size of the items in the projection NOT the size of the item in the table; created asynchronously. 

If you scan or query for attributes NOT in the index project that is bad... and results in a query to the index AND a query from the table. 

When you write a new item or update a NULL attribute projected into secondary index, that costs one write in the LSI. Change an indexed item? two LSI writes; delete an item? two LSI writes

### Global Secondary Indexes

Global Secondary Indexes can use a different partition key and a different sort key (the partition and sort key *can* be different from those on the table.) - A global secondary index is considered "global" because queries on the index can span all of the data in a table, across all partitions and are eventually consistent. 

Global indexes have completely different read/write capacity units that is calculated on the size of the projection NOT the underlying table. CRD operations consume write capacity.


## RCU &amp; WCU 

**Eventually Consistent Reads** - By default, once there is a DynamoDB write you can access data with a second or so it is eventually consistent. 

**Strongly Consistent Read** - a strongly consistent read reflects all the writes that have been received a successful response prior to the read. 

### Read capacity unit estimation 

A unit of read capacity is 1 strongly consistent reads per second or 2 eventually consistent reads per second for items up to 4KB. Calculation algorithm:

1. Round read size up to nearest multiple of 4

1. divide by 4

1. Times reads per second

1. if eventual consistent divide by 2

#### Example read capacity problem #1

Strongly consistent reads; 3KB item size; 80 reads per sec;

Reads per Item = 1 (3KB rounds up to 4 KB /4)

Reads per Second = 80

Read Capacity Units = 1 * 80 = 80

#### Example read capacity problem #2

Weakly consistent reads; 6KB item size; 120 reads per sec;

Reads per Item = 2 (6KB rounds up to 8 KB /4)

Reads per Second = 120

Read Capacity Units = 2 * 120 = 240 / 2 = 120

## Write capacity unit estimation

Formula -> writes per item (size in KB rounded up to the nearest whole number) * writes per second

## Conditional Updates vs Atomic counters

Because record locking isn't an option on a distributed database, app developers have to implement some sort of update concurrency model. There are two ways to approach this:

Conditional updates - When the system applies an update, the record is checked to make sure it has not changed since the record was read, and if it has not then it is updated. If the record has changed the update fails. This is an idempodent change so it can be made many times to see if it will work.

Atomic Counters - The other way to work this is to allows all write requests to be applied in the order they are received by incrementing or decrementing the attribute value. Challenging to figure out how this works. Overall, I'd say this is another case where you mark a record inactive, and insert it as a new record.

## Scans vs Queries

Queries - find items based on primary key attribute; optionally provide sort key and value; use a `ProjectionExpress` so the query only returns some of the attributes. Defaults to sorted Ascending by the sort key; use `ScanIndexForward` to false for Descending. 

Scan - examines every item in the table; avoid this. Supports eventually consistent and consistent reads. 1 Mg per scan max.

Scan vs Query - A query result is an eventually consistent read but you can request it to be a strongly consistent read.

Overall, you want to avoid table scans therefore designing tables to use the `Query`, `Get` or `BatchGetItems` APIs. 

## DynamoDB Components

Dynamo Logs - It's a database log that emits events. 

### Partitions

Partitions hold 10GB (including LSI) and 3000 RCU &amp; 1000 WCU; when one of those limits is reached the data is then spread by partitian key across many partitians. A single partitian key is therefore limited to 10GB and 3000 RCU and 1000 WCU.

Figuring out the number of partitians is pretty easy:

NumPartitians = MAX( RoundUp( Desired RCU / 3000 + Desired WCU / 1000), (Data / 10 GB)) 

Minimizing hot partitians is really an issue of proper provisioning and [partitian key design](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/). The important thing to do is to make sure the primary key is unique. 

### Streams

Streams are ordered lists of record updates to a table that is stored for 24 hours with no duplicate entries in near real-time. Streams can be configured four ways:

- `KEYS_ONLY` - only the key attributes are written

- `NEW_IMAGE` - the entire post is written to the stream

- `OLD_IMAGE` - the entire pre update item is written to the stream

- `NEW_AND_OLD_IMAGES` - the pre and post item are written to the stream

There are two basic use cases for streams: replication and triggers. Cross region replication is enabled by a [CloudFormation template](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.CrossRegionRepl.html) that has been deprecated. 

## Use cases

- Record size < 400? DynamoDB

- Record Size > 400? Pointer to data in S3; OR vertically partition large data into separate table

- Write Heavy Partitions ( the [two candidates in voting app](https://youtu.be/bCW3lhsJKfw?t=31m25s) is a great example of this) - add random value to candidateID then create lambda to aggregate

- Static Time Series Table - time series with no updates with a fixed "interest" TTL -> pipe data into hot table with lots of RCU and WCU; warm table with less RCU & WCU; cold table in glacier or S3 

- Dynamic Time Series Table - updates to TTL in a "delete after 30 days of last touch sort of scenario"; create write sharded GSI to query expired items; Lambda stored procedure to delete items; use streams to migrate data to cold storage; or use the new [TTL Timestamps](https://aws.amazon.com/blogs/aws/new-manage-dynamodb-items-using-time-to-live-ttl/) feature.

- Product Catalog Hot Item - cache hot items; pull through cache or move cache building to backend process using Lambda

- Transactions - Manage versioning across items with metadata by tagging attributes with multiple versions; like ACID code app to deal with updates in progress, error handling, recovery logic

- query filters vs composite key indexes - filtering only removes items from the found set AFTER they are read; instead, create a composite index (likely using a GSI) with the key and the filter attribute

- query for item that contains an attribute that does NOT exist in other items? Global secondary on attribute because of sparse indexes

## Error

All of these error return HTTP 400.

- `LimitExceededException` - There are too many concurrent control plane operations. The cumulative number of tables and indexes in the CREATING, DELETING or UPDATING state cannot exceed 10. You can create global and local secondary indexes at the same time you create a table, but you must wait for the first table with a secondary index to become active before creating the next one.

- `ProvisionedThroughputExceededException` - You exceeded your maximum allowed provisioned throughput for a table, partician, one or more global secondary indexes. With global secondary indexes, if one of the indexes runs low on write capacity, all of the tables indexes might get throttled, even if one or more of the indexes aren't affected. 

- `ItemCollectionSizeLimitExceededException` - When a single partitian key (and LSI) are greater than 10GB 
