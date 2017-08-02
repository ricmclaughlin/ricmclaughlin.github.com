---
layout: post
title: "AWS - Cloudtrail"
description: ""
category: posts
tags: [aws, cloudtrail, aws-dev-ops-pro, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

AWS [CloudTrail](https://aws.amazon.com/documentation/cloudtrail/) is a fully managed web service that records an auditing of AWS API calls for your account and delivers log files to you encypted in an s3 bucket. This is a key component for auditing and compliance needs - both HIPAA and PCI require 6 years of access logs to be stored and without CloudTrail that just isn't possible! CloudTrail stores log data in JSON format. CloudTrail adds value to logging is not a replacement for other system or AWS service logs.

You can have a total of 5 trails per region.

## Log Contents 

The logs contain the user ID, time, source IP, req and res. CloudTrail records the API calls by a user OR on behalf of an AWS service; use the `invokedBy` field in the request to figure that out.

## Configuration

CloudTrail is configured regionally and can optionally be configured to be enabled for all regions. The logs it generates are delivered to an S3 Bucket and can be aggregated across regions. World wide events are recorded to any trail that is configured for all regions. 

By default CloudTrail uses SSE-S3. KMS keys can be used; key and S3 bucket must be in the same region. Because so much, should-be-secured-data is stored in these logs, a security administrator can create a trail that applies to all regions and encrypt the log files with one a single KMS key. In addition, creating a bucket policy to restrict access to the logs is a good idea as well.

An important feature to consider is how to configure logs to notify in the event of misconfiguration.

The log file validation feature creates a digest of log files and the CLI can validate the logs afterwards. This feature makes sure that all the log files are present and unmodified by hashing the data using SHA-256 then signing them. 

## Operation

I'd recommend creating an SNS notification for when log file delivery has occurred. Use this notification to trigger loading the data into an analysis tool. 

In the past CloudTrail data was really only good for figuring out what happened in the past. Now you can use the CloudWatch Logs feature and monitor the CloudTrails data in semi-real time by filtering the log for events and creating an event then an alarm for the event. Logs are delivered within 15 minutes of an API call and delivery of the logs is timed about every 5 minutes.

Popular events to monitor for include: 

1. Creation, deletion and modification of security groups and VPC

1. Changes to IAM or S3 Bucket Policies

1. Failed Console Logins

1. Authorization failures

1. Doing anything to an EC2 instance


## CloudTrail Log Analysis

There are tons of options to analyze cloud trail data. You can use external sources like [AlertLogic](https://www.alertlogic.com/solutions/log-correlation-and-analysis/), [Loggly](https://www.loggly.com/intro-to-log-management/), [Splunk](https://www.splunk.com/) OR Sumologic or perhaps use the ubquitous [ELK stack](https://www.elastic.co/webinars/introduction-elk-stack). You can also add Apache Spark on EMR to analyze these logs.

## Cross Account Consolidated CloudTrail

In cases where there are numerous accounts, cross account Consolidated CloudTrail logs are a handy feature. To make this work you need a role that enables other account to place logs in the paying account S3 bucket.