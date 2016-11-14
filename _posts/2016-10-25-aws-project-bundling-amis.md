---
layout: post
title: "AWS Project - Bundling AMIs"
description: ""
category: projects
tags: [aws, how-to]
---
{% include JB/setup %}

## What is "Bundling an AMI"
An AMI is a computer image configured for a specific purpose. Using an AMI you can easily create a new EC2 instance. The AMI includes a template for the root volume, launch permissions and a block device mapping.

 - AMI:EC2 Instances::blueprint:Sky Scraper

## Why Would You Bundle an AMI?
Bundling your own AMI has several advantages:

- Decreases time from launch to "running state"

- Standardize the operating environment for an type of EC2 Instance

- Make ELB possible - you gotta use AMIs to spin up new EC2 Instances!

One key point: AMIs are regional; you can copy AMIs to another region but they get renamed.

## How to Bundle an AMI
This is a very straightforward process assuming you have a running instance:

1. Configured the instance just how you like it.

1. In the EC2 section of the consolue, then select the instance. 

2. Click the "actions" button then "create image"

3. Name the image, give it a description, add any additional storage you might require

4. Wait a bit, and bam you have a new AMI. 

## Best Practices

Sharing AMIs is a lovely practice. I might recommend following these steps to semi-harden your gift to other AWS folks:

1. Update the AMI tools at boot time

2. Disable password logins for root

3. Disable local root access

4. Remove SSH host key pairs

3. Install Public Key Credentials

Launch permissions, S3 bucket permissions, and user-defined tags must be copied manually to an instance based on an AMI. User data is part of the AMI, itself, and does not need to be copied manually.

## Resources

[A Friendly guide for AMI Bundling](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sharing-amis.html)

[QwikLab: Bundling Amazon EBS-Backed AMIs](https://qwiklabs.com/focuses/2547)