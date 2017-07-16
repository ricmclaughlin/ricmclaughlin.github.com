---
layout: post
title: "AWS - Direct Connect"
description: ""
category: 
tags: [aws, direct-connect, aws-solutions-arch-pro]
---
{% include JB/setup %}
# Direct Connect

[Direct Connect](https://aws.amazon.com/directconnect/) makes it easy to connect an on-premise data center with AWS. Instead of running AWS bound traffic out your regular Intenet connection, Direct Connect enables a 1 or 10 GB dedicated connections to be setup using industry standard 802.1 VLANS to your VPC - provided you remembered to subnet and address your VPC correctly! There are lower bandwidths of connection available through partners but no SLA available. You can use BGP to have multiple direct connects in either an active-active (recommended) or active-failover setup. Overall, Direct Connect gives you some great benefits:

1. Reduce your Internet Bandwidth cost - you are probably paying a lot of money to an ISP and could get a cheaper connection directly to AWS

1. Consistently higher bandwidth and lower latency 

1. No VPN is required to secure the VPC and no public address is required to access instances in the VPC.

## Direct Connect Components

Customer Gateway is the customer side of the connection. Virtual Private Gateways are the VPC end of the direct connect setup.

Virtual Interfaces (VIF) - the connection. They come in two flavors:

- private VIF - used to access VPC using internal IP addresses or private VPC

- public VIF - allows public connections to EC2 or S3 or public subnet; requires a public CIDR block range 

In the US, a single direct connect connections can access all 4 US regions provided you use a public VIF, BGP and VPN.

### Direct connect vs VPN Capability

- VPN = quick to bring up and easy; slower and sucks Internet bandwidth

- Direct Connect = consistent lower latency and fixed bandwidth; more secure; lower cost