---
layout: post
title: "AWS Solutions Arch - Whitepapers"
description: ""
category: 
tags: [aws, soluarch]
---
{% include JB/setup %}

# Well Architected Framework Overall
Stop guessing your capacity needs - make it scalable from the onset

Test Systems at Scale - it is easy to make a system to test, why not?

Lower Risk of Architecture Change - if it screws up be prepared to roll back

Automate to make architectural experimentation easier -if it screws up you CAN roll it back

Allow for evolutionary architecture - you can iterate...

## 4 Pillars of a Well Architected Framework

### Security - DIPD

Data Protection - Least Privilege Access System; Encrypt data with rotating keys; versioning; region residency within region

Infrastucture Protection - AWS does this; VPC is your resonsibility

Privilege Management - ensures authorization and authentication; ACLs, Roles & password management

Detective Controls - logging with CloudTrail, CloudWatch, AWS Config

#### Best Practices

- Focus on automating security best practices

- Focus on securing your system, apply security at all ayers, 

- Trace security events, Automate security events

### Reliablity

Foundations

Change Management

Failure Management

#### Best Practices

- Test Recover Procedures

- Automatically Recover from Failure

- Scale Horizonally

- Stop Guessing Capacity - make it elastic from the onset

### Performance Efficiency

### Cost Optimization



## Resources
[AWS Well-Architected Framework](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)