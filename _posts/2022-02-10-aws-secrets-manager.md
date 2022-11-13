---
layout: post
title: "AWS - Secrets Manager"
description: ""
category: posts
tags: [aws, secrets-manager, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

# What does Secrets Manager do?
AWS Secrets Manager helps you protect secrets needed to access your applications, services, and IT resources.

## Service Features
- Easily replicate secrets to multiple region
- Secure and audit secrets centrally - audit with CloudTrail
- Custom Lambda functions to extend Secrets Manager rotation to other secret types, such as API keys and OAuth tokens. 
- Generate random secrets

## When to use Secrets Manager instead of Parameter store
- Secrets manager can rotate Amazon RDS, Amazon Redshift, and Amazon DocumentDB  keys
- Secrets manager can fire a lambda to rotate keys for other services
- Fine grained access control (like restricting retrieval from an IP address range)
- Cross region capabilities


