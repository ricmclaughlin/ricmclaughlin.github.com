---
layout: post
title: "AWS Solutions Arch - KMS-HSM"
description: ""
category: posts
tags: [aws, kms]
---
{% include JB/setup %}

## Overview
Encryption Key Management is a major function of online systems that requires lots of organization and no one will ever notice your efforts if things go right. Yet, if you screw it up, the world will come crashing down. Enter, [KMS](https://aws.amazon.com/kms/) and its good buddy [CloudHSM](https://aws.amazon.com/cloudhsm/). 


# KMS
KMS uses asymmetric encryption (2 different keys). Symmetric is not supported.
Customer Master Key - CMK
Features:
Very durable - 
Quorum-Based Access
IAM Access controlled
Low Latency and High Throughput
Regional Independence - your key can't travel the world...
Integrated with AWS CloudTrail & Cloud watch


## Setup

Roles don't need access to 'admin' access the key to USE the key. Role, users and external users can be granted the ability to use the key. Keys can be rotated yearly and KMS magically uses the right key when decrypting data.






https://linuxacademy.com/cp/courses/lesson/course/447/lesson/2/completed/1/module/45

## Resources