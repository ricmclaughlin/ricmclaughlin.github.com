---
layout: post
title: "AWS Developer Certification"
description: ""
category: 
tags: [AWS, developercert]
---
{% include JB/setup %}
As part of studying for the [AWS Developer Certification - Associate](https://aws.amazon.com/certification/certified-developer-associate/), I'm pulling together some study notes here on my blog. For each chunky topic, I'll capture the big facts and stuff I had to remind myself to remember:

* [AWS SNS notes]({{ BASE_PATH }}/aws-developer-certification-sns)

* [AWS CloudFormation notes]({{ BASE_PATH }}/aws-developer-certification-cloudformation)

* [AWS DynamoDB notes]({{ BASE_PATH }}/aws-developer-certification-dynamo-db)

* [AWS CloudFormation notes]({{ BASE_PATH }}/aws-developer-certification-cloudformation)

* [AWS IAM notes]({{ BASE_PATH }}/aws-developer-certification-iam)

* [AWS Storage & S3 notes]({{ BASE_PATH }}/aws-developer-certification-s3)

* [AWS EC2 notes]({{ BASE_PATH }}/aws-developer-certification-ec2)

* [AWS VPC notes]({{ BASE_PATH }}/aws-developer-certification-vpc)

Overall AWS Architecture:

Regions - an independent area of several avialability zones designed to be independent of other regions

Availability zone - each availability zone is one or more data centers linked by private networking infrastructure, not public Internet

Edge Locations - located in densely populated areas designed to deliver fast content to these areas

There are a number of services on AWS that are free - auto scaling, kinesis, CloudFormation, VPC and IAM. AWS provides an SDK in Go, PHP, JavaScript, python and Java but no perl.