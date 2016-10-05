---
layout: post
title: "AWS SysOps - Elastic Load Balancer"
description: ""
category: posts
tags: [aws, sysopscert, elb]
---
{% include JB/setup %}

## Elastic Load Balancers (ELB) 
ELB distributes traffic to instances that belong to the ELB group; allows us to offload SSL certs to load balancers instead of webservers! ELB configuration requires a protocol, a front end port, and a back end port. There are two version of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL) and Application Load Balancer (any port - just HTTP, HTTPS).

ELB are NOT free - there is a charge by the hour and per GB of usage.

Only one SSL Cert per ELB. The max number of requests that can be queued is 1024; it issues a `HTTP 503 Error` when it can process any more requests - call AWS if a huge traffic spike comes!

There are three `Cookie Stickiness`, with both enabled options end up with sticky sessions (so all sessions go back to the same server), options: 

* Disable Stickiness - The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance for session.

* Enable Load Balancer Generated Cookies - the ELB generates the cookie and manages the distribution of traffic; can't set the duration of the distribution

* Enable Application Generated Cookie Stickiness - ELB generates a cookie that correlates to the application cookie.  

### External vs Internal Load Balancing
External = Public; Elastic IP; DNS
Internal = no public IP or Elastic IP; internal DNS name

### SSL on ELB



## Autoscaling and ELB Problems
General Config Problems:

1. Attempting to create instance in the wrong zone, subnet or security group or with the wrong key pair. 

2. Instance is not supported in that AZ, region or is the wrong type for autoscaling.

3. The region is out of capacity.

3. There is an invalid EBS device mapping, or you are attaching a EBS block device to a instance store AMI.

Dumbass Problems:

1. Autoscaling is not enabled on the account.

2. Autoscaling config is not working correctly. ie. there is a bug.

3. Autoscaling is in a "suspended state"


