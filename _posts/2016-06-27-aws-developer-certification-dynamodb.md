---
layout: post
title: "AWS Developer Certification - DynamoDB"
description: ""
category: posts
tags: [aws, developercert, mongodb, dynamodb]
---
{% include JB/setup %}

The [AWS Developer Certification - Associate]() has a some stuff about databases in general.

# DyanamoDB Overall
Here are a couple of notes on what I found important to know about DynamoDB.

Yes, DynamoDB is like MongoDB - but the concepts behind MongoDB have better names. Reads of a DynamoDB table, unless you specify otherwise are eventually consistent. DynamoDB uses optimistic concurrency control. It stores data in three separate data centers - not AZs. Not sure why there is a distinction.

Eventually Consistent Reads - By default, once there is a DynamoDB write you can access data with a second or so - it is eventually consistent. 

Strongly Consistent Read - a strongly consistent read reflects all the writes that have been received a successful response prior to the read. 


DynamoDB limits - DynamoDB limits the tables per region to 256 but this can be increased by contacting AWS. The maximum size of a DynamoDB range primary key attribute value is 1024 bytes.

## Conditional updates vs Atomic counters
Because record locking isn't an option on a distributed database, app developers have to implement some sort of update concurrency model. There are two ways to approach this:

Conditional updates - When the system applies an update, the record is checked to make sure it has not changed since the record was read, and if it has not then it is updated. If the record has changed the update fails. This is an idempodent change so it can be made many times to see if it will work.

Atomic Counters - The other way to work this is to allows all write requests to be applied in the order they are received by incrementing or decrementing the attribute value. Challenging to figure out how this works. Overall, I'd say this is another case where you mark a record inactive, and insert it as a new record.


## Pricing 

Pricing with DyanamoDB is difficult. Instead of being priced on just disk storage, 

## Keys and Indexes
Primary Key  - Like all databases a DynamoDB primary key is a unique identifier for a record in a table. The primary key can be simple (use the partition key) or composite (the primary key and sort key)

Partition Keys (hash key) - When DynamoDB stores data it uses the partition key to divide the table between partitions/servers. 

Sort Keys (range key) - 

Secondary Indexes - total of 5 global indexes and 5 secondary indexes for a total of 10 per table

* Secondary Local Indexes - same partition key with different sort key defined at table creation ONLY( has the same hash key as the table, but a different range key); consumes tables defined read capacity.

* Global Secondary Indexes - uses a different partition key and a different sort key (the partition and sort key ~can~ be different from those on the table.) - A global secondary index is considered "global" because queries on the index can span all of the data in a table, across all partitions. Global indexes have completely different read/write capacity units.

## Read capacity unit estimation 
Formula -> (size per item KB rounded up to nearest mulitple of 4 /4) * read per sec = strongly consistent read; divide by 2 if eventually consistent.

If you exceed provisioned throughput you receive a 400 HTTP status code - `ProvisionedThroughputExceededException`

### Example read capacity problem #1

Strongly consistent reads; 3KB item size; 80 reads per sec;

Reads per Item = 1 (3KB rounds up to 4 KB /4)

Reads per Second = 80

Read Capacity Units = 1 * 80 = 80

### Example read capacity problem #2

Weakly consistent reads; 6KB item size; 120 reads per sec;

Reads per Item = 2 (6KB rounds up to 8 KB /4)

Reads per Second = 120

Read Capacity Units = 2 * 120 = 240 / 2 = 120

## Write capacity unit estimation
Formula -> writes per item (size in KB rounded up to the nearest whole number) * writes per second
write capacity unit - on write per second up to 1KB

## DynamoDB Streams
It's a database log that emits events.

## Scans vs Queries
Overall, you want to avoid table scans therefore designing tables to use the `Query`, `Get` or `BatchGetItems` APIs. Scan vs Query - A query result is an eventually consistent read but you can request it to be a strongly consistent read.

Queries - find items based on primary key attribute; optionally provide sort key and value; use a `ProjectionExpress` so the query only returns some of the attributes. Defaults to sorted Ascending by the sort key; use `ScanIndexForward` to false for Descending.

Scan - examines every item in the table; avoid this.

## Web Identity Provider Access to DynamoDB
This is a huge issue because many times users are authenticated by an ID provider then need access to Dynamo. Steps:

1. Authenticate with ID provider 

2. Receive Token from ID provider

3. App calls `AssumeRoleWithWebIdentity` passes in provider toek and ARN for IAM role.

4. STS provides DynamoDB access for a period of 15 minutes to 1 hours. 1 hour is the default.

# Databases in General
Online Transaction processing (OLTP) databases - Database systems like MySQL, PostgreSQL, Oracle, Aurora and MariaDB handle transactions... and transactions are important. Just think about doing an ATM deposit transaction... you want the your money to get credited to your account or fail completely. You want to know for sure. When this is the case you want an OLTP database. In addition, these systems tend to use a normalized data structure.

Online Analytics Processing (OLAP) databases - Databases systems enable data analysis capabilities WITHOUT transactions and don't use a normalized data structure. In general, these systems pull and transform data from an OLTP system and then do reporting. Often called a data warehouse.

Database Caching - AWS offers Elasticache which provides an in memory caching layer using either the open source Memcached or Redis engines.

# Resources
## Qwik Labs
* [Introduction to Amazon DynamoDB](https://qwiklabs.com/focuses/2376)
* [Working with Amazon DynamoDB](https://qwiklabs.com/focuses/2865)

## Reading
[DynamoDB - How it Works](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.html)

## Videos
[Deep Dive: Amazon DynamoDB](https://www.youtube.com/watch?v=VuKu23oZp9Q)

