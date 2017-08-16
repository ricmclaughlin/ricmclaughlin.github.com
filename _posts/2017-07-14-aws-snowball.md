---
layout: post
title: "AWS - Snowball"
description: ""
category: posts
tags: [aws, snowball, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

[Import/Export Snowball](https://aws.amazon.com/importexport/) AWS offers the ability to send them a disk and have them import the disk to _EBS, S3 or Glacier_ then to possibily export the data to _S3_ (only most recent version is exported if versioned.) 

Because shipping disks around is silliness and this had to be totally painful, AWS has a specially created device called an import/export Snowball - a portable NAS you can ship back to AWS and they import the data to S3. Import encryption is optional; exported data is encrypted. Data can only be exported from S3.

