---
layout: post
title: "AWS Developer Certification - DynamoDB"
description: ""
category: posts
tags: [aws, developercert, mongodb, dynamodb]
---
{% include JB/setup %}

The [AWS Developer Certification - Associate]() has a some stuff about databases in general.

## Databases in General
Online Transaction processing (OLTP) databases - Database systems like MySQL, PostgreSQL, Oracle, Aurora and MariaDB handle transactions... and transactions are important. Just think about doing an ATM deposit transaction... you want the your money to get credited to your account or fail completely. You want to know for sure. When this is the case you want an OLTP database. In addition, these systems tend to use a normalized data structure.

Online Analytics Processing (OLAP) databases - Databases systems enable data analysis capabilities WITHOUT transactions and don't use a normalized data structure. In general, these systems pull and transform data from an OLTP system and then do reporting. Often called a data warehouse.

Database Caching - AWS offers Elasticache which provides an in memory caching layer using either the open source Memcached or Redis engines.

## DyanamoDB Overall
Here are a couple of notes on what I found important to know about DynamoDB.

Yes, DynamoDB is like MongoDB - but the concepts behind MongoDB have better names. Reads of a DynamoDB table, unless you specify otherwise are eventually consistent. DynamoDB uses optimistic concurrency control.

### Pricing 

Pricing with DyanamoDB is difficult. Instead of being priced on just disk storage, 

### Parition Keys and Indexes

Partition Keys - When DynamoDB stores data it uses the partition key to divide the table between partitions/servers. The primary key of the table can be the partition key only, which makes it a simple key, or a composite key using the partition key and the sort key. 

Secondary Indexes - total of 5 global indexes and 5 secondary indexes for a total of 10 per table

* secondary local indexes - same partition key different sort key( has the same hash key as the table, but a different range key); consumes tables defined read capacity; can only be created WITH the table not after the table has been created.

* global indexes - uses a different partition key and a different sort key (an index with a hash and range key that can be different from those on the table.) - A global secondary index is considered "global" because queries on the index can span all of the data in a table, across all partitions. Global indexes have completely different read/write capacity units.

### Read capacity unit estimation 
one strongly consistent read per second and two eventually consistent read per second up to 4KB
Formula -> Reads per item (size in KB rounded up to nearest mulitple of 4 /4) * (read factor * read per sec) = total read capacity; the read factor for strongly consistent = 1; read factor for eventually consistent read = .5

#### Example read capacity problem #1

Strongly consistent reads; 3KB item size; 80 reads per sec;

Reads per Item = 1 (3KB rounds up to 4 KB /4)

Read Factor = 1 (strongly consistent)

Reads per Second = 80

Read Capacity Units = 1 * 1 * 80 = 80

#### Example read capacity problem #2

Weakly consistent reads; 6KB item size; 120 reads per sec;

Reads per Item = 2 (6KB rounds up to 8 KB /4)

Read Factor = .5 (strongly consistent)

Reads per Second = 120

Read Capacity Units = 2 * .5 * 120 = 120

### Write capacity unit estimation
Formula -> writes per item (size in KB rounded up to the nearest whole number) * writes per second
write capacity unit - on write per second up to 1KB

Scan vs Query - A query result is an eventually consistent read but you can request it to be a strongly consistent read.

Atomic Counters - allows all write requests to be applied in the order they are received by incrementing or decrementing the attribute value.

DynamoDB limits - DynamoDB limits the tables per region to 256 but this can be increased by contacting AWS. The maximum size of a DynamoDB range primary key attribute value is 1024 bytes.




