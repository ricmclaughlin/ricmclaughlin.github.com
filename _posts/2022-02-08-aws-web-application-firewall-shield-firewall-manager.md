---
layout: post
title: "AWS - Web Application Firewall - Shield"
description: ""
category: posts
tags: [aws, aws-dev-ops-pro, vpc, shield, waf, aws-services]
---
{% include JB/setup %}


# WTF is WAF (Web Application Firewall)
WAF is a filtering capability that helps find and react to common web exploits for three basic use cases: reduced availability, compromise security, consume excess resources
Instead of just blocking attack sources, WAF protects again attack patterns based on the entire makeup of the request using filters which can allow, block or simply monitor the activity. WAF can be deployed against a API gateway, CloudFront distribution OR an Application Load Balancer. Of course, WAF is PCI compliant.
WAF Web ACL are assigned to resources such as API gateway, CloudFront, ALB

WAF can allow all requests, block all requests, or count the requests that match

Filter rules are executed in order and will act on the first rule that is matched OR the default rule will be executed. You must specify a default action for requests that don't match filter rules. The default action can be a fail/close where all non-matching requests fail (a default deny sort of thing) OR you can have a default allow filter rule.

Of course, WAF is PCI compliant.

## Web ACL terminology

*Conditions* can include XSS, IP address range, geo restrictions, query string/header parsing (using REGEX), SQL injection
*Rules* pull conditions together with a logical AND; rules are ordered and work to filter out bad requests; the default action occurs when no rules fire

## Web ACL Workflow
1. create a resource that can be protected including API gateway, CloudFront, ALB
1. create Web ACL - name it, give it a metric name space, a region, and associate it with a resource that gets traffict (HTTP/2 or WebSocket)
1. Create conditions - include XSS, IP address range, geo restrictions, query string/header parsing, SQL injection
1. Combine conditions together into a rule - for instance, put all the bad bot IPs and suspicious IPs in to a rule call "Bad user-agents" and BLOCK these IPs
1. Create more rules that will execute in order
1. Define a default action... like allow requests that don't match any rules

### Pricing

Simple pricing structure: 

$5 per web ACL per month
$1 per rule per month
0.60 per million requests

## Shield
Shield helps protect again DDoS attacks.

Shield comes in two flavors: Standard, which is included at no cost and protects against layer 3 & layer 4 attacks (SYN/UDP & reflection attacks), and Advanced ($3000 per month) which will absorb most attacks, give you a free coverage for usage (EC2, ELB, CloudFront, Route53) during the attack and access to the DDoS response team. 

# WTF is Firewall Manager?
[Firewall manager](https://aws.amazon.com/firewall-manager/) enables centralized management of firewall rules accross accounts and applications using AWS Organizations. Can be used to apply WAF rules, Shield protections and Security groups (for EC2 and ENI)

## Resources
Great Jeff Barr [introduction blog post about WAF for ALB ](https://aws.amazon.com/blogs/aws/aws-web-application-firewall-waf-for-application-load-balancers/) from right after 2016 re:Invent
