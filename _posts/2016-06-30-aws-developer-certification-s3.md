---
layout: post
title: "AWS Developer Certification - S3"
description: ""
category: posts
tags: [aws, developercert, s3]
---
{% include JB/setup %}

Although the main topic of this post is S3, the overarching topic, the main, main topic is storage on AWS. Let's cover the "other" storage topics first then head on over and talk S3. Using a sequential prefix, such as time-stamp or an alphabetical sequence, increases the likelihood that Amazon S3 will target a specific partition for a large number of your keys, overwhelming the I/O capacity of the partition. If you introduce some randomness in your key name prefixes, the key names, and therefore the I/O load, will be distributed across more than one partition.

[glacier](https://aws.amazon.com/glacier/) - an object storage capability that is mostly offline and way, way cheaper than S3. It might take hours to retrieve an object from glacier so it is mostly suited towards stuff you need to store but will rarely, if ever, need to access.  

[EBS Volumes](https://aws.amazon.com/ebs/) - block level storage that can be logically attached to EC2 instances, must be in the same availablity zone as the EC2 instance; could be of magnetic, general purpose SSD and IOPS SSD class storage; encryptable; can not be shared between more than one EC2 instance 

Instance Store - physically attached block level storage for EC2 storage; for temp storage like cache and buffering; ephemeral 

Database Storage - [DynamoDB](https://aws.amazon.com/dynamodb/), which is basically MongoDB as a service, [RDS](https://aws.amazon.com/rds/), which could be one of many open source or other databases such as mySQL or its compatible cousin Aurora, PostgreSQL, Oracle or SQL Server and any databases that might run on an EC2 instance.

In-Memory caching - [ElastiCache](https://aws.amazon.com/elasticache/) (Memcached and Redis) or software on EC2 instances

[Redshift](https://aws.amazon.com/redshift/) - data warehouse service that is very cheap and quite function; the fastest [growing group at AWS](http://www.theregister.co.uk/2015/04/15/amazon_redshift_big_growth/) 

S3 Limits - the minimium size for a s3 object is 0 bytes; 100 S3 buckets per account

S3 lifecycle Policy - S3 bucket policies require a Principal be defined

S3 website hosting - To make a website on S3 at a minimium upload an index document to your S3 bucket, enable static website hosting in your S3 bucket properties, select the 'Make Public' permission for your bucket’s objects. The only allowed domain prefix when creating Route 53 Aliases for S3 static websites is the 'www' prefix.

Server side encryption can be enabled using the REST API using x-amz-server-side-encryption header.

Use Multipart upload to stop and resume uploads and ideally for any file over 100mg. A benefit of multi-part upload is that you can upload a file as it is being created.

Bucket names can not start with a '.' or '-' and cannot be formatted like an IP address. Remove public read access and use signed URLs with expiry dates to prevent read access from unauthorized sites.

