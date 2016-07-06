---
layout: post
title: "AWS Developer Certification - EC2"
description: ""
category: posts 
tags: [aws, developercert]
---
{% include JB/setup %}

AWS [Elastic Compute Cloud (EC2)](http://aws.amazon.com/documentation/ec2/) is the grand-pappy of cloud computing products. In a nutshell, EC2 enables you to create any number of virtual computers with a huge variety of software preloaded on images. 

There are several purchase options for EC2: 

* on-Demand Instances - need an instance? buy an instance. Most expensive.

* Reserved Instances - cheaper and guaranteed for the availability zone you need

* Spot instances - you set a max price and if the prices rises about your instance gets nuked; A Spot instance runs as long as its capacity is available and your bid price is higher than the Spot price... 

* Free instance - free, yet small and mostly tiny

Instance types - Each instance type offers different compute, memory, and storage capabilities and are grouped in instance families based on these capabilities. t & m types are general purpose, the c type is processor oriented, r is memory oriented, i & d are storage optimized and g is GPU optimized. 

EBS volumes - permanent storage with snapshot capability automatically mounted. There are three different volume types including general purpose, provisioned IOPS SSD which can be striped together for faster performance and magnetic volumes. Generally mapped to /dev/sda1.

Instance storage - physically attached to the host but ephemeral

Elastic Load Balancers - distributes traffic to instances that belong to the ELB group; allows us to offload SSL certs to load balancers instead of webservers! There are three `Cookie Stickiness` options both enabled options end up with sticky sessions (so all sessions go back to the same server). The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance.
