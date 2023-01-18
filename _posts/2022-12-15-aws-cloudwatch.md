---
layout: post
title: "AWS - CloudWatch"
description: ""
category: posts
tags: [aws-services, aws, mgt-governance, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

[CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) is probably the second most important service on AWS AFTER IAM. Why? Without CloudWatch it is almost impossible to figure out what is happening to your infrastructure running on AWS. Pricing is based on GB ingested per month, GB archived per month and alarms per month... and it ~~MIGHT be~~ is cheaper to store logs in S3.

## Components
There are numerous main parts to CloudWatch:
* Metrics - it collects data in a time series. The source of this data can be an AWS service or an application using the `PutMetricData` API action. 
* Alarms - a super cool way to alert when something *happens* like a state changes
* Dashboard - A super cool way to visualize what is happening on your metrics using the AWS management console.
* Logs - the ability to extract/be sent logs from various sources
* Events - simplified, event streams processing



## Metrics
Metrics are time-ordered set of data points. Metrics only exist in the region they are created, can't be deleted and expire after 14 days if no additional data is added. All metrics have a Name, a Namespace and one or dimensions. Metrics for things like non-associated EBS volumes are not reported.
- Retention Policy - metrics expire after 14 days. 
- Log Agent - the EC2 agent; can be an RPM package or installed via a python script
- Dimensions - a name/value pair that defines a category and a value for that metric. This enables sorting and grouping of metrics. Detailed monitoring enables additional dimensions.
- Namespaces - containers for metrics. Each namespace is something like AWS/EBS... or to put it another way "AWS/Service"

### Metric Filters
Turning log data into metrics is done through the use of [metric filters](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html). These log file patterns get turned into metrics AFTER they have been defined developed so, no, they do not retro-actively filter data. They only return the last 50 results. 

Metric filter elements:
They include the following key elements:
- Filter pattern - what to look for
- Metric Name - what should we name the metric?
- Metric Namespace - what section should our new metric appear in?
- Metric Value - what value do you want to publish?

## Alarms
Alarms take must be in same region as the data; have three states `OK`, `ALARM`, and `INSUFFICIENT_DATA`. Metrics are for 1 or 5 minutes so alarms have to be equal to or higher frequency. You can use the `mon-put-metric-alarm` command to create or update an alarm using the ancient Java command line tool. There is a max of 5000 alarms per AWS account. Alarm history is stored for 14 days. Alarms can trigger EC2 actions (reboot, stop, terminate, recover), autoscaling and SNS; alarm events can also be handled by EventBridge.

## Logs
Using CloudWatch logs, you can monitor logs from any source in AWS, like EC2, Route53, or CloudTrail, or on-prem using the CloudWatch Logs Agent, CloudWatch Unified Agent, and the SDK and then archive that data. It's an aggregation tool that will indefinitely store your logs. KMS encryption is supported.

- Log Group - a group of log streams that have the same properties, policies, and access controls. Log expiration policies specify how long to keep logs from 1 day to forever and all streams in the group inherit this setting. 
- Log Stream - a series of event from the same source; auto deleted after two months
- Log Event - an event that happened; includes a Timestamp and a message

Log Filters are filter expressions used to search logs and turned into the data into a metric. CloudWatch Logs Insights can be used to query logs and add queries to CloudWatch Dashboards.

### Exporting, Subscriptions & Streaming
Once the data is in Cloudwatch logs, it is pretty straightforward to send it elsewhere for processing. Logs can be exported to S3 but it can take up to 12 hours, the bucket must be encrypted with AES-256 (SSE-S3) not KMS, and it must be automated using the `CreateExportTask` call. 

Log Subscriptions Filters enable sending data to Kinesis Firehouse (perhaps to S3) or Kinesis Data Stream in near-real time, or Lambda (real-time). It's easy to load the data directly into ElasticSearch from there.

### CloudWatch Agent SSM Integration
Using the `AWS-ConfigureAWSPackage` document the SSM Run Command can install the CW agent. The SSM State Manager feature also works for this. Configure the CW agent using the config from SSM Parameter store.

## Monitoring Approach
At the time this course I was taking was written, which is way far in the past, here are 5 statistics total including average, min, max, sum, and `SampleCount`. To view these metrics from the command line `GetMetricStatistics` API command. 

Overall picture - compare SUM and difference between min and max values to get an overview of the data

Time Frames - alarms should be equal to or greater than the metric frequency

## Synthetics Canary
Synthetics canaries are configurable scripts that run on a schedule, to monitor endpoints and APIs. Canaries trigger alarms, are written in Node.js or python against a headless Chrome browser and check and store, availability, latency and screenshot of UI. There are blueprints to start with:
- Heartbeat monitor
- API Canary
- Broken Link Checker
- Visual monitoring - which compares screen shots
- Canary Recorder
- GUI workflow builder