---
layout: post
title: "AWS - Systems Manager"
description: ""
category: posts
tags: [aws, systems-manager, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

# Systems Manager (SSM)
Helps manages EC2 at scale; needs agent (and associated role) on EC2 instances; enabled to work on-prem too. 

## SSM `run` command
The ability to run commands remotely, with control over rate of execution, without SSH access at scale. A common use case is to send an command/trap an event with to EventBridge then trigger a SSM `run` command. Perhaps to send a command to a set of EC2 instances in the `terminating:wait" stage?

## Patching
SSM is also great for cross-platform patching using _path baselines_ which can be applied individually or using the tag key *Patch Group*/*PatchGroup* (applied using Tag Manager or Fleet Manager) in a maintenance window with the `AWS-RunPatchBaseline` command. During patching you can control the rate of execution based on concurrency or error threshold. The SSM Inventory feature enables patch compliance.

## Session Manager
SSM has a Session Manager feature that enables shell access to EC2 instances and on-prem servers without SSH. Supports Linux, macOS, &amp; Windows while logging connections and executed commands. As one might expect, EventBridge, SNS, S3, and  CloudWatch Logs can receive the log items. 

## OpsCenter
Aggregates information such as issues, events, and alerts across AWS resources such Config, CloudTrail, CloudWatch Alarms, and CloudFormation into OpsItems. OpsItems can trigger Automation Runbooks.

## Parameter Store
Part of AWS Systems Manager (SSM), parameter store is serverless storage service for configuration and secrets. Can be used for passwords, database connection strings, license codes, and API keys. Parameters are versioned, can be stored encrypted (using KMS) or plaintext and in hierarchies using a tree structures. Parameter store can also be used to pass variables between Cfn templates. Security through IAM; notifications through EventBridge.

Secrets Manager secrets can also be access via Parameter Store. SSM publishes AMI values that are read-only.

There are two tiers of parameters: Standard (10K parameters, 4kb) or Advanced (100K parameters, 8kb, Parameter Policies). Parameter Policies enable a TTL on sensitive data; when TTL expires an EventBridge event is fired which could enable a password change.

## Automation
Automation simplifies common maintenance, deployment, and remediation tasks for EC2, RDS, Redshift,  S3 and other services; Automation uses runbooks, a type of script. The automation action `AWS-CreateImage` can be used to create AMI in a runbook.