---
layout: post
title: "AWS SysOps - Elastic Load Balancer"
description: ""
category: posts
tags: [aws, sysopscert, elb]
---
{% include JB/setup %}

## Elastic Load Balancers (ELB) 
ELB distributes traffic to instances that belong to the ELB group - this can be in a single region or multiple regions. One nifty features it that it allows us to offload SSL certs to load balancers instead of webservers! ELB configuration requires a protocol, a front end port, and a back end port. There are two version of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL) and Application Load Balancer (any port - just HTTP, HTTPS).

ELB are NOT free - there is a charge by the hour and per GB of usage.

Only one SSL Cert per ELB. The max number of requests that can be queued is 1024; it issues a `HTTP 503 Error` when it can process any more requests - call AWS if a huge traffic spike comes!

There are three `Cookie Stickiness`, also know as session affinity, with both enabled options end up with sticky sessions (so all sessions go back to the same server), options: 

* Disable Stickiness - The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance for session.

* Enable Load Balancer Generated Cookies - the ELB generates the cookie and manages the distribution of traffic;  also know as "duration based"; can set the duration of the distribution with zero seconds disabling cookie expiration

* Enable Application Generated Cookie Stickiness - ELB generates a cookie that correlates to the application cookie - the duration is set by the application cookie; name the cookie in the setting.

### Health Checks
To enable removing unhealthy instances from the round robin, each ELB can do a health check of the instances in the load balancing group. Can use different ports, including port 80, and set a response timeout, a health check interval, an unhealthy threshold and a healthy threshold.

### ELB Metrics
SurgeQueueLength - Length of waiting queue - closer to zero the better

SpilloverCount - How many requests are NOT serviced by the load balancer - closer to zero the better

Latency - How long a page takes to return

### External vs Internal Load Balancing
External = Public; Elastic IP; DNS
Internal = no public IP or Elastic IP; internal DNS name

### SSL on ELB
One of the key features of ELB is the ability to terminate SSL connections for instances in the load balancing group - SSL is still a highly compute intensive process for webservers. In this configuration, the HTTPS client uses port 443 to communicate with the ELB and the ELB communicates on port 80 to the web server instances in the autoscaling group. 

Managing the certificate on the ELB is always the magic... in fact, managing certs in general is the magic. There are three options. 

1. Use the new AWS Certificate Manager service. Especially cool now that it is integrated with Elastic Bean Stalk - nifty and FREE! 

2. Use a certificate from IAM. 

3. Upload your own.

## Autoscaling and ELB Problems
General Config Problems:

1. Attempting to create instance in the wrong zone, subnet or security group or with the wrong key pair. 

2. Instance is not supported in that AZ, region or is the wrong type for autoscaling.

3. The region is out of capacity.

3. There is an invalid EBS device mapping, or you are attaching a EBS block device to a instance store AMI.

### Dumbass Problems:

1. Autoscaling is not enabled on the account.

2. Autoscaling config is not working correctly. ie. there is a bug.

3. Autoscaling is in a "suspended state"

### You know the traffic is coming
Lots of times you will know there is more traffic a-coming. In these cases you can "pre-warm" the ELB by contacting AWS!


