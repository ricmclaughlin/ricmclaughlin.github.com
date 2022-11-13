---
layout: post
title: "AWS - Auto Scaling"
description: ""
category: posts
tags: [autoscaling, aws, devops, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Overall

An [Auto Scaling Group ](http://docs.aws.amazon.com/autoscaling/latest/userguide/AutoScalingGroup.html) gives you the ability to manage a group of instances as a unit. 

## Launch Configurations

First, lots of basic stuff... an ASG must specify a launch configuration and only one launch config per ASG. A Launch Config can't be modified after creation.

Launch configs can be made from running EC2 instance but some storage and monitoring settings are not supported. Tags, block devices settings, userdata, and load balancer settings including the `LoadBalancerNames` attribute are not copied over to the Launch Configuration. 

Spot instances can be used in an ASG but must include a bid price. ASG will balance across AZ using bid prices. Spot instances can't use the same launch config as on-demand instances because they include a bid price.

# Scaling

## Scaling Plans

There are ~~four~~ five types of scaling plans

0. Maintain current instance Levels - (Auto Scaling Self Healing) the use cases where a single server is needed, and needed all the time, the ability to create an ASG of one is useful.

0. Manual Scaling - manually change max, min, and desired capacity

0. Scheduled Scaling - at a time, the scaling group will do something... cooldown periods are not supported; can't have more than one action happen at a specific time.

0. Dynamic Scaling - scale based changing demand using alarms and scaling policies. At a minimium you are going to need at least 2 scaling plans. 

0. Predictive Scaling - EC2 Auto Scaling can work in conjunction with AWS Auto Scaling to reactively and proactively scale stuff.

###  Manual Scaling

Simply put, this is when you change the max, min and desired capacity of the group using the CLI or console.

### Dynamic Scaling

Scaling policies implement dynamic scaling. Alarms fire the scaling policies. Scaling adjustment types change the capacity of an Auto Scaling group using a `ScalingAdjustment`. There are three different adjustment types:

- `ChangeInCapacity` - increment or de-increment by a number of instances

- `ExactCapacity` - set a new capacity explicitly

- `PercentChangeInCapacity` - percentage change

#### Scaling Policy Types

Simple Scaling - single scaling adjustment; have cooldown; default of 300 seconds

Step Scaling - scale based on size of alarm breach; no cooldown; don't lock group; continuously evaluated; instance warmup

#### ASG Instance Lifecycle &amp; Lifecycle Hooks

The two main states for instances are:

- `InService` - when the instance is happiest; the instance gets to this state via a "scale out" - Whenever an ASG starts to scale out the instances moves from the `Pending` state through the EC2_INSTANCE_LAUNCHING hook `Pending: Wait` state and `Pending:Proceed` state then to the `InService` state.

- `Terminated` - when the instance is gone... the instance gets to this state via a "scale in" (or Health Check Failure) - Whenever an ASG starts to scale in the instance moves from the `InService` to `Terminating` then into the EC2_INSTANCE_TERMINATING hook `Terminating: Wait` and `Terminating: Proceed` and finally to `Terminated`

While instances are `Pending` to be added to an ASG or `Terminating` out of an ASG there is an opportunity to add a hook into the processes. These hooks can be a CloudWatch event, an SNS or SQS message or launches a script.

The wait state is how long these hooks have to run before proceeding to the lifecycle stage. By default the wait state is 3600 seconds and can changed using the `heartbeat-timeout` parameter or be ended using the `complete-lifecycle-action` or lengthened using `record-lifecycle-action-heartbeat` commands. The max wait state is 48 hours.

At the end of the lifecycle the state will be `ABANDON` or `CONTINUE`. The ASG auto scaling cooldown does not start until the instance enters the InService state. In the case of Simple Scaling, the cooldown period does not begin until after the instance moves out of the wait state or in the case of spot instances the cooldown period begins when the bid is successful. Lifecycle Hooks can be used with Spot Instances but this does NOT prevent an instance from terminating.

| API      | Purpose       |
|----------|---------------|
| `put-lifecycle-hook` | create hook |
| `complete-lifecycle-action` | complete the hook with `ABANDON` or `CONTINUE` |
| `record-lifecycle-action-heartbeat` | amount of additional time to extend the lifecycle hook timeout|

There are also two other states:

- Stand By - If you need to trouble shoot or work with an instance, but still want it managed by the ASG then the instance goes from InService to EnteringStandby to Standby. When returned to service, the instance goes through the `EC2_INSTANCE_LAUNCHING` hook process. By default, Auto Scaling decrements the desired capacity of your Auto Scaling group when you put an instance on standby and increments your desired capacity when you add instances and does NOT perform health checks on it.

- Detach - If you need to remove an instance from InService state and remove it from the ASG you call `DetachInstances` then the service goes to `Detaching` through the `Detached` state and ends up an EC2 instance. Any instance can be attached to the ASG using `AttachInstances`.

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

## Monitoring and Controlling

### AutoScaling Metrics

| metric      | Purpose       |
|--------------------|---------------|
| `GroupMinSize` | min size |
| `GroupMaxSize` | max size |
| `GroupDesiredCapacity` | attempts to maintain this number  |
| `GroupInServiceInstances` | in-service and not pending or terminating  |
| `GroupPendingInstances` | pending instances |
| `GroupTerminatingInstance` | terminating instances  |
| `GroupStandbyInstances` | instances in stand-by state  |
| `GroupTotalInstances` | in-service, pending, &amp; terminating  |

### Health Checks

There are three types of [health checks](http://docs.aws.amazon.com/autoscaling/latest/userguide/healthcheck.html) that can be preformed in an ASG. Unhealthy instances are terminated almost immediately so never recover to healthy typically or be in a situation where `SetInstanceHealth` can set them to healthy. EIP and EBS volumes are detached and not re-attached to new instances.

Status checks - good old fashioned system &amp; instance status checks

ELB Health checks - the ELB/ALB will check the health of the instance; if an instance is in more than one ELB, all must report healthy else instance gets replaced.

Custom Health Checks - based on a check from within the instance send a message to the ASG using the 'set-instance-health' command; Not sure why it works this way it should be done at the ELB/ALB level.

## Troubleshooting

Least efficient way of solving problems? raise minimum number of instances






