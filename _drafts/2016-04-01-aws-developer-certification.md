---
layout: post
title: "AWS Developer Certification - Overview"
description: ""
category: projects
tags: [aws, developercert]
---
{% include JB/setup %}
As part of studying for the [AWS Developer Certification - Associate](https://aws.amazon.com/certification/certified-developer-associate/), I'm pulling together some study notes here on my blog. For each chunky topic, I'll capture the big facts and stuff I had to remind myself to remember.

As a note, I passed this certification exam an earned the AWS Developer Certification - Associate.

<img src="{{ BASE_PATH }}/assets/themes/ricify/images/Developer-Associateclr.jpg" width="300">

## Application Services
* [AWS Simple Notification Service (SNS)]({{ BASE_PATH }}/posts/aws-developer-sns)

* [AWS Simple Queue Service (SQS)]({{ BASE_PATH }}/posts/aws-developer-sqs)

* [AWS Simple Work Flow (SWF)]({{ BASE_PATH }}/posts/aws-developer-swf)

## Management Tools
* [AWS Cloud Formation]({{ BASE_PATH }}/posts/aws-developer-cloudformation)

* [AWS Cloud Front]({{ BASE_PATH }}/posts/aws-developer-cloudfront)

## Security and Identity
* [AWS IAM]({{ BASE_PATH }}/posts/aws-developer-iam)

## Databases
* [AWS DynamoDB, RDS, &amp; Elasticache]({{ BASE_PATH }}/posts/aws-developer-dynamodb)

## Storage
* [AWS Storage & S3 notes]({{ BASE_PATH }}/posts/aws-developer-s3) - including s3, Cloud Front, Elastic File System (EFS), Snowball, Storage Gateway and Glacier.

## Compute
* [AWS EC2]({{ BASE_PATH }}/posts/aws-developer-ec2)

* [Elastic Beanstalk]({{ BASE_PATH }}/posts/aws-developer-elasticbeanstalk)

## Networking

* [AWS VPC]({{ BASE_PATH }}/posts/aws-developer-vpc)

# Overall AWS Architecture:

1. Regions - an independent area of several avialability zones designed to be independent of other regions

2. Availability zone - each availability zone is one or more data centers linked by private networking infrastructure, not public Internet

1. Edge Locations - located in densely populated areas designed to deliver fast content to these areas

## Free AWS Services
There are a number of services on AWS that are free - auto scaling, Kinesis, CloudFormation, VPC and IAM. AWS provides an SDK in Go, PHP, JavaScript, python and Java but not perl.

## Sharing Security Responsibility
There are three levels of shared security in AWS: Infrastructure Services (for services like EC2 and RDS), Container Services (for container services) and Abstracted Services (for services like SQS, SES and other high level apps).



