---
layout: post
title: "AWS - Code[Commit, Build, Pipeline, Deploy]"
description: ""
category: posts
tags: [codedeploy, codecommit, codebuild, codepipeline, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

# CodeDeploy
The advantage of CodeDeploy is the integration with all AWS compute types and load balancers/AGW using numerous deployment options.

## Deployment options
- EC2 - Rolling deployment called "in-place"

- ASG - "in-place"  or Blue/Green using ALB

- Lambda - creates alias for function, tests using Pre-traffic hook, starts shifting traffic. Optionally CloudWatch, using an alarm, can trigger rollback and Post-traffic hooks can be run.  SAM uses CodeDeploy by default

- ECS - support for blue/green deployments &amp; canary deployments

# Code Commit
Hosted git with integrations into IAM, EventBridge, and Lambda. 

# Code Pipeline
Jenkins sort of thing except on AWS. Easy to 
