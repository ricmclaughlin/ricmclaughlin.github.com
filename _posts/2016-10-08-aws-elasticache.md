---
layout: post
title: "AWS - ElastiCache"
description: ""
category: posts
tags: [aws, elasticache, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

[ElastiCache ](https://aws.amazon.com/elasticache/) is a managed service the provides caching services for apps. There are two engines available from ElastiCache: Redis and memecached. The signifigant differences between memcached and redis make for differences in monitoring and scaling.

ElastiCache only backs up Redis clusters. Snapshots backup the data for the entire cluster at a specific time and probably cause a performance degradation. Try to backup read replicas! Multi-AZ friendly.

| Thingy | memecached | Reddis |
|--------|-----------|--------|
| Use case complexity | low | high  |
| Threading | multi  | single |
| Scaling | horizontal | vertical |
| AZs  | single | multi  |
| replication? | nope | yep |
| fail-over | nope | yep |
| persist it | nope | yep |
| pub-sub | nope | yep |

## Monitoring

[What Elasticache Metrics to Monitor](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/CacheMetrics.WhichShouldIMonitor.html) is a giant question and largely based on which caching engine is in use. 

## memcached

multithreaded; and performs well up to 90% utilization then increase size of node or # of nodes

| **Metric**  | **Description**  |**Solution**  |
|:-----------------------------------------|:--------------------------------------------------------|:----------------------| 
|CPU utilization | good up to 90% utilization | increase size of node or # of nodes |
| evictions | # records ejected from cache | larger instances and # of nodes |
| CurrConnections | # app to memcached connections | likely an application problem with no closing connections |
| SwapUsage | should be 0-50MB |  increase node size; increase ConnectionOverhead (decrease memory for caching data) |

## Redis

Single threaded; generally scale UP with larger instances OR scale out with more READ replicas

| **Metric**  | **Description**  |**Solution**  |
|:-----------------------------------------|:--------------------------------------------------------|:----------------------| 
| CPU utilization | threshold: 90 / # of CPU cores| read heavy: read replicas; write heavy: larger cache instance | 
| evictions | # records ejected from cache | more nodes |
| CurrConnections | # app to memcached connections| likely an application problem with no closing connections |

## Simple Question

You have enabled a CloudWatch metric on your Redis ElastiCache cluster. Your alarm is triggered due to an increased amount of evictions. How might you go about solving the increased eviction errors from the ElastiCache cluster?


## Triage

- Simple use case, horizontal scaling (shard), multi-threaded? Memecache

- Complex? Redis

