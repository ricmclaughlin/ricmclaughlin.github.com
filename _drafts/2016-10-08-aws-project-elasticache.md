---
layout: post
title: "AWS Project ElastiCache"
description: ""
category: 
tags: []
---
{% include JB/setup %}


There are two engines available from ElastiCache: Redis and memecached

memcached
multi threaded; good up to 90% utilization then increase size of node or # of nodes

Redis 
Single threaded; scale out with more read replicas



You have enabled a CloudWatch metric on your Redis ElastiCache cluster. Your alarm is
triggered due to an increased amount of evictions. How might you go about solving the increased
eviction errors from the ElastiCache cluster?


https://aws.amazon.com/elasticache/

http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/CacheMetrics.WhichShouldIMonitor.html