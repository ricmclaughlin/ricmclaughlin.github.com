---
layout: post
title: "AWS - Directory Service"
description: ""
category: posts
tags: [aws, iam, aws-solutions-arch-pro]
---
{% include JB/setup %}

[AWS Directory Service](https://aws.amazon.com/directoryservice/) is a series of directory services included Microsoft Active Directory in the cloud. There are numerous flavors: [AD Connector](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_ad_connector.html), [AWS Directory Service for Microsoft Active Directory (Enterprise Edition)](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html), [Cognito](http://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) &amp; Simple AD 

AD Connector and Microsoft Active Directory (Enterprise Edition) support standard Active Directory administration tools and take advantage of built-in Active Directory features such as Group Policy, trusts, and single sign-on.

## AD Connector 

AD Connector connects existing AD implementation with the AWS cloud which enables existing security polices like password expiration/history and account lockout with existing RADIUS-based MFA to manage resources like EC2 instances and applications like WorkSpaces, WorkDocs or WorkMail. AD Connectors and on-premises domains have a 1-to-1 relationship.  

## Simple AD

Simple AD is a Microsoft Active Directory–compatible directory from AWS Directory Service using [Samba 4](https://www.samba.org/) for use cases of less than 5000 users. This service does NOT support MFA or additional AD servers, support trust relationships with AD, or transfer FSMO roles for AD management. There are two sizes: small for up to 500 users and large for up to 5000 users.

## *Managed* Microsoft Active Directory ~~(Enterprise Edition)~~

Managed Microsoft Active Directory, which used to be called the "Enterprise Edition" is the same feature-rich Microsoft Active Directory just hosted on AWS. Microsoft AD is the best choice for more than 5,000 users and need a trust relationship set up between an AWS hosted directory and your on-premises directories. This is a managed service like RDS but better; it's multi-AZ, is patched, rotated and snapshotted by default. All you need to do is manage the implementation of it for the org. 

## Cloud Directory

Amazon Cloud Directory is a great choice when you need to build application directories such as device registries, catalogs, social networks, organization structures, and network topologies.

## Cognito User Pools

You can also use Amazon Cognito when you need to create custom registration fields and store that metadata in your user directory. This fully managed service scales to support hundreds of millions of users fully supports federated identities. 

## Triage

Scalable? Cloud Directory

Have AD and wanna use it on AWS? AD Connector

More than 10 directories and 5 snapshots per each directory? Call AWS for limit increase.

Need something AD compatible? Managed Microsoft AD, AD Connector, Simple AD

Simple LDAP? Cloud Directory