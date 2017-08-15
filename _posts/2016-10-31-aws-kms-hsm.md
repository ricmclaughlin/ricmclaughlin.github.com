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

KMS is a managed service that makes it easy for your to create, control, rotate and use encryption keys. It uses asymmetric encryption (2 different keys). Symmetric is not supported. KMS keys are single region. The Customer Master Key is the default key used by KMS to encrypt/decrypt. 

## Service Features

* Highly Performant - Very durable, Low Latency and High Throughput

* IAM Access controlled

* Regional Independence - your key can't travel the world...

* Integrated with AWS CloudTrail & Cloud watch

* Full AWS API, CLI and SDK suppot

## Setup

Roles don't need access to 'admin' access the key to USE the key. Role, users and external users can be granted the ability to use the key. Keys can be rotated yearly and KMS magically uses the right key when decrypting data.

# CloudHSM

CloudHSM is a single tenant hardware security module that you can place in a VPC. There is very limited integration with AWS services with only Oracle, MS SQL and Redshift well supported; S3, KMS and EBS encryption are possible but integration applications must be written.

AWS does not have access to the contents of your module. If you loose the keys, they are lost if you did not have a copy.

CloudTrail integration is supported. Syslog is as well.

CloudHSM would be great if you need FIPS 140-2 validation and don't mind fixed cost of $5k per module. And you are responsible for the HA part so really you need 2 HSM... with a cost of $10k. And a $5k setup fee per region plus an hourly fee.

The HSM does work with peered VPC as well.

## HSM or KMS ?

Overall, KMS is probably adequate unless additional protection is necessary for some applications and data that are subject to strict contractual or regulatory requirements for managing cryptographic keys, then HSM should be used.

normal, or moderate security requirements? KMS

Integrate w/ AWS services other than S3, EBS, EC2, Redshift & RDS? KMS

Low variable costs based on usage? KMS 

Centralize Key Management onsite & AWS? HSM

Complete Key Control? HSM

FIPS 140-1 or 140-2? HSM

High-performance in-VPC cryptographic acceleration (bulk crypto) (including Apache and Nginx for SSL offload)? HSM

