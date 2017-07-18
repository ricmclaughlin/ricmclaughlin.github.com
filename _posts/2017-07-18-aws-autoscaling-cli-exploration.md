---
layout: post
title: "aws autoscaling cli exploration"
description: ""
category: projects
tags: [aws, autoscaling, aws-dev-ops-pro, aws-solutions-arch-pro, cli-exploration]
---
{% include JB/setup %}


This is a very simple exploration of [AWS Auto Scaling](https://aws.amazon.com/autoscaling/) from the [AWS CLI](https://aws.amazon.com/cli/). To get started with the CLI first [install it](http://docs.aws.amazon.com/cli/latest/userguide/installing.html), then use `aws configure` to [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for your account and region. 

There are numerous areas to explore with Auto Scaling and I'm doing my work in us-west-2.

## Launch Configurations

##### Examine all launch configurations

```bash
aws autoscaling describe-launch-configurations
```

##### Delete a Launch Configuration (by name)

```bash
aws autoscaling delete-launch-configuration --launch-configuration-name my-launch-config
```

####  Create Simple Launch Configuration

```bash
aws autoscaling create-launch-configuration --launch-configuration-name my-super-launch-config --image-id ami-835b4efa --instance-type g2.8xlarge

```
####  Create Simple Launch Configuration (with a rational instance size)

```bash
aws autoscaling create-launch-configuration --launch-configuration-name my-super-launch-config --image-id ami-835b4efa --instance-type t2.micro

```


####  Create Simple Launch Configuration (spot pricing)

```bash
aws autoscaling create-launch-configuration --launch-configuration-name my-spot-config --image-id ami-835b4efa --instance-type g2.8xlarge --spot-price "0.50"
```

####  Create a Complex Launch Configuration (with text file user data)

```bash
aws autoscaling create-launch-configuration --launch-configuration-name my-launch-config --key-name my-key-pair --security-groups sg-eb2af88e   --instance-monitoring Enabled=true --no-ebs-optimized --user-data file://myuserdata.txt --no-associate-public-ip-address --placement-tenancy dedicated --iam-instance-profile my-amazing-role --image-id ami-835b4efa --instance-type g2.8xlarge 
```


## Creating ASG

##### Examine all launch configurations

```bash
aws autoscaling describe-auto-scaling-groups
```

##### Delete a Launch Configuration (by name)

```bash
aws autoscaling  delete-auto-scaling-group --auto-scaling-group-name simple-asg
```

#### Create a simple ASG (single AZ)

```bash
aws autoscaling  create-auto-scaling-group --min-size 1 --max-size 2 --launch-configuration-name my-super-launch-config --auto-scaling-group-name simple-asg --vpc-zone-identifier subnet-08199041
```

## Modifying ASG

#### Adjust the size of the ASG... to very small

```bash
aws autoscaling  update-auto-scaling-group --min-size 0 --max-size 0  --auto-scaling-group-name simple-asg 
```

#### protect the new instances from scale-ing

```bash
aws autoscaling  update-auto-scaling-group --auto-scaling-group-name simple-asg --new-instances-protected-from-scale-in
```

## Termination policies

##### OldestInstance

Scenario: Change of instance size in launch configuration; replace the `OldestInstance` first

```bash
aws autoscaling  update-auto-scaling-group --auto-scaling-group-name simple-asg --termination-policies OldestInstance
```

##### OldestLaunchConfiguration

Scenario: Change in launch configuration so replace the `OldestLaunchConfiguration` first

```bash
aws autoscaling  update-auto-scaling-group --auto-scaling-group-name simple-asg --termination-policies OldestLaunchConfiguration
```

##### NewestInstance

Scenario: Test new launch configuration so replace the `NewestInstance` first

```bash
aws autoscaling  update-auto-scaling-group --auto-scaling-group-name simple-asg --termination-policies NewestInstance
```

## Scaling Policies

##### What Policies are There?

```bash
aws autoscaling describe-policies --auto-scaling-group-name simple-asg
```

#### Increment Capacity (postive scaling adjustment)

```bash
aws autoscaling put-scaling-policy --auto-scaling-group-name simple-asg --policy-name ScaleOut --scaling-adjustment 1 --adjustment-type ChangeInCapacity
```

#### Decrement Capacity (negative scaling adjustment)

```bash
aws autoscaling put-scaling-policy --auto-scaling-group-name simple-asg --policy-name ScaleIn --scaling-adjustment -1 --adjustment-type ChangeInCapacity
```

#### Percentage Change

```bash
aws autoscaling put-scaling-policy --auto-scaling-group-name simple-asg --policy-name ScalePercentChange --scaling-adjustment 25 --adjustment-type PercentChangeInCapacity --cooldown 60 --min-adjustment-step 2
```

## Lifecycle Hooks

Scenario: Create a Launching lifecycle hook, send a heart beat, abandon

##### What Lifecycle Hook are There?

```bash
aws autoscaling describe-lifecycle-hooks --auto-scaling-group-name simple-asg
```

##### What Types of Lifecycle Hook are There?

```bash
aws autoscaling describe-lifecycle-hooks --auto-scaling-group-name simple-asg
--------------------------------------------
|        DescribeLifecycleHookTypes        |
+------------------------------------------+
||           LifecycleHookTypes           ||
|+----------------------------------------+|
||  autoscaling:EC2_INSTANCE_LAUNCHING    ||
||  autoscaling:EC2_INSTANCE_TERMINATING  ||
|+----------------------------------------+|

```

#### Create a Launching Hook

Note: The role use here should use the "AutoScaling Notification Access" type and the `AutoScalingNotificationAccessRole` policy.

```bash
aws autoscaling put-lifecycle-hook --lifecycle-hook-name my-lifecycle-hook --auto-scaling-group-name simple-asg --lifecycle-transition autoscaling:EC2_INSTANCE_LAUNCHING \
--notification-target-arn arn:aws:sns:us-west-2:100935286947:MySNSTopic \
--role-arn arn:aws:iam::100935286947:role/AutoScalingSNSRole
```

#### Trigger Launch

```bash
aws autoscaling  update-auto-scaling-group --min-size 1 --max-size 1  --auto-scaling-group-name simple-asg 
```

#### Send Heartbeat

```bash
aws autoscaling  record-lifecycle-action-heartbeat --auto-scaling-group-name simple-asg \
--lifecycle-hook-name my-lifecycle-hook \
--instance-id i-01f73ebbbf91426d6
```

#### Abandon

```bash
aws autoscaling complete-lifecycle-action --auto-scaling-group-name simple-asg \
--lifecycle-hook-name my-lifecycle-hook \
--lifecycle-action-result ABANDON \
--instance-id i-01f73ebbbf91426d6
```


#### Complete
```bash
aws autoscaling complete-lifecycle-action --auto-scaling-group-name simple-asg \
--lifecycle-hook-name my-lifecycle-hook \
--lifecycle-action-result CONTINUE \
--instance-id i-0b563cfeb586c3cec
```


## Standby

#### Enter Standby

Actually, given the min size of 0 on the ASG this won't totally work... but would on an ASG with of a larger size...

```bash
aws autoscaling enter-standby --auto-scaling-group-name simple-asg \
--should-decrement-desired-capacity \
--instance-id i-0b563cfeb586c3cec
```



#### Exit Standby

```bash
aws autoscaling exit-standby --auto-scaling-group-name simple-asg \
--instance-id i-0b563cfeb586c3cec
```

## Kill ASG

First, kill instance(s)..

```bash
aws autoscaling terminate-instance-in-auto-scaling-group  \
--instance-id i-0b563cfeb586c3cec \
--no-should-decrement-desired-capacity
```

then, kill the group:

```bash
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name simple-asg
```



