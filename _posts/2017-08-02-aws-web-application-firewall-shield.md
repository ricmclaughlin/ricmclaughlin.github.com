---
layout: post
title: "AWS - Web Application Firewall - Shield"
description: ""
category: 
tags: []
---
{% include JB/setup %}

L7 - application attack - floods; slowloris; DNS query

L6 - SSL Abuse

L4 - SYN floods

L3 - UDP - mplification/Reflection attacks - attacker sends in a small packet to an unrelated server that needs a 28 to 54 times greater response then the unrelated server using a spoofed address; unrelated server send response to target of attack.

Mitigation is all about minimizing the attack surface, absorbing the attack, safeguarding the resources, learning normal behavior and having a plan for attacks.

- Minimize attack surface - Bastion box, limit access to resources to known IP ranges, WAF Sandwich (WAF between an external and internal ELB)

- Absorbing the attack - scale horizontally or vertically which spread the attack over a larger area (and makes the attackers scale), gives you time to analyze the attack

- Safeguarding the resources - Using Cloudfront by adding geo restrictions/blocking or Origin Access Identity (S3 access through CloudFront URLs only) Route53 by adding an alias to different distribution or ELB and using private DNS, WAF service or a WAF market place solution and Shield.

- Learn Normal behavior - and set alarms for abnormal traffic patterns.

Intrusion Detection System (IDS) - inspects all inbound and outbound network activity and IDs suspicious patterns. 

Intrusion Protection System (IPS) - protect the stuff from intrusion

Generally IDS/IPS systems site in a public subnet and write data from recieved from an EC2 agent to a log the forward the log to a SOC or sometime in S3.
