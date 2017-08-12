---
layout: post
title: "AWS - Security"
description: ""
category: posts
tags: [aws, vpc, aws-solutions-arch-pro, aws-guides]
---
{% include JB/setup %}

# Design information security management systems and compliance controls
  
  CloudWatch
  Inspector
  CloudTrail
  VPC Flow Logs

# Design security controls with the AWS shared responsibility model and global infrastructure
  
  - Cloud HSM

  - KMS

# Design identity and access management controls
  
  STS
    ID broker Scenarios
      Auth with LDAP first, then hit STS to get role info for temp access
  IAM
  Directory Services
  Tag Manager

# Design protection of Data at Rest controls

  Encryption - S3, RDS 
  Bucket Policies
  MFA Delete
  Versioning
  Backup to separate account

# Design protection of Data in Flight and Network Perimeter controls

## Network Security

There are four main types of attacks:

* L7 - application attack - floods; slowloris; DNS query

* L6 - SSL Abuse

* L4 - SYN floods - Amplification/Reflection attacks - attacker sends in a small packet to an unrelated server that needs a 28 to 54 times greater response then the unrelated server using a spoofed address; unrelated server send response to target of attack.

* L3 - UDP Attacks - a UDP flood attack can be initiated by sending a large number of UDP packets to random ports on a remote host. As a result, the distant host will: Check for the application listening at that port; See that no application listens at that port; Reply with an ICMP Destination Unreachable packet.

### Attack Mitigation 

Mitigation is all about minimizing the attack surface, absorbing the attack, safeguarding the resources, learning normal behavior and having a plan for attacks.

- Minimize attack surface - Bastion box, limit access to resources to known IP ranges, WAF Sandwich (WAF between an external and internal ELB)

- Absorbing the attack - scale horizontally or vertically which spread the attack over a larger area (and makes the attackers scale), and using CloudFront gives you time to analyze the attack

- Safeguarding the resources - Using CloudFront by adding geo restrictions/blocking or Origin Access Identity (S3 access through CloudFront URLs only) Route53 by adding an alias to different distribution or ELB and using private DNS, WAF service or a WAF market place solution and Shield.

- Learn normal behavior - and set alarms for abnormal traffic patterns. Generally using CloudWatch and VPC flow logs is the way to figure out if there is a DDoS attach underway and what normal traffic is. Important CloudWatch metrics to watch include: Cloudfront TotalErrorRate and Requests, EC2 CPUUtilization and NetworkIn and ELB SurgeQueueLength, and RequestCount.

### General Protection Services

Two main types of services:

Intrusion Detection System (IDS) - inspects all inbound and outbound network activity and IDs suspicious patterns; this is just a monitoring service. Generic versions of IDS don't work on AWS because they rely on packet sniffing sort of promiscuous mode and that is a no-go; a proxy service or proxy server sort of architecture might work or redirecting copies of traffic to a service using agents on servers.

Intrusion Protection System (IPS) - inspects all inbound and outbound network activity AND protects the stuff from intrusion

Generally IDS/IPS systems site in a public subnet and write data from received from an EC2 agent to a log the forward the log to a SOC or sometime in S3.

## Services

### Mitigation
  


  ASG, CloudFront, ELB, Route53
  DDoS Protection - waf, shield
  CloudWatch



## Resources

- [Overview of Security Processes White Paper](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf)

- [DDoS White Paper](https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
