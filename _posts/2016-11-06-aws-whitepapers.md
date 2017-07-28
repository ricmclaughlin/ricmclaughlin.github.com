---
layout: post
title: "AWS Solutions Arch - Whitepapers"
description: ""
category: posts
tags: [aws, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# [Strategies for Managing Access to AWS Resources in AWS Marketplace](https://d0.awsstatic.com/whitepapers/strategies-for-managing-access-to-aws-resources-in-aws-marketplace.pdf)

EC2 Instance Roles - this is nothing more than creating an instance profile "container" for a role so it can be applied to an instance.

Access Resources in different account on behalf of users - IAM role that trusts the other account root using `sts:ExternalId` condition; and provides access to resource; use `sts:AssumeRole` in other account

Account Switching Roles - create sts parameters using `AWS.STS()`, then `AWS.TemporaryCredentials`, then use temp credentials to access service

# [Encrypting Data at Rest](http://d0.awsstatic.com/whitepapers/AWS_Securing_Data_at_Rest_with_Encryption.pdf)

Key Management Infrastructure is comprised of a key storage and key management function wich work together to secure data at rest. THere are three models to how this works:

* Customer controls encryption method and KMI = Client side encryption and HSM

* Customer controls encryption method, KMI storage while AWS controls KMI management = HSM

* AWS controls encryption method, KMI Storage and management functions = KMS

# [Migrating AWS Resources to a New AWS Region](http://d0.awsstatic.com/whitepapers/aws-migrate-resources-to-new-region.pdf)

IAM, CloudWatch and the management console are operate outside of regions so don't require a move.

Lots of resources are region specific and won't move, including:

- Key Pairs

- SG (actually VPC Specific)

- AMI (copying to a new region does NOT copy tags or permissions)

- EIP (must change address with DNS because EIP won't move)

- Reserved Instances 

Many resources must be re-established:

- networking stuff like VPC, ELB, ASG, Direct connect

- Cloudfront

Many resources must be copied and re-established

- S3/Glacier = new name for bucket

- RDS (using dumps), DynamoDB (using streams, table copy or import export)

- EMR, ElasticSearch, Redshift, ElasticCache = dump and load sort of stuff

# [AWS Well-Architected Framework](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)

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


# [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/aws-security-whitepaper.pdf)

This whitepaper is mostly review yet there are some bigger points:

- Amazon's corporate network is separate from the AWS network; 

- instances are isolated and memory is scrubbed prior to being reused by the hypervisor; a firewall sits between the physical layer and the security group

AWS protects you from:

- DDoS attacks 

- Man-in-the-Middle-Attacks

- IP Spoofing

- Port Scanning

- Packet Sniffing

# [Security at Scale: Governance in AWS](https://d0.awsstatic.com/whitepapers/compliance/AWS_Security_at_Scale_Governance_in_AWS_Whitepaper.pdf)

There are tons of ways that AWS enhances and improves on-prem management by managing IT resources, security and performance. Overall, there is little here new...


## Not very useful whitepapers

* [Amazon Web Services: Risk and Compliance](https://d0.awsstatic.com/whitepapers/compliance/AWS_Risk_and_Compliance_Whitepaper.pdf) - this one is amalogomation of the [Overview of Security Processes](https://d0.awsstatic.com/whitepapers/aws-security-whitepaper.pdf) and [Well-Architected Framework](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)

* [Architecting for the Cloud](https://d0.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf) - nothing too new here...



