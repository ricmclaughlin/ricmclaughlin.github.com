---
layout: post
title: "AWS - Migration"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Plan and execute for applications migrations

- [vmware vCenter plugin](/posts/aws-vmware) - for migration to EC2 and extend reach of vCenter to new geos without procurement

- [Storage Gateway - Gateway-stored](/posts/aws-storage-gateway) - sits on vm system then takes snapshots stored to migrate VMs to EC2 with consistent snapshots

- [Data Pipeline](/posts/aws-data-pipeline) - provision & terminate; on-prem; scheduled; components: data node, activity, precondition, schedule

- [Server Migration Service](https://aws.amazon.com/server-migration-service/) - takes in a vm ware instance and output an AMI; manages sync process

- [Cloud Data Migration](https://aws.amazon.com/cloud-data-migration/) - unmanaged tools including rsync, S3/Glacier CLI

- [Import/Export Disk](https://aws.amazon.com/snowball/disk/) - from physical storage shipped to AWS to an encrupted form on S3 bucket, Glacier, or EBS Snapshot; export to s3 encrypted only

- Database Migration Service - enables migration from lots of different databases to AWS databases and *from* AWS to on-prem

# Demonstrate ability to design hybrid cloud architectures

- [VPN](/posts/aws-advanced-networking) - transition from VPN to DX = raise BGP cost for VPN

- [Direct Connect](/posts/aws-direct-connect) - private vif = private addr; public vif = public services

- [VPC](/posts/aws-vpc) - vpc sizes; reserved CIDR; peering not transitive

### STS Use Cases

- SSO to console; no SAML? Broker with `STS:AssumeRole`; requires IAM user; very similiar to acct -> acct setup

- SS0 to API; no SAML? Broker with `GetFederationToken`; requires IAM user

- SSO to AD or other SAML? no broker; `AssumeRoleWithSAML`

- WIF? auth with IdP; `AssumeRoleWithWebIdentity`