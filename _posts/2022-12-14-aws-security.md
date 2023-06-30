---
layout: post
title: "AWS - Security"
description: ""
category: posts
tags: [aws-solutions-arch-pro, aws-guides]
---
{% include JB/setup %}

# Security Services
## Gateway Load Balancer
Essentially a centralized proxy server that operates at layer 3 to forward traffic to a set of security services and load balances between virtual devices if needed. Uses the GENEVE protocol on port 6081 to forward traffic to EC2 instances OR private IP addresses. Cross-Zone Load Balancing is disabled by default and costs if enabled.

## Detective
Detective analyzes, investigates, and identifies the root cause of security issues or suspicious activities using ML &amp; graphs. Uses VPC Flow Logs, CloudTrail, and GuardDuty data to create a visualizations in an unified view. 

## Security Hub
Centralized security dashboarding tool to manage security across accounts using automated security checks aggregating GuardDuty, Inspector, Macie, IAM access Analyzer, Systems Manager, Firewall Manager and Partner network solutions. Requires Config to be enabled. Security Hub generates EventBridge events, kicks off automated Security Hub findings, and enables Detective based analysis.

## Amazon GuardDuty
Uses ML to detect anomalies from CloudTrail Events, VPC flow logs, DNS logs, Kubernetes audit log then fire EventBridge events. Can protect against CryptoCurrency attacks. There is a limit of 5000 accounts that can be managed by GuardDuty in an Organization so there is the ability create a Delegated Administrator account FROM the Organization management account ONLY.

## Security Logs
Lots of service generate logs that can be useful for finding security issues. 
| Service       | S3 Delivery? | CloudWatch Logs Delivery? |
|---------------|--------------|---------------------------|
| ELB           | yes          | no                        |
| CloudTrail    | yes          | yes                       |
| VPC Flow Logs | yes          | yes                       |
| Route53       | no           | yes                       |
| S3 Access     | yes          | no                        |
| CloudFront    | yes          | no                        |
| Config        | yes          | no                        |

## Services to mitigate DDoS

### Minimize attack area
- Lock down access to production IP addresses? NAT
- well-formed TCP connections only; no SYN floods or UDP reflection attacks? ELB

### Absorb
- ASG - get bigger to absorb the attack...
- Shield - insurance for when the attack comes

### Safeguard Exposed Resources
(listed from outside to inside) 

- Route53 - low cost aliases, geo restriction, private IP
- CloudFront - Origin Access Identity, geo restriction &amp; blocking
- AWS Network Firewall
- WAF - inspect and disallow traffic; can be attached to the CloudFront distribution, ALB, or API Gateway; does not work for NLB
- NACL - filter bad traffic
- Instance firewall - not effective behind ALB because the source IP is block

## Management
- [Firewall Manager](/posts/aws-web-application-firewall-shield-firewall-manager) can help manage SG, Web ACL (WAF), and Shield protections which works in conjunction with AWS organizations 

## Security Management Service Triage

### Identify
- Security architecture guidance? Well-architected tool
- Security (&amp; operational) operational guidance? Trusted Advisor
- Inspect EC2, ECR images or Lambda functions network? Inspector
- shared resources ID? Access Analyzer
- ID excess permissions? Access Analyzer
- Policy validation? Access Analyzer

### Protect
- Store and rotate secrets? Secrets manager
- Store non-rotated configuration and passwords? Systems Manager Parameter Store (no certs)
- Store, create, provision certs? Certificate Manager
- MiTM attacks? use HTTPS instead of HTTP, DNSSEC (supported by Route53 for domain registration and DNS services using KMS)
- SSL certs on EC2 instances? retrieve cert from SSM Parameter store at boot (assuming proper permissions) OR ACM OR IAM

### Respond
- Parse CloudTrail, Flow, DNS GuardDuty logs? Detective (using ML)
- Aggregate alerts from GuardDuty, Inspector, Security Hub
- Changes to infrastructure? CloudTrail &amp; Config?

### Remediate
- Security and compliance documentation? Artifact

# Network Security Background
There are four main types of attacks:
* L7 - application attack - floods; slowloris; DNS query
* L6 - SSL Abuse
* L4 - SYN floods - Amplification/Reflection attacks - attacker sends in a small packet to an unrelated server that needs a 28 to 54 times greater response then the unrelated server using a spoofed address; unrelated server send response to target of attack.
* L3 - UDP flood - a UDP flood attack can be initiated by sending a large number of UDP packets to random ports on a remote host. As a result, the distant host will: Check for the application listening at that port; See that no application listens at that port; Reply with an ICMP Destination Unreachable packet.

## Attack Mitigation 
Mitigation is all about minimizing the attack surface, absorbing the attack, safeguarding the resources, learning normal behavior and having a plan for attacks.
- Minimize attack surface - Bastion box, limit access to resources to known IP ranges, WAF Sandwich (WAF between an external and internal ELB)
- Absorbing the attack - scale horizontally or vertically which spread the attack over a larger area (and makes the attackers scale), and using CloudFront gives you time to analyze the attack
- Safeguarding the resources - Using CloudFront by adding geo restrictions/blocking or Origin Access Identity (S3 access through CloudFront URLs only) Route53 by adding an alias to different distribution or ELB and using private DNS, WAF service or a WAF market place solution and Shield.
- Learn normal behavior - and set alarms for abnormal traffic patterns. Generally using CloudWatch and VPC flow logs is the way to figure out if there is a DDoS attach underway and what normal traffic is. Important CloudWatch metrics to watch include: Cloudfront TotalErrorRate and Requests, EC2 CPUUtilization and NetworkIn and ELB SurgeQueueLength, and RequestCount.

## General Protection Services
Two main types of services:
- Intrusion Detection System (IDS) - inspects all inbound and outbound network activity and IDs suspicious patterns; this is just a monitoring service. Generic versions of IDS don't work on AWS because they rely on packet sniffing sort of promiscuous mode and that is a no-go; a proxy service or proxy server sort of architecture might work or redirecting copies of traffic to a service using agents on servers.
- Intrusion Protection System (IPS) - inspects all inbound and outbound network activity AND protects the stuff from intrusion

Generally IDS/IPS systems site in a public subnet and write data from received from an EC2 agent to a log the forward the log to a SOC or sometime in S3.