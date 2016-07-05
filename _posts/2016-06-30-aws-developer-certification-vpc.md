---
layout: post
title: "AWS Developer Certification - VPC"
description: ""
category: posts
tags: [aws, developercert, vpc]
---
{% include JB/setup %}

[Virtual Private Clouds](https://aws.amazon.com/vpc/) are foundational infrastructure components much like [IAM](https://aws.amazon.com/iam/) and CloudFormation. 

What is a VPC? imagine a in-house network with routers, subnetting, routing tables and computing resources... except in the cloud. 

A default VPC gives users easy access to computing resources with now configuration at all. The default VPC has a default internet gateway, a default private IP and a default public IP. A default VPC can be deleted and can only be restored by contacting AWS.

VPC peering - allow you the ability to directly route between two VPC using private IP addresses - this might also be configured to share VPC between AWS accounts.

VPC creation - Tenancy - using the default option all hardware is shares using hypervisor and other sharing technologies; using the "dedicated" option makes everything, including EC2 instances, on dedicated hardware - think more expensive.

Non-Default VPC - By default, an instance in a nondefault VPC is not assigned a public IP address

Security Groups - firewall port filtering at the instance level renamed; allows filtering inbound and outbound; return traffic is allowed so it is stateful; supports allow rules only

Network Access Control List - firewall port filtering at the subnet level renamed; allows filtering inbound and outbound; return traffic is not specfically allowed so it is stateless; Each ACL rule has a number and rules are evaluated ascending; Best practice is to create rule numbers by 5 so other rules can be added later; Once rules are added all other traffic is denied by default

Customer Gateway - a vpn endpoint inside a VPC

NAT Instance - actually a EC2 instances; not exactly sure why you would use this... A NAT Gateway 

NAT Gateway

RDS instances in a VPC - 

Elastic IP Addresses - 

Elastic Networks Interfaces - 





