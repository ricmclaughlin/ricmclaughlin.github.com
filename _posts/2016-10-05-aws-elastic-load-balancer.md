---
layout: post
title: "AWS - Elastic Load Balancer"
description: ""
category: posts
tags: [aws, devops, elastic-load-balancer, aws-dev-ops-pro]
---
{% include JB/setup %}

[Elastic Load Balancer](https://aws.amazon.com/documentation/elastic-load-balancing/) distributes traffic to instances that belong to the ELB group - this can be in a single AZ or multiple AZs. One nifty features it that it allows us to offload SSL certs to load balancers instead of webservers! ELB configuration requires a protocol, a front end port, and a back end port. ELB are NOT free - there is a charge by the hour and per GB of usage. Only one SSL Cert per ELB. The max number of requests that can be queued is 1024, can only use one subnet per AZ; can use up to 5 Security groups and also use deletion protection.

There are two version of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL; layer 4) and Application Load Balancer (any port - just HTTP, HTTPS; layer 7). ELBs do not have a defined IPV4 address but do support both IPV4 & IPV6.

Lots of differences between externally and internally facing load balancers:

- External = must be placed in a public subnet; Elastic IP; external DNS

- Internal = no public IP or Elastic IP; internal DNS name

Lots of times you will know there is more traffic a-coming. In these cases you can "pre-warm" the ELB by contacting AWS!

## SSL on ELB

One of the key features of ELB is the ability to terminate SSL connections for instances in the load balancing group - SSL is still a highly compute intensive process for webservers. In this configuration, the HTTPS client uses port 443 to communicate with the ELB and the ELB communicates on port 80 to the web server instances in the autoscaling group. Although a great feature, end-to-end encryption is an important aspect to consider in system design.

Managing the certificate on the ELB is always the magic... In fact, managing certs in general, is the magic. There are three options. 

1. Use the new AWS Certificate Manager service. Especially cool now that it is integrated with Elastic Beanstalk - nifty and FREE! 

2. Use a certificate from IAM. 

3. Upload your own.

In addition to a cert, you need to define an SSL Negotiation configuration, called a security policy, which is a combination of SSL protocols, SSL ciphers, and the Server Order Preference option.

## Health Checks

To enable removing unhealthy instances from the round robin, each ELB can do a health check of the instances in the load balancing group. The health check can use different ports, including port 80, and set a response timeout, a health check interval, an unhealthy threshold and a healthy threshold. 

ELB [health checks](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-healthchecks.html) only determine if the instance will get traffic routed to it; optionally, an ASG can use ELB health checks in addition to the EC2 status checks it uses.

## Metrics

Metrics are reported every 60 seconds but if there is no traffic, no metric will be reported. For the ELB there are two useful dimensions the `AvailabilityZone` and `LoadBalancerName`; the ALB adds a `TargetGroup` dimension.

* SurgeQueueLength - Length of waiting queue - closer to zero the better (up to 1024)

* SpilloverCount - How many requests are NOT serviced by the load balancer in EXCESS of the QueueLength - closer to zero the better; This is a bad, bad thing. Avoid. ELB reports a `503 - Service Unavailable`

* Latency - How long a page takes to return

* BackendConnectionErrors - unsuccessful connection to the backend; Look at SUM and difference between min and max values

* HealthyHostCount, UnHealthyHostCount - how well the backend instances are holding up

* HTTPCode_Backend_XXX - response codes from the backend (2XX, 3XX, 4XX)

* HTTPCode_ELB_4XX - malformed on incomplete client requests

* HTTPCode_ELB_5XX - no healthy backend instance or request rate too high

* RequestCount - # of requests over 1 or 5 minute interval

## Logging

Strangely [ELB Logging](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/access-log-collection.html) is disabled by default and are generated on a best-case situation so data might be missing; logs are stored in S3 and delivered every hour OR every 5 minutes.

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

ALB can also forward X-Forwarded-For header so logging can occur at the instance layer NOT the ALB (because ALB routing is best effort to complete). To accomplish the same thing with an ELB you need to enable the [Proxy Protocol Headers](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-proxy-protocol.html) which adds a header for the backend to parse but this does not enable sticky session or `X-Forward-For` header.

The ALB can also add the custom "X-Amzn-Trace-Id" HTTP header on all requests to improve tracability. 

## Differences between ELB &amp; ALB

| Thingy | ELB   |      ALB      |
|-----------------|:-------------:|:------:|
| HTTP & HTTP | Yes | Yes |
| TCP/SSL (Layer 4) | Yes | No |
| Layer? | 4 (forwards request) | 7 (terminates; parses header; new request) |
| Layer 7 | No | Yes |
| Websockets | No | Yes |
| Path Routing | No | Yes |
| Host Routing | No | Yes |
| Cookie Stickiness | Yes | ELB Cookies only (called AWSALB) |
| Health Check | TCP, ICMP, HTTP, HTTPS| HTTP, HTTPS |
| Require MultiAZ? | No | Yes |

# ALB

Path based routing & management - because the routing is based on URL matching target groups can be containers, different port on the same box or simply different target groups. The enables the ability to manage each target group individually.

Target groups are EC2 instances or containers managed as an entity and checks the health of the group automatically.

## Listeners

ALB listeners supports HTTP & HTTPS only. Rules determine where the traffic gets forwarded. The default rule has no conditions and runs if no other rules are matched. Each rule has a priority, host and path and can only `Forward` to a target group. 

## ELB Facts

In a EC2-Classic situation, an ELB support ports, 25, 80, 443, 465, 587 and 1024-65535 (how is this on the test???) while in an EC2-VPC it support ports 1-65535. An Elastic IP can no be assigned to a ELB. DNS apex zone support is in the house while multiple non-wild card SSL certs will require multiple ELB.

## ELB Cookie Stickiness

There are three `Cookie Stickiness`, also know as session affinity, with both enabled options end up with sticky sessions (so all sessions go back to the same server), options: 

* Disable Stickiness - The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance for session.

* Enable Load Balancer Generated Cookies - the ELB generates the cookie and manages the distribution of traffic;  also know as "duration based"; can set the duration of the distribution with zero seconds disabling cookie expiration

* Enable Application Generated Cookie Stickiness - ELB generates a cookie that correlates to the application cookie - the duration is set by the application cookie; name the cookie in the setting.

## Autoscaling and ELB Trouble Shooting

### Improving Operations

- Install a web server on each instance, so the default health checks work well.

- Enable keep-alive timeout so the ELB to backend connection stays open

- Enable Path MTU Discovery

- Use TCP if your application does NOT use common HTTP codes

- When using TCP 

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

## Patterns 

[Idle timeouts](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/config-idle-timeout.html) - decrease the connection idle timeouts from the front of the system to the back... this defaults to 60 seconds on the ELB. This does not apply to the ALB.

[More than one AZ]() - always associate an ELB with more than one AZ

[ELB routing results in AZ load in-balance]() - sometimes the lack of DNS servers causes an AZ to get hotter than others because some networks can't service the requests and cached results are used; enable cross zone routing with Route 53