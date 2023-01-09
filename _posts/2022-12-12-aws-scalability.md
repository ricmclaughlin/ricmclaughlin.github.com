---
layout: post
title: "AWS - scalability"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Demonstrate the ability to design a loosely coupled system

- SQS

- SNS -> SQS

- SNS Mobile Push

- [Kinesis](/posts/aws-kinesis) - data producers; shards; Records - Sequence, partition key, data; 24 hours storage -> 7 days; Not persistent storage; Consumers
  
# Demonstrate ability to implement the most appropriate front-end scaling architecture

[CloudFront](/posts/aws-cloudfront) 
- Web or RTMP distros
- Geo Restrictions = white or black list
- Custom URL = Dedicated IP SSL Cert ($$$) or SNI Cert 
- Supports all HTTP verbs; only caches GET, HEAD, OPTIONS
- Alias naked domain names via Route53 to CloudFront
- Invalidation

# Demonstrate ability to implement the most appropriate middle-tier scaling architecture
- Elasticache - Memecached = simple; horizontal scaling; multi-threaded; sharded

- Elasticache - Redis = complex data; sort; persistence; multi-AZ; automatic fail-over; pub/sub; backup/restore

- SNS Mobile Push - push to phones with amaz-balls support for Windows Phone


