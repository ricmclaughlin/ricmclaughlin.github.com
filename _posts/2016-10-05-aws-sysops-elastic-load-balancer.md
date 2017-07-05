---
layout: post
title: "AWS SysOps - Elastic Load Balancer"
description: ""
category: posts
tags: [aws, sysops, elb, solutionsarch, devopspro]
---
{% include JB/setup %}

# Elastic Load Balancers (ELB) 

ELB distributes traffic to instances that belong to the ELB group - this can be in a single region or multiple regions. One nifty features it that it allows us to offload SSL certs to load balancers instead of webservers! ELB configuration requires a protocol, a front end port, and a back end port. ELB are NOT free - there is a charge by the hour and per GB of usage. Only one SSL Cert per ELB. The max number of requests that can be queued is 1024, can only use one subnet per AZ; can use up to 5 Security groups and also use deletion protection.

There are two version of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL; layer 4) and Application Load Balancer (any port - just HTTP, HTTPS; layer 7). ELBs do not have a defined IPV4 address.

Lots of differences between externally and internally facing load balancers:

- External = must be placed in a public subnet; Elastic IP; external DNS

- Internal = no public IP or Elastic IP; internal DNS name

Lots of times you will know there is more traffic a-coming. In these cases you can "pre-warm" the ELB by contacting AWS!

## SSL on ELB

One of the key features of ELB is the ability to terminate SSL connections for instances in the load balancing group - SSL is still a highly compute intensive process for webservers. In this configuration, the HTTPS client uses port 443 to communicate with the ELB and the ELB communicates on port 80 to the web server instances in the autoscaling group. 

Managing the certificate on the ELB is always the magic... In fact, managing certs in general is the magic. There are three options. 

1. Use the new AWS Certificate Manager service. Especially cool now that it is integrated with Elastic Bean Stalk - nifty and FREE! 

2. Use a certificate from IAM. 

3. Upload your own.

In addition to a cert, you need to define an SSL Negotiation configuration, called a security policy, which is a combination of SSL protocols, SSL ciphers, and the Server Order Preference option.

## Health Checks

To enable removing unhealthy instances from the round robin, each ELB can do a health check of the instances in the load balancing group. Can use different ports, including port 80, and set a response timeout, a health check interval, an unhealthy threshold and a healthy threshold. 

## Monitoring

### Metrics

These metrics are measured and sent through every minute. There are two useful dimensions the AvailabilityZone and LoadBalancerName for the ELB and the the ALB adds a `TargetGroup` dimension.

* SurgeQueueLength - Length of waiting queue - closer to zero the better (up to 1024)

* SpilloverCount - How many requests are NOT serviced by the load balancer in EXCESS of the QueueLength - closer to zero the better

* Latency - How long a page takes to return

* BackendConnectionErrors - unsuccessful connection to the backend

* HealthyHostCount, UnHealthyHostCount

* HTTPCode_Backend_XXX - response codes from the backend (2XX, 3XX, 4XX)

* HTTPCode_ELB_4XX - malformed on incomplete client requests

* HTTPCode_ELB_5XX - no healthy backend instance or request rate too high

* RequestCount - # of requests over 1 or 5 minute interval

### Logging

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

ALB can also forward X-Forwarded-For header so logging can occur at the instance layer NOT the ALB (because ALB routing is best effort no complete). To accomplish the same thing with an ELB you need to enable the [Proxy Protocol Headers](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-proxy-protocol.html) which adds a header for the backend to parse but this does not enable sticky session or X-Forward-For header.

## Differences between ELB &amp; ALB

Listeners define what ports and protocols the ELB/ALB listens to. Which is supported is a big difference between ELB/ALB 

| Thingy | ELB   |      ALB      |
|-----------------|:-------------:|:------:|
| HTTP & HTTP | Yes | Yes |
| TCP/SSL | Yes | No |
| Layer? | 4 | 7 |
| Layer 7 | No | Yes |
| Websockets | No | Yes |
| Path Routing | No | Yes |
| Host Routing | No | Yes |
| Cookie Stickiness | Yes | ELB Cookies only (called AWSALB) |
| Health Check | TCP, ICMP, HTTP, HTTPS| HTTP, HTTPS |

# ALB

Path based routing & management - because the routing is based on URL matching target groups can be containers, different port on the same box or simply different target groups. The enables the ability to manage each target group individually.

Target groups are EC2 instances or containers managed as an entity and checks the health of the group automatically.

## Listeners

ALB listeners supports HTTP & HTTPS only. Rules determine where the traffic gets forwarded. The default rule has no conditions and runs if no other rules are matched. Each rule has a priority, host and path and can only `Forward` to a target group. 

# ELB

Cross-zone load balancing must be enabled. 

## Cookie Stickiness

There are three `Cookie Stickiness`, also know as session affinity, with both enabled options end up with sticky sessions (so all sessions go back to the same server), options: 

* Disable Stickiness - The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance for session.

* Enable Load Balancer Generated Cookies - the ELB generates the cookie and manages the distribution of traffic;  also know as "duration based"; can set the duration of the distribution with zero seconds disabling cookie expiration

* Enable Application Generated Cookie Stickiness - ELB generates a cookie that correlates to the application cookie - the duration is set by the application cookie; name the cookie in the setting.


## Autoscaling and ELB Trouble Shooting

### Improving Operations

1. Install a web server on each instance, so the default health checks work well.

1. Enable keep-alive timeout.

1. Enable Path MTU Discovery

### General Config Problems

1. Attempting to create instance in the wrong zone, subnet or security group or with the wrong key pair. 

2. Instance is not supported in that AZ, region or is the wrong type for autoscaling.

3. The region is out of capacity.

3. There is an invalid EBS device mapping, or you are attaching a EBS block device to a instance store AMI.

4. An ELB will issues a `HTTP 503 Error` when it can process any more requests - call AWS if a huge traffic spike comes!

#### Dumbass Problems:

1. Autoscaling is not enabled on the account.

2. Autoscaling config is not working correctly. ie. there is a bug.

3. Autoscaling is in a "suspended state"

## End to End Encryption

Create a public key policy
Create a back-end instance authentication policy

## Patterns 

[Idle timeouts](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/config-idle-timeout.html) - decrease the connection idle timeouts from the front of the system to the back... this defaults to 60 seconds on the ELB. This does not apply to the ALB.

[More than one AZ]() - always associate an ELB with more than one AZ

[ELB routing results in AZ load in-balance]() - sometimes the lack of DNS servers causes an AZ to get hotter than others because some networks can't service the requests and cached results are used; enable cross zone routing with Route 53