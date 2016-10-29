---
layout: post
title: "AWS Project - Basic Account Audit"
description: ""
category: projects
tags: [aws, how-to]
---
{% include JB/setup %}

Say you wanted to do a basic audit of a small AWS account. What sort of things would you assess? Here is a light-weight version of what I would look at. Not exhaustive. A two hour review.

trusted advisor

## Overall
Least priviledge

Encryption - HTTPS/TLS, Data-at-Rest
EC2 roles 
Port filtering
Keys and Key rotation

## User Permissions in IAM
Security Status

- MFA

- Password Spec

Policy simulator as Audit Evidence
Credential Report


## EC2
Security Groups
Roles

## VPC
ACLs

## Cloud Watch Data


## CloudTrail
Is cloud trail enabled for all Cloud 