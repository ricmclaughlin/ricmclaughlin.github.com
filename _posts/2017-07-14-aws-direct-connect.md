---
layout: post
title: "AWS - Direct Connect"
description: ""
category: posts
tags: [aws, direct-connect, aws-solutions-arch-pro, networking]
---
{% include JB/setup %}

# Direct Connect

[Direct Connect](https://aws.amazon.com/directconnect/)(DX) makes it easy to connect an on-premise data center with AWS. Instead of running AWS bound traffic out your regular Intenet connection, Direct Connect enables a 1 or 10 GB dedicated connections to be setup using industry standard 802.1 VLANS to your VPC - provided you remembered to subnet and address your VPC correctly! 

An AWS Direct Connect location provides access to AWS in the region with which it is associated. You can provision a single connection to any AWS Direct Connect location in North America and use it to access public AWS services in all North America regions and AWS GovCloud (US). 

Direct connect is not redundant by default but can be if you use BGP to have multiple direct connects in either an active-active (recommended) or active-failover setup with two CGW. Layer 2 connections are not supported.

There are lower bandwidths of connection available through partners using VLAN trunking - 802.1Q but no SLA available. 

Overall, Direct Connect gives you some great benefits:

1. Reduce your Internet Bandwidth cost - you are probably paying a lot of money to an ISP and could get a cheaper connection directly to AWS

1. Consistently higher bandwidth and lower latency 

## Direct Connect Components

Customer Gateway is the customer side of the connection. Virtual Private Gateways are the VPC end of the direct connect setup.

Virtual Interfaces (VIF) - the connection similiar to a tunnel for a VPN. They come in two flavors:

- private VIF - used to access VPC using internal IP addresses or private VPC or S3 VPC endpoints

- public VIF - allows public connections to EC2 using public IP addresses, public IP addresses or S3 public end points; requires an ASN, VLAN, a public CIDR block range 

## Transition from VPN to Direct Connect

Once Direct Connect line is installed, configure DX and the VPN to occupy the same BGP Community. Then configure BGP to assign a higher cost the VPN and the traffic will flow through the DX line.
