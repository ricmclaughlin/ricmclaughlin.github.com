---
layout: post
title: "AWS - CloudWatch CLI Exploration"
description: ""
category: projects
tags: [aws, cloudwatch, aws-dev-ops-pro, aws-solutions-arch-pro, cli-exploration]
---
{% include JB/setup %}

This is a very simple exploration of [AWS CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) from the [AWS CLI](https://aws.amazon.com/cli/). To get started with the CLI first [install it](http://docs.aws.amazon.com/cli/latest/userguide/installing.html), then use `aws configure` to [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for your account and region. 

There are numerous areas to explore with CloudWatch and I'm doing my work in us-west-2.



aws cloudwatch list-metrics

## Alarms

##### Did it work? What is out there?

```bash
aws cloudwatch describe-alarms
```

#### Create Alarm

```bash
aws cloudwatch put-metric-alarm --alarm-name MyCPUAlarm --alarm-description "Alarm when CPU exceeds 70 percent" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 70 --comparison-operator GreaterThanThreshold  --dimensions "Name=InstanceId,Value=i-00b9f133ae1a5e033" --evaluation-periods 2 --alarm-actions arn:aws:sns:us-west-2:100935286947:MySNSTopic --unit Percent
```

#### Test Fire an Alarm

```bash
aws cloudwatch 
```

#### Disable/Enable Alarm

Note: Alarm state will still change but alarms won't fire

```bash
aws cloudwatch disable-alarm-actions --alarm-names MyCPUAlarm
```

```bash
aws cloudwatch enable-alarm-action --alarm-names MyCPUAlarm
```

## Metrics

##### What metrics do I have?

```bash
aws cloudwatch list-metrics
```

#### Get the Metric

```bash
aws cloudwatch get-metric-statistics --metric-name CPUUtilization --namespace AWS/EC2 --dimensions "Name=InstanceId,Value=i-00b9f133ae1a5e033" --statistics Maximum --start-time 2014-04-08T23:18:00 --end-time 2014-04-09T23:18:00 --period 3600
```

## Scenario

sourced from [Scenario: Publish Metrics to CloudWatch](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/PublishMetrics.html)

#### Add lots of data

Note: If the metric does not exist CloudWatch creates it

```bash
aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
--timestamp 2017-07-14T20:30:00Z --value 87 --unit Milliseconds
aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
--timestamp 2017-07-14T20:30:00Z --value 51 --unit Milliseconds
aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
--timestamp 2017-07-14T20:30:00Z --value 125 --unit Milliseconds
aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
--timestamp 2017-07-14T20:30:00Z --value 235 --unit Milliseconds

aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
--timestamp 2017-07-14T21:30:00Z --statistic-values Sum=577,Minimum=65,Maximum=189,SampleCount=5 --unit Milliseconds

aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
--statistic-values Sum=806,Minimum=47,Maximum=328,SampleCount=6 --unit Milliseconds
```

#### Get Metric

```bash
aws cloudwatch get-metric-statistics --namespace GetStarted --metric-name RequestLatency --statistics Average \
--start-time 2017-07-13T00:00:00Z --end-time 2017-07-17T00:00:00Z --period 60

```


