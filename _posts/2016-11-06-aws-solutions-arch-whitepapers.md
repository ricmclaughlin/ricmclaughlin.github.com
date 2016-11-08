---
layout: post
title: "AWS Solutions Arch - Whitepapers"
description: ""
category: posts
tags: [aws, soluarch]
---
{% include JB/setup %}

# Well Architected Framework Overall
This whitepaper is pretty much the AWS Solutions Architect Bible... overall, it preaches:

* Stop guessing your capacity needs - make it scalable from the onset

* Test Systems at Scale - it is easy to make a QA or an exactly-the-same-as-production system to test, why not?

* Lower Risk of Architecture Change - deploying infrastructure as code if it screws up be prepared to roll back

* Automate to make architectural experimentation easier - if it screws up you CAN roll it back

* Allow for evolutionary architecture - you can iterate...

## 4 Pillars of a Well Architected Framework

### Security - DIPD

Data Protection - Least Privilege Access System; Encrypt data with rotating keys; versioning; region residency within region

Infrastucture Protection - AWS does this; VPC is your resonsibility

Privilege Management - ensures authorization and authentication; ACLs, Roles & password management

Detective Controls - logging with CloudTrail, CloudWatch, AWS Config

#### Security - Best Practices

- Focus on automating security best practices

- Focus on securing your system, apply security at all ayers, 

- Trace security events, Automate security events

### Reliablity - FFC

* Foundations - is the foundation of your setup good?

* Change Management - How do you manage change?

* Failure Management - How do you detect and respond to failures?

#### Reliablity - Best Practices

- Test Recover Procedures - Test recovery in QA, staging, and.... production. 

- Automatically Recover from Failure - when bad stuff happens your system should recover from it gracefully.

- Scale Horizonally - add more little units of computing power instead of growing small numbers of units of computer power

- Stop Guessing Capacity - make it elastic from the onset

### Performance Efficiency

* Compute - the right sized instance makes the task run most efficiently

* Storage - the right attributes of your storage needs dictate what storage service to use

* Database - relational/document; OLTP/OLAP

* Space-time trade-off - there is a trade off between time and space; be aware of it


#### Performance Efficiency - Best Practices
* Democratize Advanced Technologies - instead of developing expertise USE a service with embedded expertise

* Go Global in Minutes - when global needs arise address them

* Use Server-less Architectures - services are efficient with cost, performance and reliability built in

* Experiment more often - use the flexibility and low cost to experiment and drive efficiency

### Cost Optimization

- Matched Supply and Demand - expand AND contract infrastructure based on demand

- Cost Effective Resources - what is the least cost way of solving the problem that satisfies the requirements

- Expenditure Awareness - infrastructure costs money

- Optimizing Over Time - iterate on existing technology and embrace new technologies as they arise


#### Cost Optimization - Best Practices
* Transparently Attribute Costs - you business has cost centers for a reason

* Use Managed Services - services are efficient with cost, performance and reliability built in

* Trade Capital Expense for Operating Expense - why buy when you can rent?

* Benefit from Economies of Scale - Amazon is cheaper than doing it yourself

* Stop Spending Money on Data Center Operations - duh.

## Resources
[AWS Well-Architected Framework](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)

# Amazon Web Services: Overview of Security Processes



[Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/aws-security-whitepaper.pdf)