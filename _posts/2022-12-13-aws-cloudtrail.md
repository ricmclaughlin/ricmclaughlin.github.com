---
layout: post
title: "AWS - Cloudtrail"
description: ""
category: posts
tags: [aws, mgt-governance, aws-dev-ops-pro, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

AWS [CloudTrail](https://aws.amazon.com/documentation/cloudtrail/) is a fully managed web service that records an auditing of AWS API calls for your account and delivers log files to you encypted in an s3 bucket or [CloudWatch](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/monitor-cloudtrail-log-files-with-cloudwatch-logs.html). CloudTrail stores log data in JSON format. CloudTrail adds value to logging is not a replacement for other system or AWS service logs.

You can have a total of 5 trails per region. CloudTrail is enabled and applied to all regions by default. The logs contain the user ID, time, source IP, req, and res. CloudTrail records the API calls by a user OR on behalf of an AWS service; use the `invokedBy` field in the request to figure that out. Stored for 90 days by default; longer retention by sending log to S3 then use Athena to analyze.

When using Organizations, an organization wide trail can be created in the management account

## Event types
- Management Events - can separate read and write events; enabled by default
- Data Events - S3 object level operations; Lambda execution activity; disabled by default; can separate read and write events
- Insights Events - costs $ therefore disabled by default. Detects unusual activity like resource provisioning, service limit hits, bursts of IAM activity, gaps in periodic maintenance activity AFTER analyzing normal activity to create a baseline then analyzes write events. Anomaolies appear in the console, in the S3 based log file, and as an event in EventBridge.

## Configuration

By default CloudTrail uses SSE-S3. KMS keys can be used; key and S3 bucket must be in the same region. Because so much, should-be-secured-data is stored in these logs, a security administrator can create a trail that applies to all regions and encrypt the log files with one a single KMS key. In addition, creating a bucket policy to restrict access to the logs is a good idea as well.

An important feature to consider is how to configure logs to notify in the event of misconfiguration.

The log file validation feature creates a digest of log files and the CLI can validate the logs afterwards. This feature makes sure that all the log files are present and unmodified by hashing the data using SHA-256 then signing them. 

## Operation
Logs are delivered within 15 minutes of an API call and delivery of the logs is timed about every 5 minutes. 
Semi-realtime event speed:
0. EventBridge
0. Use CloudWatch Logs feature and monitor the CloudTrails data in semi-real time by filtering the log for events then creating a metric/alarm for the event. 
0. Use an S3 event. (better for log integrity, cross account patterns, and long-term storage)

Popular events to monitor for include: 

1. Creation, deletion and modification of security groups and VPC

1. Changes to IAM or S3 Bucket Policies

1. Failed Console Logins

1. Authorization failures

1. Doing anything to an EC2 instance





