---
layout: post
title: "AWS - Advanced Networking"
description: ""
category: posts
tags: [aws, vpc, aws-solutions-arch-pro, aws-guides]
---
{% include JB/setup %}

# Demonstrate ability to design and implement networking features of AWS

- largest and smallest network in VPC? CIDR Block between /16 and /28

- IP in /24 network? 251; First 4 IP Addresses and last 1 per CIDR Block used by AWS

- ELB

- HPC, Enhanced networking, multiple IP addresses, ENI, and Placement groups - part of EC2

- NAT & Scaling NAT

- VPC Peering

## HPC Use Cases

- Grid - distributed work loads that are loosely coupled and don't require tight communication between nodes; ASG has application here

- Cluster Computing - two or more instances connected together to support an application; high throughput low latency communication between nodes; weather computations; placement groups; Jumbo Frames - Bigger MTU (9000 byte) enables higher throughput; Risky outside of VPC; most useful in VPC with shared file systems like Lustre & NFS

## VPC Multicast Support

By default VPC does not support IP multicast yet numerous legacy application require it. To support this functionality, a [virtual overlay network](https://aws.amazon.com/articles/6234671078671125) can be created at the OS level of the instances. 

This OS level network includes a tunnel and virtual networking that must have a different CIDR range that is independent of the VPC. The tunnel are typically of the GRE or L2TP types and can be made with OpenVPN or Ntop's N2N.

# Demonstrate ability to design and implement connectivity features of AWS

Access S3 bucket in VPC? S3 end point

## Direct Connect

- Access private stuff? private VIF

- Access Public stuff? public VIF

- Access Public stuff in any US region? 

## VPN

Good connection with limited HA use two IPSec tunnel connections with [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol#Requirements_of_a_router_for_use_of_BGP_for_Internet_and_backbone-of-backbones_purposes) for failure recovery. For better HA, use two VPNs and with four IPSec tunnel connections.

VPNs might use RSA asymmetric keys (like SSL) or use Diffie-Hellman which provides Perfect Forward Secrecy and typically uses AES encryption.

There are four options to VPN into a VPC including hardware VPN, Direct Connect, VPN CloudHub, Software VPN. 

### VPN Setup

1. Create VPG and attach it to the VPC

2. Create CGW with static public IP address; ASN for BGP to enable dynamic routing; static route for non-BGP for static routing

3. Create VPN Connection in VPC (set VPG, CGW & Routing)

### VPN Limits

To connect securely with your VPC on AWS you can create a Virtual Public Gateway and connect that to a Customer Gateway forming a VPN. Dual VPN tunnels provide higher availability and that is the default configuration. Limits on VPN include:

- 5 Virtual Private Gateways per region

- 1 Virtual Private Gateways per VPC

- 50 Customer Gateways per Region

If you traffic is in excess of 4 Ggps, you need AWS Direct Connect which can provide up to 10Ggps. Direct Connect uses BGP and an Autonomous System Number (ASN) and IP prefixes. 

### Direct connect vs VPN Capability

- VPN = quick to bring up and easy; slower and sucks Internet bandwidth

- Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost

# Key Resources

- [Network Admin Guide to VPC](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Introduction.html)

- [Deep dive: Direct Connect & VPNs](https://www.youtube.com/watch?v=Qep11X1r1QA)

