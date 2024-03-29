---
layout: post
title: "AWS - EC2 Auto Scaling"
description: ""
category: posts
tags: [compute, devops, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Overall
An [Auto Scaling Group ](http://docs.aws.amazon.com/autoscaling/latest/userguide/AutoScalingGroup.html) gives you the ability to manage a group of instances as a unit. 

## ~~Launch Configurations~~ Launch Templates
ASG used to use Launch Configurations which have one version and limited set of features. A [Launch Template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/launch-templates.html) enables all EC2 instance configuration parameters (like a mix of Spot and On-Demand instances) and parent child like relationship between templates, although they don't call it that.

# Scaling
## Scaling Options
There are four types of scaling plans:
0. Manual Scaling - manually change max, min, and desired capacity
0. Scheduled Scaling - at a time, the scaling group will do something... cooldown periods are not supported; can't have more than one action happen at a specific time.
0. Dynamic Scaling - scale based changing demand using alarms and scaling policies. At a minimum you are going to need at least 2 scaling plans. 
0. Predictive Scaling - EC2 Auto Scaling can work in conjunction with AWS AutoScaling to reactively and proactively scale stuff.

### Dynamic Scaling
Scaling policies implement dynamic scaling. Alarms fire the scaling policies. Scaling adjustment types change the capacity of an Auto Scaling group using a `ScalingAdjustment`. There are three different adjustment types:
- `ChangeInCapacity` - increment or de-increment by a number of instances
- `ExactCapacity` - set a new capacity explicitly
- `PercentChangeInCapacity` - percentage change

#### Dynamic Scaling Policy Types
Simple Scaling - single scaling adjustment; have cooldown; default of 300 seconds
Step Scaling - scale based on size of alarm breach; no cooldown; don't lock group; continuously evaluated; instance warmup
Target target - set capacity based on a single metric - the best metrics are `CPUUtilization`, `Average Network In/Out`, and `RequestCountPerTarget` 

### Update Policies
Although a [Cloudformation feature](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-updatepolicy.html), Update Policies applies to how CloudFormation deals with a Launch Configuration change, VPC change or changes to an ASG that contains instances that don't match the current launch configuration. There are two options:
- `AutoScalingReplacingUpdate` - if `WillReplace` is true the ASG and its instance are replaced and go in service when the `MinSuccessfulInstancesPercent` of the creation policy is met; until then the old one stays around
- `AutoScalingRollingUpdate` - which sets batch sizes or all-at-once configuration with a minimum number of instances online; use the `SuspendProcesses` property to keep the ASG from running during update.
Closely related is the `AutoScalingScheduledAction` which is for updating a group that has scheduled actions.

### Termination Policies
ASG [Termination Policies](http://docs.aws.amazon.com/autoscaling/latest/userguide/as-instance-termination.html#custom-termination-policy) determine which instances should be terminated when a scale-in event occurs. In the console, they get setup in the ASG configuration setting and get executed in presentation order. When a scale-in event occurs the AutoScaling feature checks for an imbalance of instances across AZ **then** runs the termination policy. Some use cases for each type of termination policy includes:
- OldestInstance - gradually update instances with a new instance type
- NewestInstance - test out a new launch configuration
- OldestLaunchConfiguration - phase out old config
- ClosestToNextInstanceHour - save money
- Default - balances across AZs then oldest launch config then closet to billing hour

#### Scale-in Protection
Scale-in protection can be applied to an ASG or an instance in the ASG and starts once the instance is `InService` and this keeps the instance from being terminated during scaling operations. Detaching instances loose their scale-in protection. Instance protection does not prevent termination in the following cases:
- Manual termination
- Health check replacement
- Spot instance interruption

### Lifecycle Processes
There are numerous AS processes that can be suspended. Generally, suspending AS processes is used as a debugging activity.
- Launch - Suspending the launch process disrupts 
- Terminate - scale in no longer happens when this is suspended
- HealthCheck
- ReplaceUnhealthy - uses the terminate and launch processes...
- AZRebalance
- AlarmNotification - suspends the ability to execute policies triggered by alarms
- ScheduledActions - suspends timed scaling actions
- AddToLoadBalancer - adds instances to load balancer/target group
- InstanceRefresh - new thing to update autoscaling group instances with new template config

### Health Checks
There are three types of [health checks](http://docs.aws.amazon.com/autoscaling/latest/userguide/healthcheck.html) that can be preformed in an ASG. Unhealthy instances are terminated almost immediately so never recover to healthy typically or be in a situation where `SetInstanceHealth` can set them to healthy. EIP and EBS volumes are detached and not re-attached to new instances.
- Status checks - good old fashioned system (host) &amp; instance (vm) status checks
- ELB Health checks - the ELB/ALB will check the health of the instance; if an instance is in more than one ELB, all must report healthy else instance gets replaced.
- Custom Health Checks - based on a check from within the instance send a message to the ASG using the 'set-instance-health' command; Not sure why it works this way it should be done at the ELB/ALB level.

## Triage
- Upgrade AMI? update launch configuration/template and terminate manually OR EC2 Instance Refresh for Auto Scaling (create new launch template and specify min % healthy and warm-up time)
- Perform action during autoscaling? Lifecycle hooks
- Load balance between two ELB? Route53 weighted record (slow)
- Decrease time between Launch lifecyle and HealthCheck OK? Prebake AMI