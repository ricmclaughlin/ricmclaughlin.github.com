---
layout: post
title: "AWS Sysops High Availability"
description: ""
category: posts
tags: [aws, sysopscert, highavailability]
---
{% include JB/setup %}

There are two major ideas in high avialability: Scalability and Elasticity. Elasticity is the ability to scale up and scale down based on demand so you only pay for what you need WHEN you need it. Scalability is the ability to response effectively to the  growth of volume in traffic and needs.

## Scaling out vs Scaling up
Horizonal scaling by adding more of the same sort of systems and balancing the load between them is implemented using auto-scaling. 
Auto-scaling is relatively complicated to setup but more importantly it requires a sessionless application or the ability to save session on the client or on a centralized session store. One big problem with auto-scaling is the delay in scaling up and scaling down although scheduled scaling is possible to mitigate the effects of delay in scaling up and down.

Scaling up, also called vertical scaling can not be automated easily. To resize and EC2 instance the AMI must have the same virtualization type; EBS volumes can help but must be suspended to make; you most likely need to suspend or reconfigure the autoscaling group.

## Application in AWS

### DynamoDB
Scalability - nothing to provision or do (provided the app in engineered correctly)
Elasticity - increase provisioned read and writes - this could be accomplished using scripts to decrease provisioned reads and writes as well

### EC2, Elasticache
Scalability - increase instance size, launch more instances
Elasticity - autoscaling

### RDS
A multi-AZ failover process is key in the event of an AZ failure. Failover can be enabled from the Console or via the API. Replication from one zone to another causes a lot of higher write and commit latency - this is a syncronous process - provisioned IOPS is recommended. Patching is another benefit of standy instances - patch the standby, failover, then patch the primary. Backups can be created from the standy instance and that can eliminate I/O locking and latency spikes from backups on the primary.

Failover is automatically triggers when the AZ or the underlying hardware fails or there is a manual reboot with failover is initialized. A slow server and corrupt data will NOT lead to a failover. Getting notification about a failover as you would expect - RDS events.

Failovers are implemented as a DNS change so the application servers have to re-establish connections with the failover instance.

### Bastion Hosts
White list the bastion host IP using an Elastic IP address. You might want to image your bastion host and spin it up when you need it or create an auto-scaling group of bastion hosts - perhaps an autoscaling group of one?

### Fixed IP Address Application
Lots of times apps are tied to a specific IP address making scalability challenging. In these cases, using an Elastic IP and assigning it to the instance can give you some good flexibility. Vertical scalability is likely the only option for these scenarios.

RDS Read Replicas
Auto backup must be enabled; this is done by turning the backup retention period to greater than 0; RDS takes a backup copy of the database then enable replication. Without multi-AZ operations, IO is suspended while RDS snapshots the database; must have InnoDB engine

no multi-az on read replica

replica lag






