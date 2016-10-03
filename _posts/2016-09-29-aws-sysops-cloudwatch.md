---
layout: post
title: "AWS SysOps - CloudWatch"
description: ""
category: posts
tags: [aws, developercert, cloudfront]
---
{% include JB/setup %}

[CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) is probably the second most important service on AWS AFTER IAM. Why? Without CloudWatch it is almost impossible to figure out what is happening to your infrastructure running on AWS. 

There are three parts to CloudWatch:

* Metrics - it collects data in a time series. The source of this data can be an AWS service or an application using the `PutMetricData` API action. Namespaces are containers for metrics. Dimensions are a name/value pair that ID a metric BUT only aggregate for EC2 and other AWS defined dimensions.

* Alarms - a super cool way to alert when something happens; think SNS message when your ELB spills overs.

* Dashboard - A super cool way to visualize what is happening on your metrics using the AWS management console.

# Monitoring
Use the `GetMetricStatistics` API command from the command line. From an analysis perspective, look at SUM and difference between min and max values to get an overall picture.

## EC2 Instances

EC2 instances report CloudWatch metric every 5 minutes; selecting the "Enable CloudWatch detailed monitoring" to see the metrics in 1 minute intervals. Unlike many instance options you can enable this feature after the instance starts.

The amount of memory being used, The amount of swap space used, How much disk space is available; EC2 instances must have permissions to write information to CloudWatch.

### EBS
Status checks run every 5 minutes. Can report ok, warning, impaired; and reports insufficient-data when running.

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

## ElastiCache Service
The differences between memcached and redis make for differences in monitoring. memecached is multithreaded while redis is single threaded.

| **Metric**  | **Solution**  |
|:-----------------------------------------|:--------------------------------------------------------| 
| memecached - CPU utilization | good up to 90% utilization then increase size of node or # of nodes|
| redis - CPU utilization | threshold: 90 / # of CPU cores; read heavy: read replicas; write heavy: larger cache instance | 
| evictions | memecached: larger instances and # of nodes; redis: redis more nodes |
| CurrConnections | likely an application problem with no closing connections |
| memecached swap | should be 0-50MB; increase node size; increase ConnectionOverhead (decrease memory for caching data) |

## Elastic Load Balancing Service
If you know there is a lot of traffic on the way, call AWS and get them to "pre-warm" your ELB.

| **Metric**  | **Solution**  |
|:-----------------------------------------|:--------------------------------------------------------| 
| HTTP Latency | AVG and MAX is useful; reported by AZ |
| BackendConnectionErrors | Look at SUM and difference between min and max values |
| SurgeQueueLength | Max in queue is 1024 |
| SpillOverCount | This is a bad, bad thing. Avoid. ELB reports a `503 - Service Unavailable` |
| UnHealthyHostCount | Just like what it says |

## Resources

### Documentation
[CloudWatch Dev Doc](https://aws.amazon.com/cloudwatch/developer-resources/)
[redis vs memecached](http://www.infoworld.com/article/3063161/application-development/why-redis-beats-memcached-for-caching.html)
