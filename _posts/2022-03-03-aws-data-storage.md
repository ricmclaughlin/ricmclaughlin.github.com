---
layout: post
title: "AWS - Data Storage"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Demonstrate ability to make architectural trade off decisions involving storage options

[S3](/posts/aws-S3) - Parallelization, alphabetical order, randomness in URL -> SSHHDDMMYY

## S3

Good for: Web Storage, Static websites, Data Lake, backup/archive

Bad For: File, System, Structured data, archive (use Glacier class), dynamic websites

### S3 Storage Classes

Rebuildable asset? S3-IA (because you can regenerate it!)

Infrequently access (Backup & Archive or DR yet still active)? S3-IA

Greater than 4 hours retrieval, cheap (archive, digital preservation)? Glacier

### Use Cases

Change S3 access permissions? IAM & bucket policy

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


## EFS vs FSx for Windows

EFS: Shared File storage for EC2 multi-threaded linux

Linux >7000 file operations per sec? Max I/O else General Purpose (burstable)

FSx for Windows: Active directory, any Windows use-case

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

## FSx vs EFS

FSx for windows = is just a SMB windows-based, file server supporting SMB, AD and DFS namespaces; can be used for centralized storage for Sharepoint, SQL server, Workspaecs or IIS.

FSx Lustre = HPC applications; sub-millisecond latencies, 100 GPS throughput and millions of IOPS; can be backed by S3

EFS = NFS for Linux boxes

## DataSync/Storage Gateway/Snowball

Use Cases:

- DataSync - Sync from on-prem to S3, EFS, EBS FSx (agent that includes encryption and integrity checks for NFS or SMB)

- Between storage services? DataSync (EFS -> EFS on EC2 instance)

- Low end DR = S3 directly using DataSync (Storage Gateway is less performant).

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

## Products with automated backup

- Redshift

- Redis

- RDS

## Products that don't automatically backup

- EC2 (EBS)

# Determine use of synchronous versus asynchronous replication

- Multi-AZ RDS = synchronous

- RDS Read Replica = asynchronous

# Determining access issues

Check if a bucket is public OR objects are public? Not Trusted Advisor (can't check for public objects in the bucket) instead use EventBridge/S3 Events OR Config.

# EC2 attached storage

Long term data storage? EBS

data shared between instance fast? memecached or redis

persist data shared between instances? EFS

don't care? instance store

DB or INTENSE datawarehouse server = IOPS/io1

General purpose = gp2

Large I/O including EMR, Kafka, log processing and data warehouse ETL = st1 (sequential data reads)

Super high disk IO? either RAID 0 or RAID 10 EBS, EBS-optimized instances, modern Linux kernel