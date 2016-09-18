---
layout: post
title: "AWS Developer Certification - EC2"
description: ""
category: posts 
tags: [aws, developercert, ec2]
---
{% include JB/setup %}

AWS [Elastic Compute Cloud (EC2)](http://aws.amazon.com/documentation/ec2/) is the grand-pappy of cloud computing products. In a nutshell, EC2 enables you to create any number of virtual computers with a huge variety of software preloaded on images. 

## Purchase Options
There are several purchase options for EC2: 

* On-Demand Instances - need an instance? Rent an instance. Most expensive.

* Reserved Instances - cheaper and guaranteed for the availability zone you need that is a steady state and predictably used

* Spot Instances - you set a max price and if the prices rises about your instance gets nuked; A Spot instance runs as long as its capacity is available and your bid price is higher than the Spot price... may get terminated. If Amazon terminates then you don't get charged for partial; if you terminate you DO get charged.

* Free Instance - free, yet small and mostly tiny

## Instance Types
Each instance type offers different compute, memory, and storage capabilities and are grouped in instance families based on these capabilities. DIRTMCG is the mnemonic!

* d - Dense storage

* i - IOPS storage

* r - RAM oriented

* t - Tiny general

* m - main choice or general purpose

* c - Compute/processor optimized

* g - Graphics optimized

## Storage

## Root Storage
Instance storage - physically attached to the host but ephemeral; Root devices are not encrypted; Root devices are deleted by default with the instance is terminated but this can be changed.
There are three types of root storage:

1. General purpose SSD -> up to 10K IOPS

2. Provisioned IOPS -> over 10k IOPS

3. Magnetic - Cheap and slow

## EBS volumes 
Elastic Block Storage - permanent storage with snapshot capability automatically mounted. There are three different volume types including general purpose, provisioned IOPS SSD which can be striped together for faster performance and magnetic volumes. Generally mapped to /dev/sda1 and can be optionally encrypted.

EBS volumes can also be Cold HDD and Throughput Optimized HDD.

## Elastic Load Balancers (ELB) 
ELB distributes traffic to instances that belong to the ELB group; allows us to offload SSL certs to load balancers instead of webservers! There are three `Cookie Stickiness` options both enabled options end up with sticky sessions (so all sessions go back to the same server). The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance.

ELB configuration requires a protocol, a front end port, and a back end port. There are two version of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL) and Application Load Balancer (any port - just HTTP, HTTPS).

ELB are NOT free - there is a charge by the hour and per GB of usage.

## Instance Meta-Data
There is a ton of meta-data available about each EC2 instance available via this [handy URL: curl http://169.254.169.254/latest/meta-data/](curl http://169.254.169.254/latest/meta-data/)

# Resources
## Qwik Labs
[Introduction to Amazon Elastic Compute Cloud (EC2)](https://qwiklabs.com/focuses/2921)

[Introduction to Amazon Elastic Block Store (EBS)](https://qwiklabs.com/focuses/2920)
[Creating Amazon EC2 Instances (for Linux)](https://qwiklabs.com/focuses/2548)

## Reading
[EC2 User Guide](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
[EBS FAQ](https://aws.amazon.com/ebs/faqs/)
[ELB Overview](https://aws.amazon.com/elasticloadbalancing/)


