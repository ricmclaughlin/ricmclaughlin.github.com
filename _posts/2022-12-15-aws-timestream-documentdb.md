---
layout: post
title: "AWS - Timestream & DocumentDB"
description: ""
category: posts
tags: [aws, analytics, databases, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Timestream
[Timestream](https://aws.amazon.com/timestream/) is a fast, scalable, and serverless time-series database service that makes it easier to store and analyze trillions of events per day up to 1,000 times faster. SQL compatible DB with scheduled queries to update aggregations; recent data is in-memory while historical data is kept on magnetic storage. Great use cases include: Kinesis Streams, AWS IoT, Prometheus, and Telegraf.

# DocumentDB
[DocumentDB](https://aws.amazon.com/documentdb/) (with MongoDB compatibility) is a fully managed native JSON document database that makes it easy and cost effective to operate critical document workloads at virtually any scale without managing infrastructure. 

DocumentDB is full managed, HA with replication across 3 AZ that grows in increments of 10GB and scales automatically. Like Aurora there is a primary node and replica nodes. 

## Pricing
It's similar to Aurora:
- On-demand instances (per second with a minimum of 10 minutes)
- Database I/O
- Database Storage
- Backup Storage
