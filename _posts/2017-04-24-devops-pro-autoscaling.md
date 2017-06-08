---
layout: post
title: "DevOps Pro - Autoscaling"
description: ""
category: posts
tags: [autoscaling, devopspro]
---
{% include JB/setup %}

An [Auto Scaling Group ](http://docs.aws.amazon.com/autoscaling/latest/userguide/AutoScalingGroup.html) gives you the ability to manage a group of instances as a unit. 

## Auto Scaling Self Healing

In use cases where a single server is needed, and needed all the time, the ability to create an ASG of one is useful.

## ASG Launch Configuration

First, lots of basic stuff... an ASG must specify a launch configuration and only one launch config per ASG. A Launch Config can't be modified after creation.

Launch configs can be made from running EC2 instance but some storage and monitoring settings are not supported. Tags, block devices settings, and load balancer settings including the ```LoadBalancerNames``` attribute is not copied over to the Launch Configuration. Spot instances can't use the same launch config as on-demand instances because they include a bid price.

## ASG Instance Lifecycle

### Lifecycle Hooks

While instances are ```Pending``` to be added to an ASG or ```Terminating``` out of an ASG there is an opportunity to add a hook into the processes. These hooks can be a CloudWatch event, an SNS or SQS message or launches a script. 

The wait state is how long these hooks have to run before proceeding to the lifecycle stage. By default the wait state is 3600 seconds and can changed using the ```heartbeat-timeout``` parameter or be ended using the ```complete-lifecycle-action``` or lengthened using ```record-lifecycle-action-heartbeat``` commands. The max wait state is 48 hours.

At the end of the lifecycle the state will be ```ABANDON``` or ```CONTINUE```. The ASG auto scaling cooldown does not start until the instance enters the InService state.

Lifecycle Hooks can be used with Spot Instances but this does NOT prevent an instance from terminating.

### Scale Out 

Whenever an ASG starts to scale out the instances moves from the Pending state through the EC2_INSTANCE_LAUNCHING hook ```Pending: Wait``` state and ```Pending: Proceed``` state then to the ```InService``` state.

### Scale In (or Health Check Failure)

Whenever an ASG starts to scale in the instance moves from the ```InService``` to ```Terminating``` then into the EC2_INSTANCE_TERMINATING hook ```Terminating: Wait``` and ```Terminating: Proceed``` and finally to ```Terminated```

### Stand By

If you need to trouble shoot or work with an instance, but still want it managed by the ASG then the instance goes from InService to EnteringStandby to Standby. When returned to service the instance goes through the EC2_INSTANCE_LAUNCHING hook process.

### DetachInstances

If you need to remove an instance from InService state you call ```DetachInstances``` then the service goes to Detaching through the Detached state and ends up an EC2 instance. Any instance can be attached to the ASG using ```AttachInstances```.

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

### AutoScaling Metrics

GroupMinSize
GroupMaxSize
GroupDesired

GroupInServiceInstances
GroupPendingInstances
GroupTerminatingInstance
GroupStandbyInstances
GroupTotalInstances

## Scaling Policies

These little guys change the capacity of an Auto Scaling group using a ScalingAdjustment. There are three different adjustment types:

- ChangeInCapacity - increment or de-increment by a nymber of instances

- ExactCapacity - set a new capacity explicitly

- PercentChangeInCapacity - percentage change

### Scaling Policy Types

Simple Scaling - single scaling adjustment; have cooldown; 

Step Scaling - scale based on size of alarm breach; no cooldown; don't lock group; continuously evaluated; instance warmup

## Auto Scaling Group Properties

## Autoscaling API

| API      | Purpose       |
|----------|---------------|
| ```enter-standby``` | Pause the instance for maintenance  |
| ```exit-standby``` | Unpause the instance for maintenance      |
| ```create-launch-configuration``` | create |
| ```delete-launch-configuration``` | delete (there is no modify) |
| ```update-auto-scaling-group``` | Update ASG |
| ```put-lifecycle-hook``` | create hook |
| ```put-scaling-policy``` | create scaling policy |







