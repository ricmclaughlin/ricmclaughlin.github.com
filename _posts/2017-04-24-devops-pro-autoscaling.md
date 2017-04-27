---
layout: post
title: "DevOps Pro - Autoscaling"
description: ""
category: posts
tags: [autoscaling, devopspro]
---
{% include JB/setup %}

## Overview

### ASG Lifecycle Hook

While instances are ```Pending``` to be added to an ASG or ```Terminating``` out of an ASG there is an opportunity to add a hook into the processes. These hooks can be a CloudWatch event, an SNS or SQS message or launches a script. The wait state is how long these hooks have to run before proceeding to the lifecycle stage.

#### Wait State

By default the wait state is 3600 seconds but can be shortened using the ```complete-lifecycle-action``` or lengthened using ```record-lifecycle-action-heartbeat``` commands. 

### ASG Termination Policies

ASG (Termination Policies)[http://docs.aws.amazon.com/autoscaling/latest/userguide/as-instance-termination.html] determine which instances should be terminated when a scale-in event occurs. In the console, they get setup in the ASG configuration setting and get executed in presentation order. When a scale-in event occurs the AutoScaling feature checks for an imbalance of instances across AZ then runs the termination policy. Some use cases for each type of termination policy includes:

- OldestInstance - gradually replace an old instance type

- NewestInstance - test out a new launch configuration

- OldestLaunchConfiguration - phase out old config

- ClosestToNextInstanceHour - save money

- Default - why add anything about default??? just because!

#### Scale-in Protection

Scale-in protection can be applied to an ASG or an instance in the ASG and starts once the instance is ```InService```. Instance protection does not prevent termination in the following cases:

- Manual Termination

- Health Check Replacement

- Spot instance interuption

### Suspending and Resuming AS Processes

There are numerous AS processes that can be suspended. Generally, suspending AS processes is used as a debugging activity.

- Launch - Suspending the launch process disrupts 

- Terminate - scale in no longer happens when this is suspended

- HealthCheck

- ReplaceUnhealthy - uses the terminate and launch processes...

- AZRebalance

- AlarmNotification - suspends the ability to execute policies triggered by alarms

- ScheduledActions - suspends timed scaling actions

- AddToLoadBalancer - Don't add new instance to the load balancer... useful for testing instances in the ASG before traffic gets to them; when re-enabled instances do NOT get added to the load balancer automatically

### Custom Health Checks

Using the API a skilled developer can create a custom health check that might give a more accurate picture of instance health.
