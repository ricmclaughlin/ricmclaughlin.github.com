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

* t - Tiny general (burstable CPU performance)

* m - main choice or general purpose; m3 offer instance stores; m4 uses EBS backed stores

* c - Compute/processor optimized

* g - Graphics optimized

## Virtualization Types

HVM - Emulates a bare-metal experience for virtualization. Can runs for Windows. 

PV - Short for ParaVirtualization is the previous version of virtualization. Uses a special boot loader called PV-GRUB and does not run Windows.

## Status Checks

System Status Checks - generally networking, power, hardware and system software problems that can be resolved by stopping and restarting the instance or contacting AWS.

Instance Status Checks - configuration problems, out of memory, corrupted file system, or incompatible kernel. AWS does NOT help you with these sort of problems.

## Storage

Performance for EBS is measured in IOPS which is a complicated metric. For SSDs,  IOPS are measured in 256 Kibibytes, because they are so good a small, non-sequential reads. For HDDs, IOPS are measured in 1024 Kibibyte chunks, because the have better performance with large sequential reads. 

## Root Storage
Instance storage - physically attached to the host but ephemeral; Root devices are not encrypted; Root devices are deleted by default with the instance is terminated but this can be changed.

## EBS Volumes
Elastic Block Storage - permanent storage with snapshot capability automatically mounted. There are three different volume types including general purpose, provisioned IOPS SSD which can be striped together for faster performance and magnetic volumes. Generally mapped to /dev/sda1 and can be optionally encrypted. A long time ago, you needed to "pre-warm", the EBS... now you "initialize" it but it is not required for peak performance from the EBS volume. However, if you are restoring an EBS or mounting it for the first time you can do a `dd if={mount location} of=/dev/null bs=1M` to get it up and running fast or use the `fio` utility which reads from every block on the volume.

There are three types of root storage:

1. General purpose SSD (gp2) - Minimum of 100 IOPS then 3 IOPS per GB; can burst to 3000 IOPS if less than 1 TB. The max throughput is 160 MiB/s and this can end up being a bottleneck if read size is large. Even though you can boost performance, use if you are after less than 10K IOPS.

2. Provisioned IOPS (io1) - 30 IOPS per GB up to 20,000 IOPS; Steady consisten IOPS not really a burstable; The max throughput is 320 MiB/s; Use if you are after over 10k IOPS

3. Magnetic - Cheap and slow EBS volumes can also be Cold HDD (sc1) and Throughput Optimized HDD (st1).

## Elastic Load Balancers (ELB) 
ELB distributes traffic to instances that belong to the ELB group; allows us to offload SSL certs to load balancers instead of webservers! ELB configuration requires a protocol, a front end port, and a back end port. There are two version of ELB - Classic Load Balancer (only a few ports + TCP, HTTP, HTTPS, SSL) and Application Load Balancer (any port - just HTTP, HTTPS).

ELB are NOT free - there is a charge by the hour and per GB of usage.

Only one SSL Cert per ELB. The max number of requests that can be queued is 1024; it issues a `HTTP 503 Error` when it can process any more requests - call AWS if a huge traffic spike comes!

There are three `Cookie Stickiness`, with both enabled options end up with sticky sessions (so all sessions go back to the same server), options: 

* Disable Stickiness - The disable stickiness option is what you want and then you need to implement ElastiCache or an Amazon RDS instance for session.

* Enable Load Balancer Generated Cookies - the ELB generates the cookie and manages the distribution of traffic; can't set the duration of the distribution

* Enable Application Generated Cookie Stickiness - ELB generates a cookie that correlates to the application cookie.  

### External vs Internal Load Balancing
External = Public; Elastic IP; DNS
Internal = no public IP or Elastic IP; internal DNS name

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

## Videos
[EC2 Master Class](https://www.youtube.com/watch?v=jLVPqoV4YjU)

