---
layout: post
title: "AWS - Config"
description: ""
category: posts
tags: [aws, mgt-governance, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

[Config](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) continually assesses, audits, and evaluates the configurations and relationships of resources. Config tracks configuration and changes over time; it does NOT deny changes from happening. It's about tracking resource drift. 

If CloudTrail is enabled (and it is by default), the management events records can be accessed from Config to figure out who/when about resource configurations change. Config is a regional service but all the data can be aggregated across regions and accounts. 

Bucket access, SSH access through security groups, OR ALB configuration changes are great use cases for Config; an ideal configuration of these settings is called a _Config Rule_. Rules can be auto remediated using SSM Automations. A _A conformance pack_ is a set of Config rules and remediation actions.

There are over 75 managed config rules; more rules can be added using Lambda functions. Rules can be run on configuration changes OR in a batch mode. EventBridge, S3, and SNS can receive notification if not compliant. 

An _Aggregator_ is a read-only collection (resource) of config and compliance data from one or more accounts and multiple regions OR an Organization available in the Aggregator *reporting only* dashboard. 

# Triage
- Report on configuration or compliance setting? create SCP, IAM policy, the report on progress by creating an Aggregator.
- approved AMI? use `approved-amis-by-id` managed rules