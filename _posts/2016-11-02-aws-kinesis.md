---
layout: post
title: "AWS - Kinesis"
description: ""
category: posts
tags: [aws, kinesis, aws-dev-ops-pro, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

## What is Kinesis

Real-time data processing that captures and stores large amounts of data to power dashboard and analytics.

And it is not just one app at time... there can be multiple incoming data streams working concurrently. And it is durable, with data being written to three AZ yet not long lived with data living 24 hours and up to 7 days. 

There are three components to the [Kinesis](https://aws.amazon.com/kinesis/) products:

- [Firehose](https://aws.amazon.com/kinesis/firehose/) - a data loader which can batch, compress and encrypt the data into S3, Redshift, Elasticsearch or Kinesis Analytics

- [Analytics](https://aws.amazon.com/kinesis/analytics/) - the ability to analyze a stream using SQL in an interactive tool including a SQL editor and templates

- [Streams](https://aws.amazon.com/kinesis/streams/) - the thing that firehose works on

As a complete side note, Kinesis is the best marketed product in the AWS line. 

### Details

A stream is made of up of shards and a shard is 1MB per second write and 2MB per second read capacity. There are tons of connectors, libraries and tools [available](https://aws.amazon.com/kinesis/streams/developer-resources/). 

The max size of a datablob is 1 MB.

You can calculate the initial number of shards (number_of_shards) that your stream will need by using the input values in the following formula:

number_of_shards = max(incoming_write_bandwidth_in_KB/1000, outgoing_read_bandwidth_in_KB/2000)

The number of partition keys should typically be much greater than the number of shards. This is because the partition key is used to determine how to map a data record to a particular shard. If you have enough partition keys, the data can be evenly distributed across the shards in a stream.

## Why && When

In the past realtime process of massive amounts of data was hard... very hard, in fact.  Kinesis is quite useful when you need to do multi-stage processing of data, partition the data then load the data. Tons of application in realtime gaming, IoT & mobile app analytics, monitoring app or system logs in real-time.

