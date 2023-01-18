---
layout: post
title: "AWS - Web Application Firewall - Shield"
description: ""
category: posts
tags: [aws, aws-dev-ops-pro, shield, waf, aws-services, inspector]
---
{% include JB/setup %}


# WTF is WAF (Web Application Firewall)
WAF is a filtering capability that helps find and react to common web exploits for three basic use cases: reduced availability, compromise security, consume excess resources
Instead of just blocking attack sources, WAF protects again attack patterns based on the entire makeup of the request using filters which can allow, block or simply monitor the activity. WAF can be deployed against a API gateway, CloudFront distribution, or an Application Load Balancer. Of course, WAF is PCI compliant. 

WAF can allow all requests, block all requests, or count the requests that match the conditions you specify.

Filter rules are executed in order and will act on the first rule that is matched OR the default rule will be executed. You must specify a default action for requests that don't match filter rules. The default action can be a fail/close where all non-matching requests fail (a default deny sort of thing) OR you can have a default allow filter rule.

## Web ACL terminology
*Conditions* can include XSS, IP address range, geo restrictions, query string/header parsing (using REGEX), SQL injection

*Rules* pull conditions together with a logical AND; rules are ordered and work to filter out bad requests; the default action occurs when no rules fire

## Web ACL Workflow
1. create a resource that can be protected including API gateway, CloudFront, ALB
1. create Web ACL - name it, give it a metric name space, a region, and associate it with a resource that gets trafficked (HTTP/2 or WebSocket)
1. Create conditions - include XSS, IP address range, geo restrictions, query string/header parsing, SQL injection
1. Combine conditions together into a rule - for instance, put all the bad bot IPs and suspicious IPs in to a rule call "Bad user-agents" and BLOCK these IPs
1. Create more rules that will execute in order
1. Define a default action... like allow requests that don't match any rules

### Pricing
Simple pricing structure: 
- $5 per web ACL per month
- $1 per rule per month
- 0.60 per million requests

## Shield
Shield helps protect again DDoS attacks. Shield comes in two flavors: 
- _Standard_ is included at no cost and protects against layer 3 & layer 4 attacks (SYN/UDP & reflection attacks)
_ Advanced_ which will absorb most attacks, give you a free coverage for usage (Global Accelerator, EC2, ELB, CloudFront, Route53, EIP) during the attack and access to the DDoS response team for a cost ($3000 per month). You have to enable protects; nothing is protected by default.

# WTF is Firewall Manager?
[Firewall manager](https://aws.amazon.com/firewall-manager/) enables centralized management of firewall rules across accounts and applications using AWS Organizations. Can be used to apply WAF rules, Shield protections, Security groups (for EC2 and ENI), Network Firewall, Route53 DNS Firewall at the regional level. Once defined Firewall Manager Rules are applied for all new resources.

# Inspector
Runs automated security assessments for compute using CVE and network reach-ability for EC2. For EC2 it uses the SSM agent to scan for network accessibility issues and known vulnerabilities. For ECR, assess images as they are pushed. For Lambda, function code and package dependencies are analyzed as they are deployed. Integrates with Security Hub and EventBridge and reports a risk score.

## Resources
Great Jeff Barr [introduction blog post about WAF for ALB ](https://aws.amazon.com/blogs/aws/aws-web-application-firewall-waf-for-application-load-balancers/) from right after 2016 re:Invent

# Triage
DDOS attack coverage? Shield Advanced &amp; WAF
Centralize Firewall management? Firewall Manager