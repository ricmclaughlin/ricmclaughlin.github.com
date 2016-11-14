---
layout: post
title: "AWS Solutions Arch - Troublshooting"
description: ""
category: posts 
tags: [aws, ec2, solutionsarch]
---
{% include JB/setup %}

# EC2

### 60 new EC2 instances in az? 
soft limit of 20 EC2 instances per region; increase form and retry the template after your limit has been increased.



### Can't connect

* Security group port not open

* no IG on VPN; no route

* no public IP (EIP or public)

* no NAT; NAT has packet forwarding disabled

### Can't connect EBS

* EBS volume in wrong AZ

* EBS volume in wrong 

### Can't create more instances

* Maxed out # of instances in VPC

### AMI unavailable

* AMIs are region specific; renamed when copied between regions

### Slow
* Increase instance size

* GP to IOPS based storage

### Placement group capacity error

* Stop and start all instances

# ELB

### Load balancing not working between zones

* Enable Cross-Zone Load Balancing

### Instances not being added to ELB

* Health check to long - could be # of checks or time between checks

* Configure auto-assign public IP address

### Traffic not reaching instances

* Port mismatch 

* ELB SG is misconfigured

### EC2 Access logs show ELB instance IP

* Config ELB to forward source IP to instance

# Auto Scaling

#### Instances start and stop

* Scaling policy misconfigured - longer scale down threshold

Modify the Auto Scaling group cool-down timers & modify the Amazon CloudWatch alarm period that triggers your Auto Scaling scale down policy.
