---
layout: post
title: "AWS - Scalablity"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}



# Demonstrate the ability to design a loosely coupled system

- SQS

- SNS -> SQS

- SNS Mobile Push

- Kinesis
  data producers
  shards
  Records - Sequence, partition key, data; 24 hours storage -> 7 days; Not persistent storage
  Consumers
  
# Demonstrate ability to implement the most appropriate front-end scaling architecture

CloudFront
  Web RTMP distros
  Geo Restrictions
  Custom URL = Dedicated IP SSL Cert or SNI Cert
  Supports all HTTP verbs; only caches GET, HEAD, GET, OPTIONS
  Alias naked domain names via Route53 to CloudFront
  Invalidation

# Demonstrate ability to implement the most appropriate middle-tier scaling architecture


# Demonstrate ability to implement the most appropriate data storage scaling architecture

Memecached = simple; horizontal scaling; multi-threaded; sharded

Redis = complex data; sort; presistence; multi-AZ; automatic fail-over; pub/sub; backup/restore

# Determine trade-offs between vertical and horizontal scaling

