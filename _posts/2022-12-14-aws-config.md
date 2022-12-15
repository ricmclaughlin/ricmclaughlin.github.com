---
layout: post
title: "AWS - Config"
description: ""
category: posts
tags: [aws, config, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

[Config](https://aws.amazon.com/certificate-manager/) continually assesses, audits, and evaluates the configurations and relationships of resources. Config tracks configuration and changes over time; it does NOT deny changes from happening. It's about tracking resource drift. Config is a regional service but all the data can be aggregated across regions and accounts. 

Bucket access, SSH access through security groups, OR ALB configuration changes are great use cases for Config. If CloudTrail is enabled (and it is by default), the management events records can be accessed from Config to figure out who/when about resource configurations change.

There are over 75 managed config rules; more rules can be added using Lambda functions. Rules can be run on configuration changes OR in a batch mode. EventBridge can recieve rules if not compliant. Rules can be auto remediated through SSM Automations.
