---
layout: post
title: "AWS Developer Certification - S3"
description: ""
category: posts
tags: [aws, developercert, s3]
---
{% include JB/setup %}

Although the main topic of this post is S3, the overarching topic, the main, main topic is storage on AWS. Let's cover the "other" storage topics first then head on over and talk S3.

[glacier](https://aws.amazon.com/glacier/) - an object storage capability that is mostly offline and way, way cheaper than S3. It might take hours to retrieve an object from glacier so it is mostly suited towards stuff you need to store but will rarely, if ever, need to access.  

[EBS Volumes](https://aws.amazon.com/ebs/) - block level storage that can be logically attached to EC2 instances, must be in the same availablity zone as the EC2 instance; could be of magnetic, general purpose SSD and IOPS SSD class storage; encryptable; can not be shared between more than one EC2 instance 

Instance Store - physically attached block level storage for EC2 storage; for temp storage like cache and buffering; ephemeral 

Database Storage - [DynamoDB](https://aws.amazon.com/dynamodb/), which is basically MongoDB as a service, [RDS](https://aws.amazon.com/rds/), which could be one of many open source or other databases such as mySQL or its compatible cousin Aurora, PostgreSQL, Oracle or SQL Server and any databases that might run on an EC2 instance.

In-Memory caching - [ElastiCache](https://aws.amazon.com/elasticache/) (Memcached and Redis) or software on EC2 instances

[Redshift](https://aws.amazon.com/redshift/) - data warehouse service that is very cheap and quite function; the fastest [growing group at AWS](http://www.theregister.co.uk/2015/04/15/amazon_redshift_big_growth/) 

S3 - 

S3 lifecycle Policy



