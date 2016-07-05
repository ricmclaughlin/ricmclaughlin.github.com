---
layout: post
title: "A Simple VPC from Scratch"
description: ""
category: projects
tags: [aws, developercert, vpc]
---
{% include JB/setup %}

Creating an [Amazon Web Services ](https://aws.amazon.com/) virtual private cloud is about the most amazing systems engineering feat ever. Using a good, yet not great, website anyone with basic technical skills can create computing infrastructure with security that would have cost hundreds of thousands of dollars just years ago - and I know this because I ran the IT group at a SaaS startup some years ago and spents millions of dollars in hardware and software and 4 or 5to accomplish what would take zero dollars to setup and way, way less than 1 million dollars a year to run.

The VPC we are going to create features a single with a private subnet and a single public subnet with an Internet Gateway and routing table.

Steps:

1. Create VPC - access the VPC dashboard and click on "Your VPCs" then "Create VPC". When presented with the "Create VPC" modal dialog name the VPC something snappy like "FromScratchVPC", I'd recommend always starting with a CIDR block of "10.0.0.0/16" so there is plenty of class C subnets avaible, leave the "Tenancy" setting on "Default" which allows your VPC to run on shared hardware and then click "Yes, Create".

1. Create subnets - All subnets are private until a Internet Gateway is attached so to create the two required subnets, from the VPC dashboard all we need to do is click on "Subnets" then on "Create Subnet". From here name the first subnet "FromScratchVPC-Public", from the VPC dropdown select "FromScratchVPC", from the "Availability Zone" select a zone, and in the "CIDR block" edit control add "10.0.1.0/24" then click "Yes, Create". To create the second subnet, from the VPC dashboard all we need to do is click on "Subnets" then on "Create Subnet". From here name the first subnet "FromScratchVPC-Privatee", from the VPC dropdown select "FromScratchVPC", from the "Availability Zone" select a zone, and in the "CIDR block" edit control add "10.0.2.0/24" then click "Yes, Create".

1. Internet Gateway - To create an Internet Gateway for the public subnet, from the VPC dashboard and click on "Internet Gateway" and in the "Create Internet Gateway" modal dialog type in "FromScratchVPC-Gateway" and then click "Yes, Create". From this point refresh the "Internet Gateway" list and the "FromScratchVPC-Gateway" should show up in a "detached" state. From the "Internet Gateway" list select the "FromScratchVPC-Gateway" and associate the gateway to the "FromScratchVPC" VPC. 

1. Create route table - Now associate the "FromScratchVPC-Gateway" with the public subnet by accessing the "Route Table" menu item on the left then click on "Create Route Table" and create a new route table called "FromScratchVPC-RouteTable". Now select the "FromScratchVPC-RouteTable" and click on the "Routes" tab in the route detail and click on "add". In the "Destination" edit control add "0.0.0.0/0" and select the gateway as the target. Now click on "Subnet Associations" and associate the gateway with the "FromScratchVPC-Public" subnet.

That's it. Very straight forward. What is not clear, but evident after a bit of experimentation, is that the private subnet can communicate with the public subnet but the private subnet can't communicate with the open Internet.
