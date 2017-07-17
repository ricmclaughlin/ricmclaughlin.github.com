---
layout: post
title: "AWS - CloudWatch"
description: ""
category: posts
tags: [cloudwatch, aws, devops, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

[CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) is probably the second most important service on AWS AFTER IAM. Why? Without CloudWatch it is almost impossible to figure out what is happening to your infrastructure running on AWS. 

There are three parts to CloudWatch:

* Metrics - it collects data in a time series. The source of this data can be an AWS service or an application using the `PutMetricData` API action. 

* Alarms - a super cool way to alert when something *happens* like a state changes

* Dashboard - A super cool way to visualize what is happening on your metrics using the AWS management console.

And a fourth part which is annoying - a different [command line tool](http://docs.aws.amazon.com/AmazonCloudWatch/latest/cli/SetupCLI.html) that requires Java.

## Metrics

Metrics are time-ordered set of data points. Metrics only exist in the region they are created, can't be deleted and expire after 14 days if no additional data is added. All metrics have a Name, a Namespace and one or dimensions. Metrics for things like non-associated EBS volumes are not reports 

Retention Policy - metrics don't expire by default; how long should we keep them?

Log Agent - the EC2 agent; can be an RPM package or installed via a python script

Dimensions - a name/value pair that defines a category and a value for that metric. This enables sorting and grouping of metrics. Detailed monitoring enables additional dimensions.

Namespaces - containers for metrics. Each namespace is something like AWS/EBS... or to put it another way "AWS/Service"

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

- Log Event - an event that happened; includes a Timestamp and a message

- Log Stream - a series of event from the same source; auto deleted after two months

- Log Group - a group of log streams that have the same properties, policies, and access controls. Retention settings specify how long to keep logs from 1 day to 10 years and all streams in the group inherit this setting. 

## Monitoring Approach

At test time, which is way far in the past, here are 5 statistics total including average, min, max, sum and SampleCount. Use the `GetMetricStatistics` API command from the command line. 

overall picture - compare SUM and difference between min and max values to get an overview of the data

time frames - alarms should be equal to or greater than the metric frequency

## Events (CloudWatch Events)

This service is similiar to CloudTrail but faster; you might call it the central nervous system of AWS.

Events - Events take the form of small JSON documents generated when a resource changes state, an API call is delivered via CloudTrail to CloudWatch, OR when your app generates an event

Rules - code that matches and routes incoming events; all rules that match an event get fired; can process the JSON or pass the entire message

Targets - Where events are processed including Lambda, Kinesis, SNS or built-in targets

# Events

## EC2 Instances

EC2 instances report CloudWatch metric every 5 minutes; selecting the "Enable CloudWatch detailed monitoring" to see the metrics in 1 minute intervals. Even if you have more granular data, cloudwatch rolls the data up into 1 minute intervals. The goofy part is that you must enable a CloudWatch Write role on your EC2 instance. Unlike many instance options you can enable this feature after the instance starts. Autoscaling group metric aggregation is also reportable using detailed monitoring.

CPUUtilization, network (in/out), disk (read/write - ops and bytes) and status checks are reported to CloudWatch by default. RAM & diskspace is a custom metric. 

EC2 instances must have permissions to write information to CloudWatch; create a service role then add a "CloudWatchRole"

### Status Checks

Status checks run every 5 minutes. 

System Status => checks look at the host hardware. Stop and start the VM.

Instance Status => checks the VM; reboot the machine

### EBS Monitoring

There are four status check statuses coming from EBS volumes:

- `ok` - All good.

- `warning` = Degraded or Severely Degraded

- `impaired` = Stalled or Not Available 


#### gp2 Instances

`VolumeReadBytes` & `VolumeWriteBytes` - throughput measurements can be reported in SUM or more importantly AVERAGE. Average should tell you if you are ending up with a bottleneck or excess throughput.

`VolumeReadOps` & `VolumeWriteBytes` - This tells up the number of IOPS

`VolumeTotalReadTime` & `VolumeTotalWriteTime` - Looking a trend line of this metric can show what the future requirements might be.

`VolumeQueueLength` - How long the queue is can determine usage patterns. 

#### io1 Instances

The `VolumeThroughputPercentage` & `VolumeConsumedReadWriteOps` metrics show how much of the provisioned throughput you are using. 

## RDS Service

There are some non-critical metrics that can be monitored like `CPUUtilization`, `DatabaseConnections`, `DiskQueueDepth`, `FreeableMemory` and `FreeStorageSpace`.

More critical metrics and their solutions include:

| **Metric**  | **Solution**  |
|:-----------------------------------------|:--------------------------------------------------------| 
| SwapUsage | Increase RAM |
|ReadIOPS/WroteIOPS| Increase IOPS |
|ReadLatency/WriteLatency| Increase IOPS |
|ReadThroughPut/WriteThroughPut| Increase IOPS |
|FreeStorageSpace| Increase disk space |
|ReplicaLag | Increase IOPS or instance size |

In addition to CloudWatch you can use the RDS service console to monitor RDS events. Event Subscriptions can be created as well. 

## ElastiCache Service

Makes more sense in with the rest of the information about [ElastiCache]({{BASE_PATH}}/posts/aws-sysops-elasticache) 

## Elastic Load Balancing Service

If you know there is a lot of traffic on the way, call AWS and get them to "pre-warm" your ELB. Metrics are reported every 60 seconds - no traffic = no metric

| **Metric**  | **Solution**  |
|:-----------------------------------------|:--------------------------------------------------------| 
| HTTP Latency | AVG and MAX is useful; reported by AZ |
| BackendConnectionErrors | Look at SUM and difference between min and max values |
| SurgeQueueLength | Max in queue is 1024 |
| SpillOverCount | This is a bad, bad thing. Avoid. ELB reports a `503 - Service Unavailable` |
| HealthyUnHealthyHostCount | Just like what it says |

## API 

| **Call**  | **Purpose**  |
| `put-metric-data` | custom metrics | 
| `put-metric-alarm` | creates metric; `INSUFFICIENT_DATA` state at first | 
| `disable-alarm-actions` | alarm active; doesn't fire | 
| `enable-alarm-actions` | alarm active; fires | 
| `list-metrics` | lists all the metrics |
| `get-metric-statistic` | get a specific metric |


## Documentation

[CloudWatch Dev Doc](https://aws.amazon.com/cloudwatch/developer-resources/)
