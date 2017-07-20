---
layout: post
title: "AWS - CloudWatch"
description: ""
category: posts
tags: [cloudwatch, aws, devops, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

[CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) is probably the second most important service on AWS AFTER IAM. Why? Without CloudWatch it is almost impossible to figure out what is happening to your infrastructure running on AWS. 

There are numerous main parts to CloudWatch:

* Metrics - it collects data in a time series. The source of this data can be an AWS service or an application using the `PutMetricData` API action. 

* Alarms - a super cool way to alert when something *happens* like a state changes

* Dashboard - A super cool way to visualize what is happening on your metrics using the AWS management console.

* Logs - the ability to extract/be sent logs from various sources

* Events - simplified, event streams processing

And a fourth part which is annoying, humorous and deprecated - a java based [command line tool](http://docs.aws.amazon.com/AmazonCloudWatch/latest/cli/SetupCLI.html).

## Metrics

Metrics are time-ordered set of data points. Metrics only exist in the region they are created, can't be deleted and expire after 14 days if no additional data is added. All metrics have a Name, a Namespace and one or dimensions. Metrics for things like non-associated EBS volumes are not reports 

Retention Policy - metrics don't expire by default; how long should we keep them?

Log Agent - the EC2 agent; can be an RPM package or installed via a python script

Dimensions - a name/value pair that defines a category and a value for that metric. This enables sorting and grouping of metrics. Detailed monitoring enables additional dimensions.

Namespaces - containers for metrics. Each namespace is something like AWS/EBS... or to put it another way "AWS/Service"

### Metric Filters

Turning log data into metrics is done through the use of [metric filters](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html). A couple of interesting facts about Metric Filters:

- only work on data after they have been created

- only return the first 50 results

- must include a metric name, name space and value

## Alarms

; actions the alarms take must be in same region as the alarm; have three states `OK`, `ALARM`, and `INSUFFICIENT_DATA`.

5000 alarms per account

mon-put-metric-alarm command creates or updates an alarm

### Metrics Filters

Metric Filters define which log file patterns get turned into metrics AFTER they are developed so, no, they do not retro-actively filter data and they only return the last 50 results. They include the following key elements:

- Filter pattern - what to look for

- Metric Name - what should we name the metric?

- Metric Namespace - what section should our new metric appear in?

- Metric Value - what value do you want to publish?


## Logs

Using CloudWatch logs, you can monitor logs from any source in AWS, like EC2 or CloudTrail, and then archive that data. It's an aggregation tool. 

- Log Group - a group of log streams that have the same properties, policies, and access controls. Retention settings specify how long to keep logs from 1 day to 10 years and all streams in the group inherit this setting. 

- Log Stream - a series of event from the same source; auto deleted after two months

- Log Event - an event that happened; includes a Timestamp and a message

### Exporting Logs

Logs can be exported to S3 or ElasticSearch directly from CloudWatch.

## Events (CloudWatch Events)

This service is similiar to CloudTrail but faster; you might call it the central nervous system of AWS.

Events - Events take the form of small JSON documents generated when a resource changes state, an API call is delivered via CloudTrail to CloudWatch, OR when your app generates an event

Rules - code that matches and routes incoming events; all rules that match an event get fired; can process the JSON or pass the entire message

Targets - Where events are processed including Lambda, Kinesis, SNS or built-in targets

## Monitoring Approach

At test time, which is way far in the past, here are 5 statistics total including average, min, max, sum and SampleCount. Use the `GetMetricStatistics` API command from the command line. 

Overall picture - compare SUM and difference between min and max values to get an overview of the data

Time Frames - alarms should be equal to or greater than the metric frequency


## Documentation
[CloudWatch Dev Doc](https://aws.amazon.com/cloudwatch/developer-resources/)