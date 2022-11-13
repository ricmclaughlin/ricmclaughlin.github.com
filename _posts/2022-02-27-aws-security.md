---
layout: post
title: "AWS - Security"
description: ""
category: posts
tags: [aws, vpc, aws-solutions-arch-pro, aws-guides]
---
{% include JB/setup %}

# Design information security management systems and compliance controls
  
- [CloudWatch](/posts/aws-cloudwatch) - logs retained forever; alarms & metrics retained 14 days

- [CloudTrail](/posts/aws-cloudtrail) - Records API access to a log or Cloudwatch

- [Inspector](https://aws.amazon.com/inspector/) - this applies yet isn't covered yet...

- [VPC](/posts/aws-vpc) - VPC Flow Logs

- [Firewall Manager](/posts/aws-firewall-manager)

# Design security controls with the AWS shared responsibility model and global infrastructure
  
- [Cloud HSM](/posts/aws-kms-hsm) - single tenant; need two; keys are regional; Oracle, MS SQL, redshift, SSL offload

# Design identity and access management controls
  
- [STS](/posts/aws-sts) - client -> broker -> IdP -> broker -> STS -> broker -> client

- [IAM](/posts/aws-iam) - Pretty much all of this...
  
- [Directory Service](/posts/aws-directory-service) - simple AD = Samba 4 (NO MFA); AD Connector (MFA!!)

- Tag Manager and resource groups as described in the [costing drill down](/posts/aws-costing)

# Design protection of Data at Rest controls

- [RDS](/posts/aws-rds)

- [S3](/posts/aws-s3) - Bucket Policies; MFA Delete; Versioning; Backup to separate account; encryption

- [KMS](/posts/aws-kms-hsm) - Bucket Policies; MFA Delete; Versioning; Backup to separate account; encryption

# Design protection of Data in Flight and Network Perimeter controls

- [VPC](/posts/aws-vpc) - SG vs NACLS

- [CloudFront](/posts/aws-cloudfront) - SSL

- [Elastic Load Balancer](/posts/aws-elastic-load-balancer)

- [Web Application Firewall(WAF)](/posts/aws-web-application-firewall-shield-firewall-manager) - Web ACL

- [Shield](/posts/aws-web-application-firewall-shield-firewall-manager#Shield) 

## Network Security

There are four main types of attacks:

* L7 - application attack - floods; slowloris; DNS query

* L6 - SSL Abuse

* L4 - SYN floods - Amplification/Reflection attacks - attacker sends in a small packet to an unrelated server that needs a 28 to 54 times greater response then the unrelated server using a spoofed address; unrelated server send response to target of attack.

* L3 - UDP flood - a UDP flood attack can be initiated by sending a large number of UDP packets to random ports on a remote host. As a result, the distant host will: Check for the application listening at that port; See that no application listens at that port; Reply with an ICMP Destination Unreachable packet.

### Attack Mitigation 

Mitigation is all about minimizing the attack surface, absorbing the attack, safeguarding the resources, learning normal behavior and having a plan for attacks.

- Minimize attack surface - Bastion box, limit access to resources to known IP ranges, WAF Sandwich (WAF between an external and internal ELB)

- Absorbing the attack - scale horizontally or vertically which spread the attack over a larger area (and makes the attackers scale), and using CloudFront gives you time to analyze the attack

- Safeguarding the resources - Using CloudFront by adding geo restrictions/blocking or Origin Access Identity (S3 access through CloudFront URLs only) Route53 by adding an alias to different distribution or ELB and using private DNS, WAF service or a WAF market place solution and Shield.

- Learn normal behavior - and set alarms for abnormal traffic patterns. Generally using CloudWatch and VPC flow logs is the way to figure out if there is a DDoS attach underway and what normal traffic is. Important CloudWatch metrics to watch include: Cloudfront TotalErrorRate and Requests, EC2 CPUUtilization and NetworkIn and ELB SurgeQueueLength, and RequestCount.

## Services to mitigate DDoS

### Minimize attack area

- NAT - lock down access to production IP addresses

- ELB - well-formed TCP connections only; no SYN floods or UDP reflection attacks

### Absorb

- ASG - get bigger to absorb the attack...

- Shield - insurance for when the attack comes

### Safeguard Exposed Resources
(listed from outside to inside) 

- Route53 - low cost aliases, geo restriction, private IP

- CloudFront - Origin Access Identity, geo restriction & blocking

- NACL - filter bad traffic

- WAF - inspect and disallow traffic; can be attached to the CloudFront distribution or ALB

- instance firewall - not effective behind ALB because the source IP is block

## Management

- [Firewall Manager](/posts/aws-web-application-firewall-shield-firewall-manager) can help manage SG, Web ACL (WAF), and Shield protections which works in conjunction with AWS organizations 

### General Protection Services

Two main types of services:

- Intrusion Detection System (IDS) - inspects all inbound and outbound network activity and IDs suspicious patterns; this is just a monitoring service. Generic versions of IDS don't work on AWS because they rely on packet sniffing sort of promiscuous mode and that is a no-go; a proxy service or proxy server sort of architecture might work or redirecting copies of traffic to a service using agents on servers.

- Intrusion Protection System (IPS) - inspects all inbound and outbound network activity AND protects the stuff from intrusion

Generally IDS/IPS systems site in a public subnet and write data from received from an EC2 agent to a log the forward the log to a SOC or sometime in S3.

## Security Management Service Triage


### Identify
Security architecture guidance? Well-architected tool

Security (& operational) operational guidance? Trusted Advisor

### Protect
Store and rotate secrets? Secrets manager

Store non-rotated configuration and passwords? Systems Manager Parameter Store (no certs)

Store, create, provision certs? Certificate Manager


### Respond
Parse CloudTrail, Flow, DNS GuardDuty logs - Detective (using ML)

Aggregate alerts from GuardDuty, Inspector, Macie? Security Hub

Changes to infrastructure? CloudTrail [config?]


### Remediate


Security and compliance documentation? Artifact







## Identity Triage

AuthZ - Cognito Identity Pools

AuthN - Cognito User Pools (mobile focused)

AuthN/Z (AWS resources using directory) - AWS SSO

AuthN/Z AWS resources - IAM


## Resources

- [DDoS White Paper](https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)

- [Best Practices for Content Delivery using Amazon CloudFront](https://www.youtube.com/watch?v=s9Xt1qzD6SA)
