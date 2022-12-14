---
layout: post
title: "AWS - Snow[Cone, Ball, Mobile]"
description: ""
category: posts
tags: [aws, snow-family, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

# [Snowball](https://aws.amazon.com/importexport/) 

The Snow family of product enables easy migration of terabytes of data to the cloud without limits in storage capacity or compute power OR do computing at the edge.

AWS OpsHub, an installable application, enables the management and monitoring Snow Family devices and launching services on these devices 

## Migration Use Cases
A snowball/cone a portable NAS you can ship back to AWS and they import the data to S3 at the petabyte scale. Import encryption is optional; exported data is encrypted. Data can only be exported from S3. 

Rule of thumb 👍: If it takes more than a week over the network, use a Snow device.

Snowcone is small (2.1 kg) and has 8TB of storage. Can be sent back to AWS and also includes an DataSync agent for over-the-Internet data transfer. 

Snowball Edge comes in two varieties: Storage optimized (80 TB) and Compute optimized (42 TB). Up to 15 can be chained together to do more work in parallel.

A Snowmobile is a semi-trailer that can move 100PB of data.

### Improving Transfer Performance
Use the [Amazon S3 Adapter for Snowball](https://docs.aws.amazon.com/snowball/latest/ug/snowball-transfer-adapter.html), which is being deprecated, to speed transfer. After that, here are several ways to improve performance, most impactful to least:
0. Multiple writes at a time
0. Batch up small files in an archive (zip)
0. Don't update files during transfer
0. Reduce local network traffic
0. Eliminate network hops (directly connect device to network)

## Edge Use Cases

Both SnowCones (2CPU, 4 GB RAM) and SnowBall Edge (52 vCPU, 208 GB ram, optional GPU, 42 TB of storage) work as edge processing units. Can run Lambda functions (using IoT Greengrass) or EC2 instances and can be leased for 1 -3 years.