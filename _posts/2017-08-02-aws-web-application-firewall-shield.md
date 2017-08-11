---
layout: post
title: "AWS - Web Application Firewall - Shield"
description: ""
category: posts
tags: [aws, aws-dev-ops-pro, vpc, shield, waf, aws-services]
---
{% include JB/setup %}


## Web Application Firewall

Instead of just blocking attack sources, WAF protects again attack patterns based on the entire makeup of the request using filters which can allow, block or simply monitor the activity. WAF can be deployed against a CloudFront distribution OR an Application Load Balancer.

Filter rules are executed in order and will act on the first rule that is matched OR the default rule will be executed. You must specify a default action for requests that don't match filter rules. The default action can be a fail/close where all non-matching requests fail (a default deny sort of thing) OR you can have a default allow filter rule.

Of course, WAF is PCI compliant.

### Pricing

Simple pricing structure: 

$5 per web ACL per month
$1 per rule per month
0.60 per million requests

## Shield

Shield comes in two flavors. Standard, which is free and not of much help and and Advanced which will absorb most attacks and give you a free coverage for usage during the attack. 

## Background

There are four main types of attacks:

* L7 - application attack - floods; slowloris; DNS query

* L6 - SSL Abuse

* L4 - SYN floods - Amplification/Reflection attacks - attacker sends in a small packet to an unrelated server that needs a 28 to 54 times greater response then the unrelated server using a spoofed address; unrelated server send response to target of attack.

* L3 - UDP Attacks - a UDP flood attack can be initiated by sending a large number of UDP packets to random ports on a remote host. As a result, the distant host will: Check for the application listening at that port; See that no application listens at that port; Reply with an ICMP Destination Unreachable packet.

### Mitigation 

Mitigation is all about minimizing the attack surface, absorbing the attack, safeguarding the resources, learning normal behavior and having a plan for attacks.

- Minimize attack surface - Bastion box, limit access to resources to known IP ranges, WAF Sandwich (WAF between an external and internal ELB)

- Absorbing the attack - scale horizontally or vertically which spread the attack over a larger area (and makes the attackers scale), and using CloudFront gives you time to analyze the attack

- Safeguarding the resources - Using CloudFront by adding geo restrictions/blocking or Origin Access Identity (S3 access through CloudFront URLs only) Route53 by adding an alias to different distribution or ELB and using private DNS, WAF service or a WAF market place solution and Shield.

- Learn Normal behavior - and set alarms for abnormal traffic patterns. Generally using CloudWatch and VPC flow logs is the way to figure out if there is a DDoS attach underway and what normal traffic is. Important CloudWatch metrics to watch include: Cloudfront TotalErrorRate and Requests, EC2 CPUUtilization and NetworkIn and ELB SurgeQueueLength and RequestCount.


### General Protection Services

Two main types of services:

Intrusion Detection System (IDS) - inspects all inbound and outbound network activity and IDs suspicious patterns; this is just a monitoring service

Intrusion Protection System (IPS) - inspects all inbound and outbound network activity AND protects the stuff from intrusion

Generally IDS/IPS systems site in a public subnet and write data from recieved from an EC2 agent to a log the forward the log to a SOC or sometime in S3.
