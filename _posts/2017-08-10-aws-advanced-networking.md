---
layout: post
title: "AWS - Advanced Networking"
description: ""
category: 
tags: []
---
{% include JB/setup %}


## VPC Multicast Support

By default VPC does not support IP multicast yet numerous legacy application require it. To support this functionality, a [virtual overlay network](https://aws.amazon.com/articles/6234671078671125) can be created at the OS level of the instances. 

This OS level network includes a tunnel and virtual networking that must have a different CIDR range that is independent of the VPC. The tunnel are typicall of the GRE or L2TP types and can be made with OpenVPN or Ntop's N2N.

