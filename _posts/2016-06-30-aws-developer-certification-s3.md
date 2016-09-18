---
layout: post
title: "AWS Developer Certification - S3"
description: ""
category: posts
tags: [aws, developercert, s3]
---
{% include JB/setup %}

Although the main topic of this post is S3, the overarching topic, the main, main topic is storage on AWS. Let's cover the "other" storage topics first then head on over and talk S3. 

# Non-S3 Storage


[EBS Volumes](https://aws.amazon.com/ebs/) - block level storage that can be logically attached to EC2 instances, must be in the same availablity zone as the EC2 instance; could be of magnetic, general purpose SSD and IOPS SSD class storage; encryptable; can not be shared between more than one EC2 instance 

Instance Store - physically attached block level storage for EC2 storage; for temp storage like cache and buffering; ephemeral 

Database Storage - [DynamoDB](https://aws.amazon.com/dynamodb/), which is basically MongoDB as a service, [RDS](https://aws.amazon.com/rds/), which could be one of many open source or other databases such as mySQL or its compatible cousin Aurora, PostgreSQL, Oracle or SQL Server and any databases that might run on an EC2 instance.

In-Memory caching - [ElastiCache](https://aws.amazon.com/elasticache/) (Memcached and Redis) or software on EC2 instances

[Redshift](https://aws.amazon.com/redshift/) - data warehouse service that is very cheap and quite function; the fastest [growing group at AWS](http://www.theregister.co.uk/2015/04/15/amazon_redshift_big_growth/) 


# S3
S3 is an object, as in file, based storage and is a key-value based - the key is the file name and the data in the file which is the value in addition to other data like the Version ID, access control information and metadata. S3 offers *Read after Write* consistency for `PUTS` but **only** *Eventual* consistency for update `PUTS` or `DELETES`.

AWS charges you for storage, requests and data transfer.

## S3 Storage Classes
S3 - AWS guarantees 99.99% availability for the S3 Platform and 11 x 9 for durability.

S3-Infrequently Accessed - Less frequently used data that needs rapid access - there is a charge a retrieval fee

S3-Reduced Redundancy Storage - 99.99% durability - for files that can be regen'd.

[glacier](https://aws.amazon.com/glacier/) - an object storage capability that is mostly offline and way, way cheaper than S3. It might take hours (like 3-5 hours) to retrieve an object from glacier so it is mostly suited towards stuff you need to store but will rarely, if ever, need to access.  

## Versioning
Stores all version of an object; can be suspended but not disabled for bucket; can be used with MFA to provide extra layer of DELETE security. Cross region replication requires versioning and an IAM role setup. ALL version of the file are stored so the amount of spaced required can end up being HUGE.

## Bucket Names &amp; Such
Bucket names can not start with a '.' or '-' and cannot be formatted like an IP address. Remove public read access and use signed URLs with expiry dates to prevent read access from unauthorized sites. 

Files must be stored in buckets and buckets are a universal namespace.

Using a sequential prefix, such as time-stamp or an alphabetical sequence, increases the likelihood that Amazon S3 will target a specific partition for a large number of your keys, overwhelming the I/O capacity of the partition. If you introduce some randomness in your key name prefixes, the key names, and therefore the I/O load, will be distributed across more than one partition.

S3 Limits - the minimium size for a s3 object is 0 bytes; 100 S3 buckets per account

S3 lifecycle Policy - S3 bucket policies require a Principal be defined

Server side encryption can be enabled using the REST API using x-amz-server-side-encryption header.

Use Multipart upload to stop and resume uploads and ideally for any file over 100mg. A benefit of multi-part upload is that you can upload a file as it is being created.

## S3 Website Hosting
To make a website on S3 at a minimium upload an index document to your S3 bucket, enable static website hosting in your S3 bucket properties, select the 'Make Public' permission for your bucket’s objects or apply a bucket permission script. The only allowed domain prefix when creating Route 53 Aliases for S3 static websites is the 'www' prefix. S3 buckets do not have https!

CORS rules - The bucket hosting the assets needs CORS configuration - add the domain of the "static" site to fix this up.

# Resources
## Qwik Labs
[Introduction to Amazon Simple Storage Service (S3)](https://qwiklabs.com/focuses/2355)
[Introduction to AWS Key Management Service](https://qwiklabs.com/focuses/2367) - yeah, this is about Key Management Service but under-the-covers this is about S3.

## Reading

## Videos
[AWS S3 Deep Drive and Best Practices](https://www.youtube.com/watch?v=1TvJCLl9NNg)
