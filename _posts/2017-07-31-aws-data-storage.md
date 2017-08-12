---
layout: post
title: "AWS - Data Storage"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Demonstrate ability to make architectural trade off decisions involving storage options

S3 Parallelization, alphabetical order, randomness in URL


# Demonstrate ability to make architectural trade off decisions involving database options

Use RDS for ACID, joins, complex queries

Don't use RDS for index and query focused, BLOB

Use DynamoDB for indexed, query focused data, automated scalability

Don't use DynamoDB for prewritten apps, data larger than 400kb, BLOB, relational data

Use EC2 for DB2, Informix or Sybase

Use EC2 for complete control of DB


# Demonstrate ability to implement the most appropriate data storage architecture

EBS

# Determine use of synchronous versus asynchronous replication

Multi-AZ RDS = synchronous

RDS Read Replica = asynchronous

# Key Resources

- [AWS Storage Services Overview](https://d0.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf)