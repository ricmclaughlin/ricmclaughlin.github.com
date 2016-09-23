---
layout: post
title: "A Simple VPC from Scratch"
description: ""
category: projects
tags: [aws, developercert, vpc]
---
{% include JB/setup %}

Creating an [Amazon Web Services ](https://aws.amazon.com/) virtual private cloud is about the most amazing systems engineering feat ever. Using a good, yet not great, website anyone with basic technical skills can create computing infrastructure with security that would have cost hundreds of thousands of dollars just years ago - and I know this because I ran the IT group at a SaaS startup some years ago and spents millions of dollars in hardware and software and 4 or 5 months to accomplish what now takes zero dollars to setup and way, way less than 1 million dollars a year to run.

The VPC we are going to create features a single with a private subnet and a single public subnet with an Internet Gateway and routing table. I'm creating this VPC in the Oregon so I'm using the "us-west-2" as part of the name.

# Steps to create a simple VPC:

## 1. Create VPC 
Access the VPC dashboard and click on "Your VPCs" then "Create VPC". When presented with the "Create VPC" modal dialog name the VPC something snappy like "FromScratchVPC", I'd recommend always starting with a CIDR block of "10.0.0.0/16" so there is plenty of class C subnets available, leave the "Tenancy" setting on "Default" which allows your VPC to run on shared hardware and then click "Yes, Create".

## 2. Create Subnets
All subnets are private until a Internet Gateway is attached so to create the two required subnets, from the VPC dashboard all we need to do is click on "Subnets" then on "Create Subnet". 

- From here name the first subnet "10.0.1.0 - us-west-2a", from the VPC dropdown select "FromScratchVPC", from the "Availability Zone" select the "us-west-2a" AZ , and in the "CIDR block" edit control add "10.0.1.0/24" then click "Yes, Create". This will be our public subnet.

- To create the second subnet, from the VPC dashboard all we need to do is click on "Subnets" then on "Create Subnet". From here name the first subnet "10.0.2.0 - us-west-2b", from the VPC dropdown select "FromScratchVPC", from the "Availability Zone" select the 2b zone, and in the "CIDR block" edit control add "10.0.2.0/24" then click "Yes, Create". This is our first private subnet.

- To create the third subnet, from the VPC dashboard all we need to do is click on "Subnets" then on "Create Subnet". From here name the first subnet "10.0.0.0 - us-west-2c", from the VPC dropdown select "FromScratchVPC", from the "Availability Zone" select the 2c zone, and in the "CIDR block" edit control add "10.0.2.0/24" then click "Yes, Create". This is our second private subnet.

## 3. Assign Internet Gateway 
To create an Internet Gateway for the public subnet, from the VPC dashboard and click on "Internet Gateway" and in the "Create Internet Gateway" modal dialog type in "FromScratchVPC-Gateway" and then click "Yes, Create". From this point refresh the "Internet Gateway" list and the "FromScratchVPC-Gateway" should show up in a "detached" state. From the "Internet Gateway" list select the "FromScratchVPC-Gateway" and associate the gateway to the "FromScratchVPC" VPC.

## 4. Create route table 
Now associate the "FromScratchVPC-Gateway" with the public subnet "10.0.1.0 - us-west-2a" by accessing the "Route Table" menu item on the left then click on "Create Route Table" and create a new route table called "FromScratchVPC-RouteTable". Now select the "FromScratchVPC-RouteTable" and click on the "Routes" tab in the route detail and click on "add". In the "Destination" edit control add "0.0.0.0/0" and select the gateway as the target. Now click on "Subnet Associations" and associate the gateway with the "10.0.1.0 - us-west-2a" subnet.

## 5. Launch a Couple of Instances
WebServer-DMZ - Pick the Amazon AMI, t.2, add it to the "FromScratchVPC" VPC, the public subnet "10.0.1.0 - us-west-2a", auto assign a public IP address, add an HTTP to the security group, pick a key, then launch it.

DBServer - Pick the Amazon AMI, t.2, add it to the "FromScratchVPC" VPC, the public subnet "10.0.1.0 - us-west-2b", no public IP address, add an HTTP to the security group, pick a key, then launch it.

PROBLEM - DB Server can't reach Internet to update. Solution: Create NAT gateway..

## Create NAT Gateway
First create a new SG enable http/http inbound/outbound

Create NAT instance using NAT AMI, use SG, no public IP, public subnet

Assign Elastic IP

## Change Route Table



## Summary
That's it.
