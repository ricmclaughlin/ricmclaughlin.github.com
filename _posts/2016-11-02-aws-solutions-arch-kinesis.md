---
layout: post
title: "AWS Solutions Arch - Kinesis"
description: ""
category: posts
tags: [aws]
---
{% include JB/setup %}


## What is Kinesis
Real-time data processing, which is actually near-real time when you parse all the marketing, captures and stores large amounts of data to power dashboard and analytics.

And it is not just one app at time... there can be multiple incoming data streams working concurrently. And it is durable, with data being written to three AZ yet not long lived with data living 24 hours and up to 7 days. 

There are three components to the [Kinesis](https://aws.amazon.com/kinesis/) products:

- [Firehose](https://aws.amazon.com/kinesis/firehose/) - a data loader which can batch, compress and encrypt the data into S3, Redshift, Elasticsearch or Kinesis Analytics

- [Analytics](https://aws.amazon.com/kinesis/analytics/) - the ability to analyze a stream using SQL in an interactive tool including a SQL editor and templates

- [Streams](https://aws.amazon.com/kinesis/streams/) - the thing that firehose works on

As a complete side note, Kinesis is the best marketed product in the AWS line. 

### Details
A stream is made of up of shards and a shard is 1MB per second write and 2MB per second read capacity. There are tons of connectors, libraries and tools [available](https://aws.amazon.com/kinesis/streams/developer-resources/). 

## Why && When
In the past realtime process of massive amounts of data was hard... very hard in fact.  Kinesis is quite useful when you need to do multi-stage processing of data, partition the data then load the data. Tons of application in realtime gaming, IoT & mobile app analytics, monitoring app or system logs in real-time.

## Resources

[Introduction to Amazon Kinesis Firehose](https://qwiklabs.com/focuses/2988) 

[Building Real-Time Dashboards with Amazon Kinesis Dynamic Aggregators](https://qwiklabs.com/focuses/2596)

