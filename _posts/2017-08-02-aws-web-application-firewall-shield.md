---
layout: post
title: "AWS - Web Application Firewall - Shield"
description: ""
category: posts
tags: [aws, aws-dev-ops-pro, vpc, shield, waf, aws-services]
---
{% include JB/setup %}


## Web Application Firewall

Instead of just blocking attack sources, WAF protects again attack patterns based on the entire makeup of the request using filters which can allow, block or simply monitor the activity. WAF can be deployed against a CloudFront distribution OR an Application Load Balancer.

Filter rules are executed in order and will act on the first rule that is matched OR the default rule will be executed. You must specify a default action for requests that don't match filter rules. The default action can be a fail/close where all non-matching requests fail (a default deny sort of thing) OR you can have a default allow filter rule.

Of course, WAF is PCI compliant.

### Pricing

Simple pricing structure: 

$5 per web ACL per month
$1 per rule per month
0.60 per million requests

## Shield

Shield comes in two flavors. Standard, which is free and not of much help and and Advanced which will absorb most attacks and give you a free coverage for usage during the attack. 

