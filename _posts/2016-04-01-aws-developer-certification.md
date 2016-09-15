---
layout: post
title: "AWS Developer Certification"
description: ""
category: posts
tags: [AWS, developercert]
---
{% include JB/setup %}
As part of studying for the [AWS Developer Certification - Associate](https://aws.amazon.com/certification/certified-developer-associate/), I'm pulling together some study notes here on my blog. For each chunky topic, I'll capture the big facts and stuff I had to remind myself to remember:


## Application Services
* [AWS Simple Notification Service (SNS)]({{ BASE_PATH }}/aws-developer-certification-sns)

* [AWS Simple Queue Service (SQS)]({{ BASE_PATH }}/aws-developer-certification-sqs)

* [AWS Simple Work Flow (SWF)]({{ BASE_PATH }}/aws-developer-certification-swf)

## Management Tools
* [AWS Cloud Formation]({{ BASE_PATH }}/aws-developer-certification-cloudformation)

* [Opsworks]()

* [Trusted Advisor]()

## Security and Identity
* [AWS IAM]({{ BASE_PATH }}/aws-developer-certification-iam)

* [Key Management Services]()

## Databases
* [AWS DynamoDB, RDS, &amp; Elasticache]({{ BASE_PATH }}/aws-developer-certification-dynamo-db)

## Storage
* [AWS Storage & S3 notes]({{ BASE_PATH }}/aws-developer-certification-s3) - including s3, Cloud Front, Elastic File System (EFS), Snowball, Storage Gateway and Glacier.

## Compute
* [AWS EC2 notes]({{ BASE_PATH }}/aws-developer-certification-ec2)

* [Elastic Beanstalk]()

* [Lambda]()

## Networking

* [AWS VPC notes]({{ BASE_PATH }}/aws-developer-certification-vpc)

# Overall AWS Architecture:

1. Regions - an independent area of several avialability zones designed to be independent of other regions

2. Availability zone - each availability zone is one or more data centers linked by private networking infrastructure, not public Internet

1. Edge Locations - located in densely populated areas designed to deliver fast content to these areas

## Free AWS Services
There are a number of services on AWS that are free - auto scaling, Kinesis, CloudFormation, VPC and IAM. AWS provides an SDK in Go, PHP, JavaScript, python and Java but not perl.

## Sharing Security Responsibility

There are three levels of shared security in AWS: Infrastructure Services (for services like EC2 and RDS), Container Services (for container services) and Abstracted Services (for services like SQS, SES and other high level apps).


