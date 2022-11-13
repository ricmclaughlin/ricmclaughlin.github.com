---
layout: post
title: "AWS - Advanced Networking"
description: ""
category: posts
tags: [aws, vpc, aws-solutions-arch-pro, aws-guides]
---
{% include JB/setup %}

## HPC Use Cases

- Grid - distributed work loads that are loosely coupled and don't require tight communication between nodes; ASG has application here

- Cluster Computing - two or more instances connected together to support an application; high throughput low latency communication between nodes; weather computations; placement groups; Jumbo Frames - Bigger MTU (9000 byte) enables higher throughput; Risky outside of VPC; most useful in VPC with shared file systems like Lustre & NFS

## VPC Multicast Support

By default VPC does not support IP multicast yet numerous legacy application require it. To support this functionality, a [virtual overlay network](https://aws.amazon.com/articles/6234671078671125) can be created at the OS level of the instances. 

This OS level network includes a tunnel and virtual networking that must have a different CIDR range that is independent of the VPC. The tunnel are typically of the GRE or L2TP types and can be made with OpenVPN or Ntop's N2N.

# VPC Endpoints

VPC endpoints enable traffic to AWS services to be routed directly using a dedicated endpoint within the VPC. There are two type: Gateway endpoints (free, only for DynamoDB and S3) and Interface endpoints (cost, available for almost all services & many APN services). You can connect a single endpoint policies & 1 or more security groups to control access.

# Private link (& VPC endpoints)

Wanna enable others to use your service? Create a VPC endpoint (and private link) instead of VPC peering.

# VPN

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

## Transit Gateway
Transit gateway is a hub-spoke model for transitive peering of lots of VPC. The downside is that it requires NON-overlapping IP address ranges. Transit gateway works across region by default and across accounts using Resource Access Manager (which requires an Organizations implementation). Route tables can be used to shape connectivity (along with Transit Gateway associations), works with Direct Connect and VPN AND supports IP Multicast.

## VPN CloudHub

CloudHub is a VPN connectivity solution for multiple site VPN connectivity along with connectivity to AWS using a hub a spoke model.

## Direct Connect

- Access private stuff? private VIF

- Access Public stuff? public VIF

- Access Public stuff in any US region? 

### Direct connect vs VPN Capability

- VPN = quick to bring up and easy; slower and sucks Internet bandwidth

- Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost

## ENI vs EN vs EFA

ENI = Elastic Network Interface - use these to create dual homed instances, possibly for IP restricted software licenses, low budget HA solution or an interface per subnet

EN = Enhanced networking - uses SR-IO; no cost but requires specific instance types; uses either an ENA (100 Gps) or a VF (10 Gps) with an ENA being the default thing to do if possible

EFA = Elastic Fabric Adapter - HPC or ML applications using OS-by-pass

## AWS Global Accelerator

This service enables the use of the nearest edge location, with public IP addreses, to access AWS regional resources instead of routing requests over the Internet to the regional public IP addresses. The benefits include regional failover (HA), improve peformance, and consistent IP caching. It's CloudFront but for endpoints with a flare for non-HTTP use cases.

### Workflow to create a Global Accelerator

0. Create the Accelerator - name it, selected IP address type, use AWS IP or provide 2 IP addresses

0. Add listeners - select port, protocol, and client affinity (for stateful applications)

0. Add End Point groups - select region, % of traffic, and health checks

0. Add Endpoints - can be ALB, NLB, EC2 instance, or EIP

### Workflow to delete a Global Accelerator

0. Disable Accelerator

0. Delete Accelerator

## Networking Costs

Within AZ, private IP? Free

Cross AZ, private more? costs cheap.

Cross AZ, public IP? costs more (because it accesses Internet & also goes through IGW)

Cross regions? costs even more.

## NAT vs Bastion

Need to provide access to Internet from private subnet? NAT Gateway

Need to provide RDP/SSH Agent forwarding? Bastion

Need a HA bastion? NLB (or Classic but NOT ALB) on a static IP, bastion in each AZ (sort of expensive) OR Bastion in single AZ with an EIP that can be moved using a user data script (less expensive but will result in downtime)


# Key Resources

- [Network Admin Guide to VPC](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Introduction.html)

- [Deep dive: Direct Connect & VPNs](https://www.youtube.com/watch?v=Qep11X1r1QA)

