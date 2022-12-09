---
layout: post
title: "AWS - Kinesis Streams"
description: ""
category: posts
tags: [aws, kinesis, data-collection, aws-data-analytics aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

[Kinesis](https://aws.amazon.com/kinesis/) is a real-time data processing service that captures and stores large amounts of data to power dashboard and analytics. It's the AWS alternative to [Apache Kafka](https://kafka.apache.org/).

In the past realtime process of massive amounts of data was hard... very hard, in fact. Kinesis is quite useful when you need to do multi-stage processing of data, partition the data, and then load the data. Tons of application in realtime gaming, IoT & mobile app analytics, or monitoring apps and system logs in real-time.

And it is not just one app at time... there can be multiple incoming data streams working concurrently. And it is durable, with data being written to three AZ. Data is ummutable persisted for 24 hours by default and up to ~~7~~ 365 days and can be replayed and reprocessed.

There are three components to the [Kinesis](https://aws.amazon.com/kinesis/) products:

- [Firehose](https://aws.amazon.com/kinesis/firehose/) - a data loader which can batch, compress, and encrypt the data into S3, Redshift, ElasticSearch, [Splunk](https://www.splunk.com/) or Kinesis Analytics

- [Analytics](https://aws.amazon.com/kinesis/analytics/) - the ability to analyze a stream using SQL in an interactive tool including a SQL editor and templates

- [Streams](https://aws.amazon.com/kinesis/streams/) - ingestion at low latency

As a complete side note, Kinesis is the best marketed product in the AWS line. 

## Kinesis Components
There are tons of connectors, libraries, and tools [available](https://aws.amazon.com/kinesis/streams/developer-resources/). 


## Data Producers 

In addition to third party libraries (Spark, Log4J, Appenders, Flume, Kafka connect, etc) and AWS "Managed" sources (CloudWatch Logs, AWS IoT, Kinesis Data Analytics there are three data producers that can add records to a stream:

### Kinesis Streams API (SDK) 
Using the `PutRecord` or `PutRecords` calls; `PutRecords` is batched therefore higher throughput

### Kinesis Producer Library (KPL) 
KPL is for developing resusable producers and includes configurable retry mechanism, sync or async, and kicks off CloudWatch metrics. It does not support compression; compression is a roll-your-own sort of thing. KPL requires the use of the Kinesis Consumer Library or a helper library.

KPL includes batching to increase throughput and decrease cost. *Collect* enables the ability to record across multiple shards in the same `PutRecords` call. *Aggregate* stores multiple records in one record by increasing payload size but increases latency. 

### Kinesis Agent 
The Kinesis agent is a java app that sits on top of KPL for Linux devices. It can process multiple logs and write to multiple streams. It handles file rotation, checkpointing, and failure retries.

### Data Producer Triage

Simple, low throughput? SDK

High performance, long running? KPL

Super low latency? SDK (KPL is limited by the `RecordMaxBufferedTime` latency)

Decrease latency using KPL? decrease `RecordMaxBufferedTime` (default is 100ms)

`ProvisionedThroughputException`? likely a hot shard; solve using better partition key, increase shards, and do retries with backoff (this is best practice to begin with...)

## Streams &amp; Shards

A stream is made of up of shards and a shard is 1MB per second write and 2MB per second read capacity. 

The max size of a datablob is 1 MB.

You can calculate the initial number of shards (number_of_shards) that your stream will need by using the input values in the following formula:

number_of_shards = max(incoming_write_bandwidth_in_KB/1000, outgoing_read_bandwidth_in_KB/2000)

The number of partition keys should typically be much greater than the number of shards. This is because the partition key is used to determine how to map a data record to a particular shard. If you have enough partition keys, the data can be evenly distributed across the shards in a stream.

### Adding & Splitting Shards

Adding shards, also called "Shard Splitting", is when you need to increase the stream capacity or divide a hot shard. You can merge shards when there are two shards with low traffic and you want to reduce costs. The old shards are closed and deleted once the data is expired. It is possible for split shards to receive data out of order IF the client reads from the child shard and there is still data in the parent shard; make sure that all data from parent is retrieved before reading from child!

There is not an autoscaling function for Kinesis but can be implemented using Lambda. Resharding can only be done serially and takes a few second; there are signifigant limits on scaling past 500 shards OR doubling/halving the number of streams in a 24 hour period of time.

### Records

Records include a record key (partitian/shard key), a sequence number (assigned after ingestion) and up to a one mg blob of data. Partitian keys are assigned to records prior to write into the stream while sequence keys are generated by Kinesis after the client and are unrelated to the partitian key. Records are ordered per shard.

Producer retries, typically caused by networking timeouts, can create duplicate writes which manifests as duplicate records. The easy fix for this is to embed a unique ID in the payload and de-dupe on the client side.

## Data Consumers
Consumers can interact with a stream in two ways:

- Consumer Classic - which enables 2 MB/s per shard across all consumers & 5 API calls per second per shard (pull model). 

- Consumer Enhanced Fan-Out - which enables 2 MB/s per shard per enhanced consumer with no API calls (push model) As of Aug 2018, KCL 2.0 and Lambda support a push model via the `SubscribeToShard` function enabling > 1 consumers to get 2 MB/s per shard over HTTP/2 with reduced latency of ~70ms. Supports a soft limit of 5 consumers and costs more.

In addition to third party libraries (Spark, Log4J, Appenders, Flume, Kafka connect, etc) there are several data producers that can consume records from a stream:

### Kinesis SDK - (Classic/Pull) 

The SDK polls the shard using `GetRecords`; this can return up to 10 MB (then be throttled for 5 seconds) or up to 10K records. There is a max of 5 `GetRecords` calls per second; this limits throughput signfigantly and leads to a rule: max 1 consumer per

### Kinesis Client Library (KCL)
Required if you use KPL because it does de-aggregation; enables a clients to share multiple shards using a "group" and shard discovery. Includes checkpoint functionality with one DynamoDB row used for coordination & checkpointing per shard; this requires enough DynamoDB write and read capacity units. On-Demand DynamoDB scaling (for WCU & RCU) is supported! The `ExpiredIteratorException` suggests you need to increase WCU OR turn on On-Demand DynamoDB scaling. V 2.0 supports consumer advanced Fan-Out.

### Kinesis Connector Library
This is a legacy, older Java library that writes data to S3, DynamoDB, Redshift, and ElasticSearch. It's largely deprecated because this functionality is now in Kinesis Firehose or can be addressed with Lambda.

### AWS Lambda
Lambda is great for lightweight ETL into S3, DynamoDB, Redshift, ElasticSearch or anything you can interface with Lambda. Lambda does support a de-aggregation library for use with streams produced by KPL. Supports Consumer Enhanced Fan-Out.

## Kinesis Firehouse

## Kinesis Video Streams

#### Triage

1-3 consumers and > ~200ms latency OR low cost? KCL standard

greater than 3 consumers and < 70ms latency? Enhanced fan-out

Duplicate records? Add unique ID to record payload.






