---
layout: post
title: "AWS - Advanced Networking"
description: ""
category: posts
tags: [aws, vpc, aws-solutions-arch-pro, aws-guides]
---
{% include JB/setup %}

# Demonstrate ability to design and implement networking features of AWS

ELB

HPC

Enhanced networking

Placement groups

NAT & Scaling NAT

VPC Peering

## VPC Multicast Support

By default VPC does not support IP multicast yet numerous legacy application require it. To support this functionality, a [virtual overlay network](https://aws.amazon.com/articles/6234671078671125) can be created at the OS level of the instances. 

This OS level network includes a tunnel and virtual networking that must have a different CIDR range that is independent of the VPC. The tunnel are typically of the GRE or L2TP types and can be made with OpenVPN or Ntop's N2N.

# Demonstrate ability to design and implement connectivity features of AWS

Direct Connect
  private vs public VIFs
HTTP End Points
CIDR Block between /16 and /28
5 IP Addresses per CIDR Block

## VPN

Good connection with limited HA use two IPSec tunnel connections with [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol#Requirements_of_a_router_for_use_of_BGP_for_Internet_and_backbone-of-backbones_purposes) for failure recovery. For better HA, use two VPNs and with four IPSec tunnel connections.

### VPN Setup

1. Create VPG and attach it to the VPC

2. Create CGW with static public IP address; ASN for BGP to enable dynamic routing; static route for non-BGP for static routing

3. Create VPN Connection in VPC (set VPG, CGW & Routing)

### VPN Limits

To connect securely with your VPC on AWS you can create a Virtual Public Gateway and connect that to a Customer Gateway forming a VPN. Dual VPN tunnels provide higher availability. Limits on VPN include:

- 5 Virtual Private Gateways per region

- 1 Virtual Private Gateways per VPC

- 50 Customer Gateways per Region

If you traffic is in excess of 4 Ggps, you need AWS Direct Connect which can provide up to 10Ggps. Direct Connect uses BGP and an Autonomous System Number (ASN) and IP prefixes. 

### Direct connect vs VPN Capability

- VPN = quick to bring up and easy; slower and sucks Internet bandwidth

- Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost

# Key Resources

- [Network Admin Guide to VPC](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Introduction.html)

https://www.youtube.com/watch?v=uQ9HQVTmhBQ

https://www.youtube.com/watch?v=Qep11X1r1QA

