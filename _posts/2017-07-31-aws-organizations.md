---
layout: post
title: "AWS - Organizations"
description: ""
category: 
tags: [aws, iam, aws-solutions-arch-pro]
---
{% include JB/setup %}

AWS Organizations enable consolidation and management of numerous account into a single unit.

works in two different modes; enable all features or consolidated billing only

## Enable All Features

OU

Accounts

Policies - special service control policies


## Consolidated billing

one bill per AWS account
very easy to track charges and allocate costs
volume discounts apply for resources
Don't put resources in the billing account
billing alarms roll up the data from other accounts

## Consolidated Billing

Billing starts being consolidated the day the account signs up for consolidated billing. Consolidated billing does not give the payer access to the linked account. There is a soft limitof 20 consolidated accounts.

One big reason why to consolidate account is volume discounts. These apply!

In some cases excess EC2 Reserved Instances can rollover to consolidate accounts. Same for Amazon RDS DB instances. These only rollover provided the instance type, region and availability zones match.. so maybe very useful. However, AWS credits can be applied across accounts and that seems helpful.

The paying account is independent of the other accounts; the paying account cannot access the resources of the other accounts.



Resource Groups
Tag editor

http://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/tag-editor.html