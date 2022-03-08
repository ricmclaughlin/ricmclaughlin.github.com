---
layout: post
title: "AWS - Snowball"
description: ""
category: posts
tags: [aws, snowball, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

[Import/Export Snowball](https://aws.amazon.com/importexport/) AWS offers the ability to send them a disk and have them import the disk to _EBS, S3 or Glacier_ then to possibily export the data to _S3_ (only most recent version is exported if versioned.) Can be as little as one-fifth as cheap to use the 50TB or 80TB unit and secure by way o fhte Trusted Platform Module (TPM).

Because shipping disks around is silliness and this had to be totally painful, AWS has a specially created device called an import/export Snowball - a portable NAS you can ship back to AWS and they import the data to S3 at the petabyte scale. Import encryption is optional; exported data is encrypted. Data can only be exported from S3.

The AWS Snowball Edge is a 100TB data transfer device with onboard compute capabilities.

Consider using Snowball when there is more than 2TB of data for a T3 connection, 5TB for a 100MB connection, and 60TB for 1000Mbps connections.