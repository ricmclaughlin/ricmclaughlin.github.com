---
layout: post
title: "AWS Developer Certification - VPC"
description: ""
category: posts
tags: [aws, developercert, vpc]
---
{% include JB/setup %}
# Knowledge

## What is a VPC?
Imagine a in-house network with routers, sub-netting, routing tables and computing resources... except in the cloud. [Virtual Private Clouds](https://aws.amazon.com/vpc/) are foundational infrastructure components much like [IAM](https://aws.amazon.com/iam/) and CloudFormation.

## Why use a VPC?



## Types of VPC:
* default VPC - A default VPC gives users easy access to computing resources with now configuration at all. The default VPC has a default internet gateway, a default private IP and a default public IP. A default VPC can be deleted and can only be restored by contacting AWS.

* Non-Default VPC - By default, an instance in a non-default VPC is not assigned a public IP address

### VPC Tenancy
When creating a new VPC and using the default options, all hardware is shares using hypervisor and other sharing technologies; using the "dedicated" option makes everything, including EC2 instances, on dedicated hardware - think more expensive.

## VPC Networking
* [Elastic IP Addresses](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) - A static IP address that can easily be allocated and remapped to another computer resource; 5 per AWS account by default.

* Elastic Networks Interfaces - a virtual network interface that you can remap and assign to instances; dual homing would be a great way to think about it.

* Routing Tables - each subnet has only one routing table but one routing table can be shared by many subnets

* Customer Gateway - a vpn endpoint inside a VPC that allows computers in the corporate network access the datacenter resources

* VPC peering - allow you the ability to directly route between two VPC using private IP addresses - this might also be configured to share VPC between AWS accounts.

* Network Address Translation - EC2 instances in a private subnet can't access the Internet therefore can not access updates via a package manager. Both a NAT instance and NAT gateway can solve this problem. A NAT instance, which is a special purpose EC2 instances, is one solution but a bit old school. A [NAT Gateway](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html) is a fully managed AWS service that is way more practical and easier to implement.

## VPC Security
* Security Groups - firewall port filtering at the instance level renamed; allows filtering inbound and outbound; return traffic is allowed so it is state-ful; supports allow rules only

* Network Access Control List - firewall port filtering at the subnet level renamed; allows filtering inbound and outbound; return traffic is not specifically allowed so it is stateless; Each ACL rule has a number and rules are evaluated ascending; Best practice is to create rule numbers by 5 so other rules can be added later; Once rules are added all other traffic is denied by default

## RDS
RDS instances in a VPC - There is a mythical sort of subnet, called a [DB Subnet Group](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html), that must span multiple availability zones and can house RDS instances.  

# Resources
## Qwik Labs
* [Introduction to Amazon Virtual Private Cloud (VPC)](https://qwiklabs.com/focuses/2546?search=124038) - good basic setup of a VPC

* [Building Your First Amazon Virtual Private Cloud (VPC)](https://qwiklabs.com/focuses/2546?search=124048) - this one includes some windows specific stuff like SQL server and even a bastion host. Ugly stuff.

* $$[Creating an Amazon Virtual Private Cloud (VPC) with AWS CloudFormation](https://qwiklabs.com/focuses/2640?search=151921) - need to complete

## Reading

