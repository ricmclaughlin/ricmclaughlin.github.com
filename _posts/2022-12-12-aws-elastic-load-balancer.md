---
layout: post
title: "AWS - Elastic Load Balancer"
description: ""
category: posts
tags: [devops, networking, aws-solutions-arch-pro]
---
{% include JB/setup %}

[Elastic Load Balancer](https://aws.amazon.com/documentation/elastic-load-balancing/) distributes traffic to instances that belong to the ELB group - generally across multiple AZs. One nifty features it that it allows us to offload SSL certs to load balancers instead of webservers! ELB configuration requires a protocol, a front end port, and a back end port. ELB are charged by the hour and per GB of usage. Only one SSL Cert per ELB. The max number of requests that can be queued is 1024, can only use one subnet per AZ and must be set up to use 2 AZ; can use up to 5 Security groups and also features deletion protection.

There are ~~two~~ three versions of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL; layer 4), Network load balancers (like classic but best for TCP at scale) and Application Load Balancer (any port - just HTTP, HTTPS; layer 7). ELBs do not have a defined IPV4 address but do support both IPV4 &amp; IPV6.

Lots of differences between externally and internally facing load balancers:
- External = must be placed in a public subnet; public IP; external DNS
- Internal = no public IP; internal DNS name

Lots of times you will know there is more traffic a-coming. In these cases you can "pre-warm" the ELB by contacting AWS! When enabled Cross-Zone Load Balancing sends equal traffic to clients regardless of AZ; when disabled equal traffic traffic is sent to each AZ.

## SSL on Load Balancers
One of the key features of ELB is the ability to terminate SSL connections for instances in the load balancing group. In this configuration, the HTTPS client uses port 443 to communicate with the ELB and the ELB communicates on port 80 to the web server instances in the autoscaling group. 

Managing the certificate on the ELB is always the magic... In fact, managing certs in general, is the magic. There are three options. 
0. Use the new AWS Certificate Manager service. 
0. Use a certificate from IAM. 
0. Upload your own.

In addition to a cert, you need to define an SSL Negotiation configuration, called a security policy, which is a combination of SSL protocols, SSL ciphers, and the Server Order Preference option.

If you have multiple ELB then multiple SSL Certs are required unless you use a wildcard (SNI) certificate.

## Health Checks
To enable removing unhealthy instances from the round robin, each ELB can do a health check of the instances in the load balancing group. The health check can use different ports, including port 80, and set a response timeout, a health check interval, an unhealthy threshold, and a healthy threshold. 

Load balancer [health checks](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-healthchecks.html) only determine if the instance is available; optionally, an ASG can use ELB health checks in addition to the EC2 status checks it uses.

## Differences between ELB (Classic) &amp; ALB

| Thingy                    |           ELB          | NLB                  |                     ALB                    |
|---------------------------|:----------------------:|----------------------|:------------------------------------------:|
| HTTP & HTTPS              |           Yes          | Yes                  |                     Yes                    |
| SSL Termination           |    Yes - single cert   | Yes                  |        Yes - Server Name Indication        |
| Layer?                    |  4 (forwards request)  | 4 (TCP, UDP)         | 7 (terminates; parses header; new request) |
| Layer 7                   |           No           | No                   |                     Yes                    |
| Websockets/HTTP2          |           No           | Yes                  |                     Yes                    |
| Path Routing              |           No           | No                   |                     Yes                    |
| Cross-Zone Load Balancing |     Disabled - free    | Disabled - $$        |            Always active - free            |
| Cookie Stickiness         |           Yes          | Flow Hash                   |      Yes Cookies only (called AWSALB)      |
| Health Check              | TCP, ICMP, HTTP, HTTPS | TCP, HTTP, HTTPS     |                 HTTP, HTTPS                |
| MultiAZ?                  |           No           | Yes - one EIP per AZ |                  Required                  |

## Application Load Balancer
Path based routing & management - because the routing is based on URL matching target groups can be containers, different port on the same box or simply different target groups. ALB uses Dynamic Port Mapping to place more than one task of the same app on an ECS cluster. ALB can front Lambda by turning the HTTP request in to a JSON event. Session stickiness is supported at the target group level.

Target groups are EC2 instances or containers managed as an entity and checks the health of the group automatically. By default, ALB use Round Robin routing. 

### Listeners
ALB listeners supports HTTP &amp; HTTPS only. Rules determine where the traffic gets forwarded. The default rule has no conditions and runs if no other rules are matched. Each rule has a priority, host and path and can only `Forward` to a target group.

## Network Load Balancer
NLB are layer 4 load balancers that work with massive amounts of UDP and TCP traffic. NLB supports one static IP per AZ and also supports Elastic IP addresses. It's common to puts an NLB in front of an ALB to keep the static IP address. 

NLB use Flow Hash routing that enables a TCP/UDP connection to be routed a single target for the life of the connection.

Doing a DNS lookup on a NLB zonal name will resolves to all the NLB nodes in all enabled AZs (so typically more than 1 IP address will be returned). Add the AZ name to the NLB DNS names resolve the specific IP for that AZ - this could be useful for an application to keep traffic within a single AZ. NLB don't support SG.  

## Classic Load Balancer 
In a EC2-Classic situation, an ELB support ports, 25, 80, 443, 465, 587 and 1024-65535 (ephemeral ports) while in an EC2-VPC it support ports 1-65535. An Elastic IP can not be assigned to a ELB. DNS apex zone support is in the house while multiple non-wild card SSL certs will require multiple ELB. It also support IPv4 &amp; IPv6. Can load balance the zone apex as well. Does not support SNI certs.

### Classic Cookie Stickiness
There are three `Cookie Stickiness`, also know as session affinity, with both enabled options end up with sticky sessions (so all sessions go back to the same server), options: 

* Disable Stickiness - The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance for session.

* Enable Load Balancer Generated Cookies - the ELB generates the cookie and manages the distribution of traffic;  also know as "duration based"; can set the duration of the distribution with zero seconds disabling cookie expiration

* Enable Application Generated Cookie Stickiness - ELB generates a cookie that correlates to the application cookie - the duration is set by the application cookie; name the cookie in the setting.




