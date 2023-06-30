---
layout: post
title: "AWS - Data Storage"
description: ""
category: posts
tags: [aws-guides, storage, aws-solutions-arch-pro]
---
{% include JB/setup %}

## S3
- Re-buildable asset? S3-IA (because you can regenerate it!)
- Infrequently access (Backup, archive or DR yet still active)? S3-IA
- Greater than 4 hours retrieval, cheap (archive, digital preservation)? Glacier
Cheap archive but charged for instant retrieval? Glacier instant retrieval 
Cheap archive but ok with little delay?Glacier Flexible _expedited_ (1-5 minutes), _standard_ (3-5 hours), or _bulk_ (5-12 hours)
Pure archive with 12-48 hours? S3 Deep archive (think frozen data)
- Change S3 access permissions? role/group with policy (loose permission before assume role) OR bucket policy (to add access permissions using existing role/user/group)
- Different access to same bucket? Access points
- Protect S3 file from delete or overwrite? enable versioning
- Protect S3 bucket deletion? backup bucket to another account
- Only certain folks can delete and overwrite? enable MFA delete
- Only certain folks can change state of versioning? enable MFA
- S3 Cross-region replication? Requires versioning on source bucket; deletes and lifecycle actions not supported
- Authenticated upload? [Pre-signed upload URLs](http://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html)
- Faster download of large objects? [range based `GET`](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) or use CloudFront
- [High TPS workload](http://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html#get-workload-considerations)? prefix the key name with a hash or reverse timestamp string
- Slow file upload speed? [S3 Transfer acceleration](http://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html) or [multi-part uploads](http://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html)
- `PUT` optimization on a strong network? multi-part uploads of between 25-50MB to max throughput
- `PUT` optimization on a weak network? multi-part uploads of about 10MB to prevent upload restart
- bucket is public OR objects are public? Not Trusted Advisor (can't check for public objects in the bucket) instead use EventBridge/S3 Events OR Config.
- WORM data

## EBS
- Burst-able? gp2/3
- temp storage 365+K IOPS? instance store
- Shared file storage, no POSIX? EBS io2 multi-attach
- Shared file storage, POSIX? EFS
- NoSQL? io2/3/express (365K IOPS)
- MPP/Hadoop/EMR? D2 3.5 Gib/s
- Backup instance store? file backup on mounted EFS/EBS, no snap shotting possible

## DataSync/Storage Gateway/Snowball
- Sync from on-prem to S3, EFS, EBS FSx? DataSync (agent that includes encryption and integrity checks for NFS or SMB)
- Between storage services? DataSync (EFS -> EBS on EC2 instance)
- Low end DR? S3 directly using DataSync (Storage Gateway is less performant).
- When Storage Gateway Cached? frequently access to small amount of data; less to maintain local so cheaper outlay,
- When Storage Gateway Stored? low latency for ALL data with EBS Snapshots; snapshots can be used as a migration tool; better for traditional instance backup
- When Virtual Tape Library? fast restore. 1500 tapes; stored on S3.
- When Virtual Tape Archive? unlimited storage and OK with a 24 hour turn around time; 
- When Snowball? more than 2TB of data for a T3 connection, 5TB for a 100MB connection, and 60TB for 1000 Mbps connections.

# Database options
- ACID, joins, complex queries? RDS 
- Don't use RDS for index and query focused, BLOB
- Use DynamoDB for indexed, query focused data, automated scalability
- prewritten apps, data larger than 400kb, BLOB, relational data? !DynamoDB
- DB2, Informix or Sybase? EC2
- Complete control of DB? EC2 for 

## Backup
- Need to automate backup? EBS, FSx for Lustre
- No need for Automated backup? Redshift, Redis, RDS, FSx
- Automate EBS backup? Data Lifecycle Manager (DLM)
- Automate all AWS Backup? = AWS Backup (including EBS)

## EC2 attached storage
- Long term data storage? EBS
- data shared between instance fast? memecached or redis
- persist data shared between instances? EFS
- DB or INTENSE data warehouse server? IOPS/io1
- General purpose = gp2
- Large I/O including EMR, Kafka, log processing and data warehouse ETL? st1 (sequential data reads)
- Super high disk IO? either RAID 0 or RAID 10 EBS, EBS-optimized instances, modern Linux kernel

## FSx vs EFS
- FSx for windows? SMB/DFS/namespaces windows-based file server supporting AD; can be used for centralized storage for Sharepoint, SQL server, Workspaces or IIS.
- HPC applications? FSx Lustre (sub-millisecond latencies, 100 GPS throughput and millions of IOPS; can be loaded from S3)
- NFS for Linux boxes? EFS
- Linux >7000 file operations per sec? EFS Max I/O 
- Shared File storage for EC2 multi-threaded linux? General Purpose EFS (burstable)

## FSx
- No downtime FSx for Windows Single AZ to multi-AZ? Create a multi-AZ file system then use DataSync to migrate the data over. 
- For downtime ok FSx for Windows Single AZ to multi-AZ? Backup the single-AZ and restore multi-AZ works well.
- Reduce size of file system? create smaller version and DataSync
- Increase size of file system? increase must be greater than 10%, must have throughput of 16 MB/s, 6 hours after last increase
