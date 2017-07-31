---
layout: post
title: "AWS SysOps - Cost Optimization"
description: ""
category: posts
tags: [aws, sysops, cost]
---
{% include JB/setup %}


## Consolidated Billing

Billing starts being consolidated the day the account signs up for consolidated billing. Consolidated billing does not give the payer access to the linked account. There is a soft limitof 20 consolidated accounts.

One big reason why to consolidate account is volume discounts. These apply!

In some cases excess EC2 Reserved Instances can rollover to consolidate accounts. Same for Amazon RDS DB instances. These only rollover provided the instance type, region and availability zones match.. so maybe very useful. However, AWS credits can be applied across accounts and that seems helpful.

The paying account is independent of the other accounts; the paying account cannot access the resources of the other accounts. 

## Cloud Watch

When the "Receive Billing Alerts" option is enabled in the billing preferences, and you access the North Virginia region, you can access billing data in CloudWatch. Alarms and metrics are nifty.

## Difference Cost Optimization Ideas

EC2 reserved instances - These intances allow you to prepay for capacity upfront on terms from 1 to 3 years; not an instance - a reservation to pay less for the instance.

In addition, looking for low utilization EC2 instances and unused load balancers can bring cost savings. Remove old EBS snapshots, removing excess provisioned IOPS and downsize volumes can help as well. Although a smaller cost overall, releasing unused Elastic IP can help as well. 

RDS is expensive so finding idle RDS instances, in other words ones with 0 connections, is another way to avoid wasting money.

## Price Explorer & Price List API

You have to enable the Price Explorer, then wait for 24 hours before things will work for you. 

The Price List API is a series of JSON documents that outline the amazingly huge task of billing for AWS services. Think a complicated list with a megaton of detail.

## Reserved Instances

EC2 offers several different reserved instance types.

* Standard Reserved Instances - 

* Scheduled Reserved Instances - 

RDS and ElastiCache offer reserved instances as well

### Reserved Instance Marketplace

If you can't use your reservations you CAN sell them.

