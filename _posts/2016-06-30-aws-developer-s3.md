---
layout: post
title: "AWS Developer - S3"
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

[Redshift](https://aws.amazon.com/redshift/) - data warehouse service that is very cheap and quite functional; the fastest [growing group at AWS](http://www.theregister.co.uk/2015/04/15/amazon_redshift_big_growth/)


# S3
S3 is an object, as in file, based storage and is a key-value based - the key is the file name and the data in the file which is the value in addition to other data like the Version ID, access control information and metadata. S3 offers *Read after Write* consistency for `PUTS` but **only** *Eventual* consistency for update `PUTS` or `DELETES`.

AWS charges you for storage, requests and data transfer.

Use Multipart upload to stop and resume uploads and ideally for any file over 100mg and required over 5Gig. A benefit of multi-part upload is that you can upload a file as it is being created. 

S3 Limits - the minimium size for a s3 object is 0 bytes; 100 S3 buckets per account; 5Tb object size max.

## Security
Buckets are private by default but access control you can define a bucket policy and or an access control list. Access logs can also be defined if required. 

### Bucket Policies
While IAM policies apply to the user level, bucket policies apply to the resource level. Bucket owners can specify what other users can do the contents of the bucket - provided they can log into the console. This includes scenarios where others own the object in the bucket - think cross account PUT of an object.

#### Bucket Policies are:

1. Resource Based

1. A JSON file limited to 20kb

2. Should be used to manage all cross-account permissions for ALL S3 resources - ACL do not control detailed permissions.

1. Possibily conditional - they can use ACL attributes to conditionally grant access to objects

### S3 ACL
Stored in XML and can be used to manage access to objects NOT owned by the bucket owner. They can not explicitly deny permissions; they grant read and write permissions to other AWS accounts.

#### Elements of an Access Policy include:

1. Principle - specific to bucket policies NOT user policies.

1. Effect - allow or deney

1. Action - what we want to describe

1. Resource - the ARN

### S3 IAM Policies
S3 IAM policies can NOT grant anonymous access. But you can DENY access with IAM Policies. 

The prefix for a bucket policy is "arn:aws:s3:::"

The star (*) can be used in a policy.

Template strings can be used too like -> ${aws:username}

### Data in-Transit
Data in-transit security happens using SSL or TLS (which is SSL v 3.1)

### Data at-Rest
There are three flavors of Server Side Encryption (SSE) Server side encryption can be enabled using the REST API using x-amz-server-side-encryption header.

* S3 Managed Keys (SSE-S3) - no audit trail. Can't imagine this gets used much.

* AWS Key Management Service, Managed Keys (SSE-KMS) - this provides an audit trail.

* Customer Provided Keys (SSE-C) - you provide key; you manage key; you upload key to AWS...

* Customer encrypted - You could encrypt on the client too. 

## S3 Storage Classes
S3 - AWS guarantees 99.99% availability for the S3 Platform and 11 x 9 for durability.

S3-Infrequently Accessed - Less frequently used data that needs rapid access - there is a charge a retrieval fee

S3-Reduced Redundancy Storage - 99.99% durability - for files that can be regen'd.

[glacier](https://aws.amazon.com/glacier/) - an object storage capability that is mostly offline and way, way cheaper than S3. It might take hours (like 3-5 hours) to retrieve an object from glacier so it is mostly suited towards stuff you need to store but will rarely, if ever, need to access.  

## Versioning
Stores all version of an object; can be suspended but not disabled for bucket; can be used with MFA to provide extra layer of DELETE security. Cross region replication requires versioning and an IAM role setup. ALL version of the file are stored so the amount of spaced required can end up being HUGE. Versioning only starts after it is enable and files have been added - existing files end up with a NULL version.

## Storage Tiers & Lifecycle Policies

S3 - standard tier
 
Move to S3-IA - must be here for a minimum of 30 days (and be 128kb) before it can move to glacier

Move to Glacier - can move here directly after creation if needed
Permenantly Delete - yep, cool stuff this.

S3 lifecycle Policy - S3 bucket policies require a Principal be defined.

## Bucket Names &amp; Such
Bucket names can not start with a '.' or '-' and cannot be formatted like an IP address. Remove public read access and use signed URLs with expiry dates to prevent read access from unauthorized sites. 

Files must be stored in buckets and buckets are a universal namespace.

Using a sequential prefix, such as time-stamp or an alphabetical sequence, increases the likelihood that Amazon S3 will target a specific partition for a large number of your keys, overwhelming the I/O capacity of the partition. If you introduce some randomness in your key name prefixes, the key names, and therefore the I/O load will be distributed across more than one partition.


## S3 Website Hosting
To make a website on S3 at a minimium upload an index document to your S3 bucket, enable static website hosting in your S3 bucket properties, select the 'Make Public' permission for your bucket’s objects or apply a bucket permission script. The only allowed domain prefix when creating Route 53 Aliases for S3 static websites is the 'www' prefix. S3 buckets do not have https!

CORS rules - The bucket hosting the assets needs CORS configuration - add the domain of the "static" site to fix this up.

## S3 Transfer Acceleration
Edge Location used by CloudFront are not just for download acceleration. S3 Transfer Acceleration uses the AWS backbone to speed up uploads. This costs more money. 

## S3 API Reference Points

### Bucket API
Seeing that S3 is a REST API, all the operations... well, they are REST.

| S3 Bucket API Method  | Notes  |
|:--------------------------------------:|:----------------------------------------:|
|    DELETE                 | Bucket, CORS, website, etc.  |
|  GET  |  Bucket, CORS, website, etc.  |
|  PUT | Bucket, CORS, website, etc. | 
|  HEAD | Unsure if you have access and a bucket exist? | 

### Object API
This section of the API is REST-like. Multi Part upload - Multi-part upload API allows you to upload parts of an object once broken apart. As a file/object is being created, the multi-part upload API will allow you to upload the file to S3. Only after all parts of the object have been uploaded do you execute the CompleteMultipartUpload API call which completes a multi-part upload by assembling previously uploaded parts.

You first initiate the multi-part upload and then upload all parts using the Upload Parts operation (see Upload Part). After successfully uploading all relevant parts of an upload, you call this operation to complete the upload. Upon receiving this request, Amazon S3 concatenates all the parts in ascending order by part number to create a new object. In the Complete Multi-part Upload request, you must provide the parts list. You must ensure the parts list is complete, this operation concatenates the parts you provide in the list. For each part in the list, you must provide the part number and the ETag header value, returned after that part was uploaded.

| S3 Object API  | Notes  |
|:--------------------------------------:|:----------------------------------------:|
| DELETE                    |   single file or using a DELETE Object |
| GET | Object, Object ACL, Object Torrent |
| HEAD | get meta data (might returns 404 or 403 error) |
| POST | writes an object to S3 from a form (can also copy the file) |
| | |

### Error Codes from API
Common Errors - S3 handles error codes with HTTP response codes - which makes sense seeing that we are using a REST API here.

| S3 API HTTP Responses  | Notes  |
|:--------------------------------------:|:----------------------------------------:|
| 403 Forbidden                    |   AccessDenied or AccountProblem |
| 409 Conflict  | BucketNotEmpty - you tried to delete a bucket that was not empty. |
| 400 Bad Request | InvalidBucketName, InlineDataTooLarge, InvalidPart/InvalidPartOrder, TooManyBuckets|
| 404 Not Found | NoSuchBucket, NoSuchBucketPolicy |
| 200 - Yeah!| All is good, continue about your day!|


# Resources

## Qwik Labs
[Introduction to Amazon Simple Storage Service (S3)](https://qwiklabs.com/focuses/2355)
[Introduction to AWS Key Management Service](https://qwiklabs.com/focuses/2367) - yeah, this is about Key Management Service but under-the-covers this is about S3.

## Reading
[s3 Developer Guide](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

## Videos
[AWS S3 Deep Drive and Best Practices](https://www.youtube.com/watch?v=1TvJCLl9NNg)
