---
layout: post
title: "AWS - Data Storage"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Demonstrate ability to make architectural trade off decisions involving storage options

[S3](/posts/aws-data-storage) - Parallelization, alphabetical order, randomness in URL -> SSHHDDMMYY

## S3

Good for: Web Storage, Static websites, Data Lake, backup/archive

Bad For: File, System, Structured data, archive (use Glacier class), dynamic websites

### S3 Storage Classes

Rebuildable asset? S3-SSR (because you can regenerate it!)

Infrequently access (Backup & Archive or DR yet still active)? S3-IA

Greater than 4 hours retrieval, cheap (archive, digital preservation)? Glacier

### Use Cases

Change S3 access permissions? bucket policy

Protect S3 file from delete or overwrite? enable versioning

Protect S3 bucket deletion? backup bucket to another account

Only certain folks can delete and overwrite? enable MFA delete

Only certain folks can change state of versioning? enable MFA

S3 Cross region replication? Requires versioning on source bucket; deletes and lifecycle actions not supported

Partner upload data? [Pre-signed upload URLs](http://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html)

#### Performance Use Cases

Faster download of large objects? [range based `GET`](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) or use CloudFront

[High TPS workload](http://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html#get-workload-considerations)? prefix the key name with a hash or reverse timestamp string

Slow file upload speed? [S3 Transfer acceleration](http://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html) or [multi-part uploads](http://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html)

`PUT` optimization on a strong network? multi-part uploads of between 25-50MB to max throughput

`PUT` optimization on a weak network? multi-part uploads of about 10MB to prevent upload restart

## Glacier

Good for: Archive

Bad for: Immediate access, rapidly changing data

## EFS

Good for: Shared File storage for EC2 multi-threaded

Bad for: Archiving, temp storage, relational data

Use Cases: >7000 file operations per sec? Max I/O else General Purpose (burstable)

## EBS

Good for: EC2 Block Storage with snapshots

Bad for: temp storage (use instance store), multi-instance (use EFS), durable (use EFS/S3), Web content (S3)

## EC2 Instance Store

Good for: High I/O, temp storage

Bad for: persistant storage (s3 or EBS/EFS), RDMBS (EBS), shared storage (EFS), snapshots

Use Case: 

- NoSQL? I2 (365K IOPS)

- MPP/Hadoop/EMR? D2 3.5 Gib/s; Might prewarm to get max performance

- Backup instance store? no snapshotting, file backup on mounted EFS/EBS 

## Storage Gateway/Snowball

Use Cases:

- Low end DR = S3 directly

- Cached = less to maintain local so cheaper outlay, frequently access to small amount of data

- Stored = low latency for ALL data with EBS Snapshots; snapshots can be used as a migration tool; better for traditional instance backup

- Virtual Tape Library - fast access. 1500 tape..

- Virtual Tape Shelf - unlimited storage and OK with a 24 hour turn around time.

- Snowball - lots of data with a small pipe = loading data takes more than a week

- Import/Export - send a disk into AWS

# Demonstrate ability to make architectural trade off decisions involving database options

- Use RDS for ACID, joins, complex queries

- Don't use RDS for index and query focused, BLOB

- Use DynamoDB for indexed, query focused data, automated scalability

- Don't use DynamoDB for prewritten apps, data larger than 400kb, BLOB, relational data

- Use EC2 for DB2, Informix or Sybase

- Use EC2 for complete control of DB

# Demonstrate ability to implement the most appropriate data storage architecture

^^^See above^^^

## Products with automated backup

- Redshift

- Redis

- RDS

## Products that don't automatically backup

- EC2


# Determine use of synchronous versus asynchronous replication

- Multi-AZ RDS = synchronous

- RDS Read Replica = asynchronous

# Key Resources

- [AWS Storage Services Overview](https://d0.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf)