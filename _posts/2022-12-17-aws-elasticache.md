---
layout: post
title: "AWS - ElastiCache"
description: ""
category: posts
tags: [database, aws-solutions-arch-pro]
---
{% include JB/setup %}

Caching is a complicated thing to implement well. Time-to-live, cache expiration and other issues make caching strategies difficult. 

The _Lazy Loading_ caching strategy is where the application looks at the cache first, either finds the data or goes to the source and then adds the record to the cache. If there is a node failure it's replacement will fill in due time so no big dealio.

The _Write Through_ caching strategy updates the cache when the data is written to the DB so data is never stale or need update. Keeping all nodes up to date, high latency because of multiple writes per record, and the storage of some data that might be infrequently accessed are all problems with this strategy. 

The easiest way to implement caching on AWS is [ElastiCache ](https://aws.amazon.com/elasticache/), a managed service the provides caching services for apps. There are two engines available from ElastiCache: Redis and memecached. Reserved instances are a great choice here; spot instances are not.

## memcached vs Redis

|                     | memecached   | Redis    |
|---------------------|--------------|----------|
| Use case complexity | low          | high     |
| Threading           | multi        | single   |
| Scaling             | *horizontal* | vertical |
| AZs                 | single       | *multi*  |
| replication?        | nope         | yep      |
| fail-over           | nope         | yep      |
| persist it          | nope         | yep      |
| pub-sub             | nope         | yep      |
| auto-discovery      | yep          | no       |

## memcached
multithreaded; and performs well up to 90% utilization then increase size of node or # of nodes

| **Metric**  | **Description**  | **Solution**  |
|:--------------------------------------------|:-----------------------------------------------------------|:----------------------| 
|CPU utilization | good up to 90% utilization | increase size of node or # of nodes |
| evictions | # records ejected from cache | larger instances and # of nodes |
| CurrConnections | # app to memcached connections | likely an application problem with no closing connections |
| SwapUsage | should be 0-50MB |  increase node size; increase ConnectionOverhead (decrease memory for caching data) |

## Redis
Single threaded; generally scale UP with larger instances by snap-shotting and increasing the instance size OR scale out with more READ replicas which use aysnc replication. Automatic and manual snapshots work like RDS with storage in S3; snap-shotting read replicas is a great idea because snap-shotting will degrade performance.

| **Metric**  | **Description**  |**Solution**  |
|:--------------------------------------------|:-----------------------------------------------------------|:----------------------|
| CPU utilization | threshold: 90 / # of CPU cores| read heavy: read replicas; write heavy: larger cache instance | 
| evictions | # records ejected from cache | more nodes |
| CurrConnections | # app to redis connections| likely an application problem with no closing connections |

## Monitoring
[What Elasticache Metrics to Monitor](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/CacheMetrics.WhichShouldIMonitor.html) is a giant question and largely based on which caching engine is in use. 

## Triage
- Simple use case, non-persistent data, horizontal scaling (shard), multi-threaded? memecached
- Complex? Redis
- Multi-AZ with failover? Redis
- HA? Redis
- Read replicas? Redis
- Backup and Restore? Redis

