---
layout: post
title: "AWS SysOps - High Availability"
description: ""
category: posts
tags: [aws, sysops, highavailability]
---
{% include JB/setup %}

There are two major ideas in high avialability: Scalability and Elasticity. Elasticity is the ability to scale up and scale down based on demand so you only pay for what you need WHEN you need it. Scalability is the ability to response effectively to the  growth of volume in traffic and needs.

## Scaling out vs Scaling up
Horizonal scaling by adding more of the same sort of systems and balancing the load between them is implemented using auto-scaling. 
Auto-scaling is relatively complicated to setup but more importantly it requires a sessionless application or the ability to save session on the client or on a centralized session store. One big problem with auto-scaling is the delay in scaling up and scaling down although scheduled scaling is possible to mitigate the effects of delay in scaling up and down.

Scaling up, also called vertical scaling can not be automated easily. To resize and EC2 instance the AMI must have the same virtualization type; EBS volumes can help but must be suspended to make; you most likely need to suspend or reconfigure the autoscaling group.

# Application in AWS

## DynamoDB
Scalability - nothing to provision or do (provided the app in engineered correctly)
Elasticity - increase provisioned read and writes - this could be accomplished using scripts to decrease provisioned reads and writes as well

## EC2, Elasticache
Scalability - increase instance size, launch more instances
Elasticity - autoscaling

### EC2 Networking
Problems can arise when instances are in different AZ or regions, the instance does not provide proper bandwidth OR enhanced networking features are disabled.

`iperf3` is a great way to diagnose the problem

## RDS
I cover this material in [Data Management]({{ BASE_PATH }}/posts/aws-sysops-data-management)

## Bastion Hosts
White list the bastion host IP using an Elastic IP address. You might want to image your bastion host and spin it up when you need it or create an auto-scaling group of bastion hosts - perhaps an autoscaling group of one?

## Fixed IP Address Application
Lots of times apps are tied to a specific IP address making scalability challenging. In these cases, using an Elastic IP and assigning it to the instance can give you some good flexibility. Vertical scalability is likely the only option for these scenarios.

## Troubleshooting AutoScaling

The configuration is no longer valid: keypair has been deleted, the SG might have been deleted, autoscaling group not found, instance type not supported in the specified region, AZ not supported, invalid EBS mapping (attach an EBS block device to an instance store AMI)




