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

KMS is a managed service that makes it easy for your to create, control, rotate and use encryption keys. It uses asymmetric encryption (2 different keys). Symmetric is not supported. The Customer Master Key is the default key used by KMS to encrypt/decrypt the data key; the encrypted data key is stored with the data. 

## Service Features

* Highly Performant - Very durable, low latency, and High Throughput

* IAM Access controlled

* Regional Independence - your key can't travel the world...

* Integrated with AWS CloudTrail & Cloud watch; KMS keys are single region

* Full AWS API, CLI and SDK suppot

* pay per API call; can encrypt/decrypt up to 4KB in size

## Setup

Roles need access to 'admin' access the key to USE the key. Role, users and external users can be granted the ability to use the key. Keys can be rotated yearly and KMS magically uses the right key when decrypting data.

## Types of Customer Master Keys (CMK)

| type | Can view | can manage | dedicated to my account|
|Customer managed | yes | yes | yes |
| AWS Managed | yes | no | yes |
| AWS owned CMK | no | yes | no|

# CloudHSM

CloudHSM is a single tenant hardware security module that must in a VPC. There is very limited integration with AWS services with only Oracle, MS SQL, and Redshift well supported. There is no AWS API into this thing... One big use case is SSL decryption. S3, KMS, and EBS encryption are possible but integration applications must be written. 

AWS does not have access to the contents of your module. If you loose the keys, they are lost if you did not have a copy.

CloudTrail integration is supported. Syslog is as well.

CloudHSM would be great if you need FIPS 140-2 level 3 validation and don't mind fixed cost of $5k per module. And you are responsible for the HA part so really you need 2 HSM... with a cost of $10k. And a $5k setup fee per region plus an hourly fee.

The HSM does work with peered VPC as well. The HSM module is a cross AZ cluster and is accessed using an ENI in a public subnet of the VPC.

## HSM or KMS ?

Overall, KMS is probably adequate unless additional protection is necessary for some applications and data that are subject to strict contractual or regulatory requirements for managing cryptographic keys, then HSM should be used.

normal, or moderate security requirements? KMS

Integrate w/ AWS services other than S3, EBS, EC2, Redshift & RDS? KMS

Low variable costs based on usage? KMS 

Centralize Key Management onsite & AWS? HSM

FIPS 140-2 Level 1 or Level 2: KMS

FIPS 140-2 level 3? KMS

Complete Key Control? HSM

High-performance in-VPC cryptographic acceleration (bulk crypto) (including Apache and Nginx for SSL offload)? HSM

PKCS#11, Java Cryptopgraphy extensions (JCE), Microsoft CryptoNG? HSM

