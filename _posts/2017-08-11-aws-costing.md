---
layout: post
title: "AWS - Costing"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Demonstrate ability to make architectural decisions that minimize and optimize infrastructure cost

## Budgets

Budgets are an AI based approach to projecting monthly costs. By default, there is no budget setup and they don't show refunds. Budgets are updated every 24 hours and include SNS/CloudWatch alarms capability. 

# Apply the appropriate AWS account and billing set-up options based on scenario

## Consolidated Billing

Consolidated billing makes it very easy to track charges and allocate costs. Billing starts being consolidated the day the account signs up for consolidated billing. Consolidated billing does not give the payer access to the linked account. There is a soft limit of 20 consolidated accounts.

One big reason why to consolidate account is volume discounts. These apply!

In some cases excess EC2 Reserved Instances can rollover to consolidate accounts. Same for Amazon RDS DB instances. These only rollover provided the instance type, region and availability zones match.. so maybe very useful. However, AWS credits can be applied across accounts and that seems helpful.

The paying account is independent of the other accounts; the paying account cannot access the resources of the other accounts but billing alarms roll up the data from other accounts. Account limits, like the number of EC2 instances, only work on the account level and support is per account as well.To keep things simple, don't put resources in the billing account. 

## Resource Groups

Use the [Tag editor](http://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/tag-editor.html) to restriction actions to resources; add assigning/mod tags restrictions into a policy


## Cross Account Access

Checkout [STS use cases](/posts/aws-security)

# Ability to compare and contrast the cost implications of different architectures

- EC2 instance types

- EC2 Purchase options & restrictions

- RDS purchase options & restrictions




