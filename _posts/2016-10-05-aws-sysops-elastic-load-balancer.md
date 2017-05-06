---
layout: post
title: "AWS SysOps - Elastic Load Balancer"
description: ""
category: posts
tags: [aws, sysops, elb, solutionsarch, devopspro]
---
{% include JB/setup %}

## Elastic Load Balancers (ELB) 
ELB distributes traffic to instances that belong to the ELB group - this can be in a single region or multiple regions. One nifty features it that it allows us to offload SSL certs to load balancers instead of webservers! ELB configuration requires a protocol, a front end port, and a back end port. There are two version of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL) and Application Load Balancer (any port - just HTTP, HTTPS). ELBs do not have a defined IPV4 address.

ELB are NOT free - there is a charge by the hour and per GB of usage.

Only one SSL Cert per ELB. The max number of requests that can be queued is 1024; it issues a `HTTP 503 Error` when it can process any more requests - call AWS if a huge traffic spike comes!

### Cookie Stickiness

There are three `Cookie Stickiness`, also know as session affinity, with both enabled options end up with sticky sessions (so all sessions go back to the same server), options: 

* Disable Stickiness - The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance for session.

* Enable Load Balancer Generated Cookies - the ELB generates the cookie and manages the distribution of traffic;  also know as "duration based"; can set the duration of the distribution with zero seconds disabling cookie expiration

* Enable Application Generated Cookie Stickiness - ELB generates a cookie that correlates to the application cookie - the duration is set by the application cookie; name the cookie in the setting.

### Health Checks

To enable removing unhealthy instances from the round robin, each ELB can do a health check of the instances in the load balancing group. Can use different ports, including port 80, and set a response timeout, a health check interval, an unhealthy threshold and a healthy threshold.

### ELB Metrics (not ALB)

These metrics are measured and sent through every minute. There are two useful dimensions the AvailabilityZone Dimension and the LoadBalancerName Dimension.

* SurgeQueueLength - Length of waiting queue - closer to zero the better (up to 1024)

* SpilloverCount - How many requests are NOT serviced by the load balancer in EXCESS of the QueueLength - closer to zero the better

* Latency - How long a page takes to return

* BackendConnectionErrors - unsuccessful connection to the backend

* HealthyHostCount, UnHealthyHostCount

* HTTPCode_Backend_XXX - response codes from the backend (2XX, 3XX, 4XX)

* HTTPCode_ELB_4XX - malformed on incomplete client requests

* HTTPCode_ELB_5XX - no healthy backend instance or request rate too high

* RequestCount - # of requests over 1 or 5 minute interval

### External vs Internal Load Balancing

Lots of differences between externally and internally facing load balancers:

- External = Public; Elastic IP; DNS

- Internal = no public IP or Elastic IP; internal DNS name

### SSL on ELB
One of the key features of ELB is the ability to terminate SSL connections for instances in the load balancing group - SSL is still a highly compute intensive process for webservers. In this configuration, the HTTPS client uses port 443 to communicate with the ELB and the ELB communicates on port 80 to the web server instances in the autoscaling group. 

Managing the certificate on the ELB is always the magic... In fact, managing certs in general is the magic. There are three options. 

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

## Logging

Strangely [ELB Logging](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/access-log-collection.html) is disabled by default and are generated on a best-case situation so data might be missing; logs are stored in S3 and taken either every hour OR ever 5 minutes.

The log file name includes the end-time, ip address of the ELB and a random generated string. The logs themselves include a timestamp, client:port, backend:port, user_agent and the important:

- request_processing_time - 
  * for HTTP load balancer = complete request -> send to instance 
  * for TCP load balancer = TCP connect -> first byte to instance

- backend_processing_time
  * for HTTP load balancer = completed send to backend server -> start of response 
  * for TCP load balancer = time until connect to backend server

- response_processing_time
  * for HTTP load balancer = start of reponse head -> start of send response to client
  * for TCP load balancer = time until first byte from instance started sending reponse to client

- Request - including verb, protocol version (http 1.1 or 2.0)

## Listeners

Listeners define what ports and protocols the ELB/ALB listens to. 

| Thingy | ELB   |      ALB      |
|-----------------|:-------------:|:------:|
| HTTP & HTTP |  Yes | Yes |
| TCP/SSL |    Yes   |   No |
| Layer 4 | Yes |    No |
| Layer 7 | No |    Yes |
| Websockets | No |    Yes |
| Path Routing | No |    Yes |
| Host Routiner | No |    Yes |

ALB can also forward X-Forwarded-For header so logging can occur at the instance layer NOT the ALB (because ALB routing is best effort no complete). To accomplish the same thing with an ELB you need to enable the [Proxy Protocol Headers](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-proxy-protocol.html) which adds a header for the backend to parse but this does not enable sticky session or X-Forward-For header.

## End to End Encryption

Create a public key policy
Create a back-end instance authentication policy



ELB can 