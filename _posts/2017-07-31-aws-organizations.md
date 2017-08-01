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

Resource Groups
Tag editor

http://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/tag-editor.html