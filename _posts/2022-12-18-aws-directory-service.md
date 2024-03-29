---
layout: post
title: "AWS - Directory Service"
description: ""
category: posts
tags: [management, aws-solutions-arch-pro]
---
{% include JB/setup %}

[AWS Directory Service](https://aws.amazon.com/directoryservice/) is a series of directory services included Microsoft Active Directory in the cloud. There are numerous flavors: [AD Connector](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_ad_connector.html), [Managed Microsoft Active Directory ~~(Enterprise Edition)~~](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html), [Cognito](http://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) &amp; Simple AD 

AD Connector and Managed Microsoft Active Directory ~~(Enterprise Edition)~~ support standard Active Directory administration tools and take advantage of built-in Active Directory features such as Group Policy, trusts, and single sign-on.

## *Managed* Microsoft Active Directory ~~(Enterprise Edition)~~
_Managed Microsoft Active Directory_, which used to be called the "Enterprise Edition" is the same feature-rich Microsoft Active Directory just hosted on AWS. This is a managed service like RDS but better; it's a pair of controllers so it's 2 AZ, AND it is patched, rotated, and snap shotted by default. All you need to do is manage the implementation of it for the org and it's a good choice for more than 5,000 users.

To use on-prem and cloud directory information interchangeably, you can set up a trust relationship set up between an AWS hosted directory and your on-premises directories:
- _AD two-way Forest trust_ - which is like two parts of the same directory. 
- _AD one-way Forest trust_ has two scenarios - AWS trust on-prem OR on-prem trust AWS; 

## AD Connector 
_AD Connector_ connects existing AD implementation with the AWS cloud which enables existing security polices like password expiration/history and account lockout with existing RADIUS-based MFA to manage resources like EC2 instances and applications like WorkSpaces, WorkDocs or WorkMail. AD Connectors and on-premises domains have a 1-to-1 relationship. It does not support caching, trust relationships, SQL server integration - it's a proxy. 

## Simple AD
_Simple AD_ is a Microsoft Active Directory–compatible service that includes [Samba 4](https://www.samba.org/) and LDAP support for use cases of less than 5000 users. This service does NOT support MFA or additional AD servers, support trust relationships with AD, or transfer FSMO roles for AD management. There are two sizes: small for up to 500 users and large for up to 5000 users.

## Cloud Directory
Amazon Cloud Directory is a great choice when you need to build application directories such as device registries, catalogs, social networks, organization structures, and network topologies. It's not for IT administrators that want to manage their directory infrastructure.

## Cognito
You can also use Amazon Cognito when you need to create custom registration fields and store that metadata in your user directory. This fully managed service scales to support hundreds of millions of users while fully supporting federated identities. 

## Triage
- Scalable? Cloud Directory
- Need something AD compatible? Managed Microsoft AD, AD Connector, Simple AD
- Greater than 5000 users? Managed Microsoft AD
- Have AD and wanna use it on AWS? AD Connector
- AD Disaster Recovery? replicate the directory to an EC2 instances then set up two-way Forest Trust with Managed Microsoft AD.
- LDAP? Simple AD