---
layout: post
title: "AWS Developer - EC2"
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

* t - Tiny general (burstable CPU performance)

* m - main choice or general purpose; m3 offer instance stores; m4 uses EBS backed stores

* c - Compute/processor optimized

* g - Graphics optimized

## Virtualization Types

HVM - Emulates a bare-metal experience for virtualization. Can runs for Windows. 

PV - Short for ParaVirtualization is the previous version of virtualization. Uses a special boot loader called PV-GRUB and does not run Windows.





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

## EC2 API Reference Points
The EC2 API is huge and covers creating and deploying AMI, instances, instance specifics like EBS, Elastic IP, etc. You can manage dedicated, spot, spot fleet &amp; reserved instances. You can setup an entire VPC using this API. Amazing stuff.

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

