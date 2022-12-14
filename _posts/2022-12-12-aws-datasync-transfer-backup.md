---
layout: post
title: "AWS - DataSync, Transfer Family & AWS Backup"
description: ""
category: posts
tags: [datasync, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

# DataSync

AWS DataSync is a secure, online service that automates and accelerates moving data between on premises and AWS Storage services (including S3, EFS, and FSx) - and can also move data between AWS data storage as well.

Replication tasks are not continuous; they are scheduled. DataSync preserves file permissions and metadata on NFS, POSIX, and SMB file systems.

On-prem systems require an agent to access DataSync and can transfer up to 10 Gbps (configurable to lower if needed) with configurable TLS to encrypt data in flight. DataSync support VPC Endpoints/Private Link.

AWS services don't require an agent. The Snow family fully supports DataSync as well with a preinstalled agent.

# AWS Transfer Family
AWS Transfer Family securely scales recurring business-to-business file transfers to S3 OR EFS using SFTP, FTPS, FTP, and AS2 protocols. 

Full redundancy across multiple Availability Zones within an AWS Region. Access control in the service or integrate with existing auth system.

The public endpoint for AWS Transfer Family will change which means you can't set up IP allow lists. VPC endpoints with access from a corporate local works. To secure access from the internet using an allow list attach an elastic IP to the VPC endpoint and set up an appropriate security group to enable access.

# AWS Backup
Fully managed centralized management and automation of backups across all AWS services to S3. No custom scripts OR manual processes. Support cross region &amp; multiple accounts; only manages backups made with AWS Backup. Some services require opt-in, some support Point-in-time-recovery. On-demand or scheduled backup; tag based backup. 

- _Backup plans_ are name of the policies for AWS Backup and set up frequency, backup window, time to transfer to cold storage, and retention period.

- _Vault Lock_ enables Write Once Read Many (WORM) access that disables ANY modification, even by the root user.