---
layout: post
title: "AWS - Advanced Networking"
description: ""
category: 
tags: [aws, vpc, aws-solutions-arch-pro, networking]
---
{% include JB/setup %}

# VPN

Good connection with limited HA use two IPSec tunnel connections with [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol#Requirements_of_a_router_for_use_of_BGP_for_Internet_and_backbone-of-backbones_purposes) for failure recovery. For better HA, use two VPNs and with four IPSec tunnel connections.

[Network Admin Guide to VPC](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Introduction.html)

## VPN Setup

1. Create VPG and attach it to the VPC

2. Create CGW with static public IP address; ASN for BGP to enable dynamic routing; static route for non-BGP for static routing

3. Create VPN Connection in VPC (set VPG, CGW & Routing)

## VPN Limits

To connect securely with your VPC on AWS you can create a Virtual Public Gateway and connect that to a Customer Gateway forming a VPN. Dual VPN tunnels provide higher availability. Limits on VPN include:

- 5 Virtual Private Gateways per region

- 1 Virtual Private Gateways per VPC

- 50 Customer Gateways per Region

If you traffic is in excess of 4 Ggps, you need AWS Direct Connect which can provide up to 10Ggps. Direct Connect uses BGP and an Autonomous System Number (ASN) and IP prefixes. 

## Direct connect vs VPN Capability

- VPN = quick to bring up and easy; slower and sucks Internet bandwidth

- Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost


# Network Security

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

- Learn Normal behavior - and set alarms for abnormal traffic patterns. Generally using CloudWatch and VPC flow logs is the way to figure out if there is a DDoS attach underway and what normal traffic is. Important CloudWatch metrics to watch include: Cloudfront TotalErrorRate and Requests, EC2 CPUUtilization and NetworkIn and ELB SurgeQueueLength, and RequestCount.


### General Protection Services

Two main types of services:

Intrusion Detection System (IDS) - inspects all inbound and outbound network activity and IDs suspicious patterns; this is just a monitoring service. Generic versions of IDS don't work on AWS because they rely on packet sniffing sort of promiscuous mode and that is a no-go; a proxy service or proxy server sort of architecture might work or redirecting copies of traffic to a service using agents on servers.

Intrusion Protection System (IPS) - inspects all inbound and outbound network activity AND protects the stuff from intrusion

Generally IDS/IPS systems site in a public subnet and write data from received from an EC2 agent to a log the forward the log to a SOC or sometime in S3.


# VPC Multicast Support

By default VPC does not support IP multicast yet numerous legacy application require it. To support this functionality, a [virtual overlay network](https://aws.amazon.com/articles/6234671078671125) can be created at the OS level of the instances. 

This OS level network includes a tunnel and virtual networking that must have a different CIDR range that is independent of the VPC. The tunnel are typicall of the GRE or L2TP types and can be made with OpenVPN or Ntop's N2N.
