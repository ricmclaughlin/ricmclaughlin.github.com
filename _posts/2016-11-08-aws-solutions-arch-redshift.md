---
layout: post
title: "AWS Solutions Arch - RedShift"
description: ""
category: posts
tags: [aws, solutionsarch]
---
{% include JB/setup %}

Redshift is a data warehouse product that costs about $1000 per terabyte per year which is about 1/10 the cost of most data warehousing solutions. Instead of storing data in a row, data is stored in columns which makes it ideal for aggregating data. Data is stored squentially and compressed and along with Massive Parllel Processing make it really peppy. Only in 1 AZ; no HA.

Data is encrypted in transit and at rest and redshift manage keys. Key Management through HSM or KMS is possible.

## Configurations

Small configuration is a single node and up to 160Gb.

Multi-Node - includes a leader node and up to 128 compute nodes.

## Pricing 

Price = Compute Node Hours + backup costs + data transfer (within the VPC) 

## Resources
[Working with Redshift](https://qwiklabs.com/focuses/2866)

[Intro to Redshift](https://qwiklabs.com/focuses/2366)