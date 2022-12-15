---
layout: post
title: "AWS - KMS-HSM"
description: ""
category: posts
tags: [aws, kms, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

Encryption Key Management is a major function of online systems that requires lots of organization and no one will ever notice your efforts if things go right. Yet, if you screw it up, the world will come crashing down. Enter, [KMS](https://aws.amazon.com/kms/) and its good buddy [CloudHSM](https://aws.amazon.com/cloudhsm/). 

# KMS

KMS is a managed service that makes it easy for your to create, control, rotate and use encryption keys. For use with AWS services, KMS uses Symmetric encryption (AES-256). The Customer Master Key is the default key used by KMS to encrypt/decrypt the data key; the encrypted data key is stored with the data. Asymetric keys (RSA &amp; ECC) are supported for users outside of AWS who can't call KMS API

## Service Features

* Highly Performant - Very durable, low latency, and High Throughput

* No direct access to key - must use KMS API to use

* IAM Access controlled

* Regional Independence - your key can't travel the world...

* Integrated with AWS CloudTrail & Cloud watch; KMS keys are single region

* Full AWS API, CLI and SDK suppot

* pay per API call; can encrypt/decrypt up to 4KB in size

## Types of Customer Master Keys (CMK)

|       type       | Can view | can manage/rotate | dedicated to my account |           Key Policy           |
|:----------------:|:--------:|:-----------------:|:-----------------------:|:------------------------------:|
| Customer managed | yes      | yes; optional     | yes                     | yes                            |
| AWS Managed      | yes      | no; required      | yes                     | read only; CloudTrail friendly |
| AWS owned CMK    | no       | no; varies        | no                      | no                             |
|                  |          |                   |                         |                                |

## Setup

### KMS Key Material Origin
One time definition of key material; three types:  
- `EXTERNAL` which means you imported/manage/rotate them, must be 256 Symmetric key and asymmetric isn't supported), uses asymmetric key to encrypt the data for transport
- `AWS_KMS` which, duh, means KMS will handle it 
- `AWS_CLOUDHSM`Using HSM requires at least 2 HSM deployed across 2 AZ.

### Sharing
Roles need access to 'admin' access the key to USE the key. Role, users and external users can be granted the ability to use the key. Keys are managed regionally not globally; see multi-region

Unless the [key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) explicitly allows it, you cannot use IAM policies to allow access to a KMS key.

### Multi-region Key Approach
Primary key is created then replicated to other regions using the same ID; enables encryption and description being possible in different regions. Keys are NOT global; they are replicated and managed separately. Replicas can be promoted into primary for DR situations.

# CloudHSM

CloudHSM is a single tenant hardware security module that must in a VPC. There is very limited integration with AWS services with only S3 SSE-C, Oracle, MS SQL, and Redshift well supported. There is no AWS API into this thing... One big use case is SSL decryption. S3, KMS, and EBS encryption are possible but integration applications must be written. Support asymmetric and symmetric keys.

AWS does not have access to the contents of your module. If you loose the keys, they are lost if you did not have a copy. 

CloudTrail integration is supported. Syslog is as well. IAM permissions only apply to the CRUD of the HSM cluster; management of the cluster is done by CloudHSM software.

CloudHSM would be great if you need FIPS 140-2 level 3 validation and don't mind fixed cost of $5k per module. And you are responsible for the HA part so really you need 2 HSM... with a cost of $10k. And a $5k setup fee per region plus an hourly fee.

The HSM does work with peered VPC as well. A CloudHSM is accessed using an ENI in a public subnet of the VPC.

## HSM or KMS ?

Overall, KMS is probably adequate unless additional protection is necessary for some applications and data that are subject to strict contractual or regulatory requirements for managing cryptographic keys, then HSM should be used.

normal, or moderate security requirements? KMS

Integrate w/ AWS services other than S3, EBS, EC2, Redshift & RDS? KMS

Low variable costs based on usage? KMS 

Centralize Key Management onsite & AWS? HSM

FIPS 140-2 Level 1 or Level 2: KMS

FIPS 140-2 level 3? HSM

Complete Key Control? HSM

High-performance in-VPC bulk or realtime cryptographic acceleration () (including Apache, IIS and Nginx for SSL offload)? HSM using an on-device cryptographic user (CU)

PKCS#11, Java Cryptopgraphy extensions (JCE), Microsoft CryptoNG? HSM

