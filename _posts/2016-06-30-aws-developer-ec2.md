---
layout: post
title: "AWS Developer - EC2"
description: ""
category: posts 
tags: [aws, developercert, ec2, solutionsarch, devopspro]
---
{% include JB/setup %}

AWS [Elastic Compute Cloud (EC2)](http://aws.amazon.com/documentation/ec2/) is the grand-pappy of cloud computing products. In a nutshell, EC2 enables you to create any number of virtual computers with a huge variety of software preloaded on images. 

## Purchase Options
There are several purchase options for EC2: 

* On-Demand Instances - need an instance? Rent an instance. Most expensive.

* Reserved Instances - cheaper and guaranteed for the availability zone you need that is a steady state and predictably used; If your needs change, you can modify or exchange reserved instances, or list eligible Standard Reserved Instances for sale on the Reserved Instance Marketplace.

* Spot Instances - you set a max price and if the prices rises about your instance gets nuked; A Spot instance runs as long as its capacity is available and your bid price is higher than the Spot price... may get terminated. If Amazon terminates then you don't get charged for partial; if you terminate you DO get charged.

* Free Instance - free, yet small, and mostly `t2.micro`

You can modify your whole reservation, or just a subset, in one or more of the following ways:

* Change Availability Zones within the same region

* Change the scope of the reservation from Availability Zone to Region (and vice-versa)

* Change between EC2-VPC and EC2-Classic

* Change the instance size within the same instance type

## Dedicated Hosts

Launch configuration for instances are as follows:

- Default - shared hardware; can't be changed after launch

- Dedicated - single tenant hardware; can be changed to Host

- Host - Instance runs on dedicated host; can be changed to Dedicated

So the tenancy of an instance can only be change between variants of dedicated tenancy hosting.

If the VPC is setup dedicated, then all instances are dedicated; can't be changed at launch.

## Instance Types

Each instance type offers different compute, memory, and storage capabilities and are grouped in instance families based on these capabilities. Instances can also be classified as burstable or consistent. DIRTMCG is the mnemonic!

* d - Dense storage; mechanical disks; data ware housing &amp; and distributed file systems rock here

* i - IOPS storage; lots of SSD storage; big data or DB

* r - RAM oriented; local SSD instance store

* t - Tiny general (burstable CPU performance); HVM virtualization only

* m - Main choice or general purpose with consistent performance; m3 offer instance stores; m4 uses EBS optimized stores as the default

* c - Compute/processor optimized

* g - Graphics and GPU optimized; good for video encoding; SSD instance store

## Instance Features

EBS Optimization - dedicated bandwidth, consistency, with higher throughput

enhanced networking - SR-IOV (which reduces CPU interupts therefore improving overall performance)

Instance Store Volumes - no resiliance but high throughput and high IO

GPU availability - only avialable on the G class of instances

## Virtualization Types

HVM - Emulates a bare-metal experience for virtualization; originally very slow but improved after CPU vendors added support for virtualization instructions and soon network & I/O as well. Enhanced networking features generally require HVM. Can run for Windows. 

PV - Short for ParaVirtualization is the previous version of virtualization. Uses a special boot loader called PV-GRUB and does not run Windows. In general, poor performance is the name of the game yet attactive spot pricing can be attractive.

## Status Checks

System Status Checks - generally networking, power, hardware and system software problems that can be resolved by stopping and restarting the instance or contacting AWS. Checks the host.

Instance Status Checks - configuration problems, out of memory, corrupted file system, or incompatible kernel. AWS does NOT help you with these sort of problems. Checks the instance.

## Storage

Performance for EBS is measured in IOPS which is a complicated metric. For SSDs,  IOPS are measured in 256 Kibibytes, because they are so good a small, non-sequential reads. For HDDs, IOPS are measured in 1024 Kibibyte chunks, because the have better performance with large sequential reads. 

## Root Storage

Instance storage - physically attached to the host but ephemeral; Root devices are not encrypted; Root devices are deleted by default with the instance is terminated but this can be changed.

There tons of details about EBS.. so much that I made a separate [post about it]({{ BASE_PATH }}/posts/aws-sysops-elastic-block-storage)

## Instance Meta-Data

There is a ton of meta-data available about each EC2 instance available via this [handy URL: curl http://169.254.169.254/latest/meta-data/](http://169.254.169.254/latest/meta-data/)

## Placement Groups

A placement group is a logical group of instances within a single AZ that participate in a low latency 10Gbps network. The name for the group must be unique within your account and only certain types of instances can be launched in a placement group and ideally only ONE type of instance per placement group. You can't merge groups or move instances into a group or make a placement group span VPC.

## EC2 API Reference Points

The EC2 API is huge and covers creating and deploying AMI, instances, instance specifics like EBS, Elastic IP, etc. You can manage dedicated, spot, spot fleet, &amp; reserved instances. You can setup an entire VPC using this API. Amazing stuff.

### EC2 Metrics

* CPUUtilization - duh

* DiskReadOps & DiskWriteOps - total operations (not bytes) to and from disk (instance store)

* DiskReadBytes & DiskWriteBytes - total bytes to and from the disk (instance store)

* NetworkIn & NetworkOut - total bytes to and from the instance over the wire

* NetworkPacketsIn &amp; NetworkPacketsOut - number of packets in and out instead of bytes

* StatusCheckFailed_Instance &amp; StatusCheckFailed_System - has anything failed an instance or system check?

* StatusCheckFailed - combination of both status checks... 1 = yes; 0 = no

### EC2 Metric Dimensions &amp; Custom Metrics

These metrics, except for AutoScalingGroupName, are only available with detailed monitoring therefore update every minutes vs the normal 5 minute interval . The metrics include: AutoScalingGroupName, ImageId, InstanceId, InstanceType

The big reason to use custom metrics and to push metrics to cloudwatch in general is the aggregation of data in cloudwatch instead of all over the place.. this requires an EC2 role to be setup.

```bash
#!/bin/bash
curl https://s3.amazonaws.com//aws-cloudwatch/downloads/latest/awslogs-agent-setup.py -O
chmod +x ./awslogs-agent-setup.py
sudo python ./awslogs-agent-setup.py --region us-east-1 -n -c [bucket-for-log-files]
```

### AMI API Highlights

| **AMI**  | **Notes**  |
|:-----------------------------------------|:--------------------------------------------------------|
| [DescribeImages](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeImages.html) | Describes one or more of the images (AMIs, AKIs, and ARIs) available to you|
| AttachVolume | EBS volume to an EC2 instance |
| RegisterImage | Registers an AMI. When you're creating an AMI, this is the final step you must complete before you can launch an instance from the AMI.|

### Instance API Highlights
All actions are pretty much plural. 

| **Instance**  | **Notes**  |
|:-----------------------------------------|:--------------------------------------------------------|
|RunInstances|Launches the specified number of instances using an AMI for which you have permissions.|
|DescribeInstances| Describes one or more of your instances.|

# Resources



## Qwik Labs
[Introduction to Amazon Elastic Compute Cloud (EC2)](https://qwiklabs.com/focuses/2921)

[Introduction to Amazon Elastic Block Store (EBS)](https://qwiklabs.com/focuses/2920)

[Creating Amazon EC2 Instances (for Linux)](https://qwiklabs.com/focuses/2548)

## Reading
[EC2 User Guide](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)

[EBS FAQ](https://aws.amazon.com/ebs/faqs/)

[ELB Overview](https://aws.amazon.com/elasticloadbalancing/)

[AMI Bundling](https://aws.amazon.com/articles/Amazon-EC2/9001172542712674)

## Videos
[EC2 Master Class](https://www.youtube.com/watch?v=jLVPqoV4YjU)

