---
layout: post
title: "AWS - Snowball"
description: ""
category: posts
tags: [aws, snowball, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

[Import/Export Snowball](https://aws.amazon.com/importexport/)

AWS used to offer the ability to send them a disk and have them import the disk to EBS, S3 or Glacier then to possibily export the data to S3 (only most recent version is exported if versioned.) This had to be totally painful so now they are a specially created device called an import/export Snowball - a portable NAS you can ship back to AWS and they import the data to S3. Import encryption is optional; exported data is encrypted. Data can only be exported from S3.

