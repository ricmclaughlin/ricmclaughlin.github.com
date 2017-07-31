---
layout: post
title: "AWS - Kinesis CLI Exploration"
description: ""
category: projects
tags: [aws, kinesis, aws-dev-ops-pro, aws-solutions-arch-pro, cli-exploration]
---
{% include JB/setup %}

This is a very simple exploration of [AWS Kinesis Streams](https://aws.amazon.com/kinesis/streams/) from the [AWS CLI](https://aws.amazon.com/cli/). To get started with the CLI first [install it](http://docs.aws.amazon.com/cli/latest/userguide/installing.html), then use `aws configure` to [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for your account and region. 

In this single scenario, follow along to create, write, and read from a stream. 

# Create, Write, and Read Streams 

#### Create stream

```bash
aws kinesis create-stream --stream-name mySillyStream --shard-count 1
```

#### List the streams to see if it worked

```bash
aws kinesis list-streams
```

#### what sort of data is in the stream description? (table output format rocks)

```bash
aws kinesis describe-stream --stream-name mySillyStream
```

####  add a little data

```bash
aws kinesis put-record --stream-name mySillyStream --partition-key 123 --data testdata
```

####  get an iterator 

```bash
aws kinesis get-shard-iterator --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON --stream-
name mySillyStream
```

####  use the iterator to read the stream

```bash
aws kinesis get-records --shard-iterator <insert the long shard iterator here>
```

####  enough of this - delete the stream

```bash
aws kinesis delete-stream --stream-name mySillyStream
```