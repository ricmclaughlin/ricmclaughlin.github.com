---
layout: post
title: "AWS Project - NAT Instance in a VPC"
description: ""
category: projects
tags: [aws, vpc]
---
{% include JB/setup %}

The giant problem with private subnets in a VPC is lack of Internet access from EC2 instances in the subnet - how exactly are you going to update the OS and other packages on those instances? One answer, but probably not the best answer is to use a NAT instance in the private subnet. I'm going to walk you through how to create a NAT instance now!

Steps to create a NAT instance:
1. First, DON'T DO IT. - why would I create a EC2 instance to manage instead of using an AWS fully managed service... and quite candidly, you don't want to create a NAT instance in a VPC - you want a NAT gateway! Clearly this is a legacy system that will be deprecated in the long term.

Steps to create a NAT gateway:

To get this all setup properly you are going to need to follow the instruction to create a [simple VPC]({{ BASE_PATH }}/posts/simple-vpc-from-scratch) and create an EC2 instance in each subnet. The result of that setup should yield an EC2 instance that is addressable from the Internet (called 'EC2-public') and an EC2 instance that is inaccessible from the Internet (called 'EC2-private')

1. From the VPC dashboard, click on the "NAT Gateway" then on the "Create NAT Gateway" button and create the gateway with a new Elastic IP address. From here click the "Edit Route Tables" which should take you to the "Route Table" list.

2. From the "Route Tables" list select the default route table, which should have no subnets associated with it, and associate the "0.0.0.0/0" route with the new NAT gateway.

And then you are up and running. To confirm this worked `ssh` into the 'EC2-private' server and then do some action that would require Internet access!
