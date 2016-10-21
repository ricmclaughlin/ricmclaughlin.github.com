---
layout: post
title: "AWS SysOps - Networking"
description: ""
category: posts
tags: [aws, sysopscert, networking]
---
{% include JB/setup %}


## Route53
Route53 is an authoritative DNS service so it will translate domain names to IP addresses. Each domain is called a hosted zone. A record set is a rule within the hosted zone. 

### Route53 Health Checks
Health checks can monitor an end point, the status of CloudWatch Metric or Alarm and when the healthcheck fails the thingy ( s3 bucket, ELB or instance) is taken out of the DNS group.

### Routing Policy 
There are 5 different ways for Route53 to do its thing....route.
  
* Simple - like the name says - route to an address or a bucket
  
* Weighted - a percentage to one location and a different percentage to another; expressed as a integer value relative to one or more other integer values

* Latency - Use the latency routing policy when you have resources in multiple Amazon EC2 data centers that perform the same function and you want Amazon Route 53 to respond to DNS queries with the resources that provide the best latency.
  
* Failover - Use the failover routing policy when you want to configure active-passive failover, in which one resource takes all traffic when it's available and the other resource takes all traffic when the first resource isn't available.

* Geolocation - Use the geolocation routing policy when you want Amazon Route 53 to respond to DNS queries based on the location of your users.


## VPC
By default all instances within a subnet with a VPC can communicate.

To enable access to or from the Internet for instances in a VPC subnet, you must do the following:

*  Attach an Internet gateway to your VPC.

* Ensure that your subnet's route table points to the Internet gateway.

* Ensure that instances in your subnet have public IP addresses or Elastic IP addresses.

* Ensure that your network access control and security group rules allow the relevant traffic to flow to and from your instance.

Limits per Region

- 5 VPC per Region 

- 200 subnets per VPC

- 50 customer gateways

- 5 internet gateways

- 5 Elastic IP 

- 50 VPN connections

- 500 Security Groups


### DHCP


### Scenarios

* VPC with public subnet only - single tier apps; ie. Rails app, PosgreSQL on a single machine

* VPC with private and public subnets - multi-tiered apps; Rails app in public subnet; servers in public subnet can access private subnet; private subnet contains DB server

* VPC with public and private subnets with Hardware VPN - extension of on-premise network to VPC

* VPC with VPN - On-premise access to resources in VPC

### DB Subnet
A DB subnet defines the regions for a multi-AZ RDS setup. The high level steps to create a DB subnet are as follows:

1. Create 2 new subnets in a VPC

1. In RDS Dashboard create subnet group & add the subnets you just created

1. Now launch an RDS instance and  select Multi-AZ Deployment

1. In "configure advanced settings" select subnet group you created

Presto, you have a multi-AZ DB subnet!


## Elastic IP & Network Interfaces
Like Elastic IP addresses are independent of instances, Elastic Network Interfaces are independent of instances. The nifty trick here is that Elastic IP addresses can be associated with Elastic Network Interfaces.... which can be associated with EC2 instances. 

## VPN
To connect securely with your VPC on AWS you can create a Virtual Public Gateway and connect that to a Customer Gateway forming a VPN. Dual VPN tunnels provide higher availability. Limits on VPN include:

- 5 Virtual Private Gateways per region

- 1 Virtual Private Gateways per VPC

- 50 Customer Gateways per Region

If you traffic is in excess of 4 Ggps, you need AWS Direct Connect which can provide up to 10Ggps. Direct Connect uses BGP and an Autonomous System Number (ASN) and IP prefixes. 
