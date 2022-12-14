---
layout: post
title: "AWS - Disaster Recovery"
description: ""
category: posts
tags: [disaster-recovery, aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Disaster recovery basics RPO and RTO
  
- Recovery point objective (RPO) - amount of data, based on time, the business can loose; the shorter then more expensive 

- Recovery time objective (RTO) - The time it takes after a disruption to restore a business process to its service level including time to restore data

AWS is great for DR because it is flexible, Opex model, automation is easy.

There are four main scenarios: Backup and restore, pilot light, warm standby, and multi-site

## Backup and Restore
Just backup the data and restore it later; Backup data; services idle.. RPO can be short cheaply; RTO ends up being really long

- [RDS](/posts/aws-rds) - auto backup with max 35 days retention; no backup of read-replicas; On-premise RDS replication

- [Elasticache](/posts/aws-elasticache) - redis can be backed up (very similiar to RDS); not memecached

- [Redshift](/posts/aws-redshift) - snapshots with continuous S3 backup for nodes; snapshot cross-region copy feature; restore = new cluster complete with configuration

- [EBS](/posts/aws-elastic-block-storage) - NOT automatic; encrypted volumnes = encrypted snapshot

- [S3/Glacier](/posts/aws-s3) - S3 is a great target for backup; Glacier has a long RTO metric

- [Storage Gateway](/posts/aws-storage-gateway) - continuous backup

- [Snowball &amp; Import/Export Snowball](/posts/aws-snowball) - this gets the initial chunk of data OR big chunks of data, to S3

## Pilot light
Live data; services idle 
- [Route53](/posts/route53) - health checks

- [ASG](/posts/aws-autoscaling) - Autoscaling min/max adjustment & stored launch Configuration

- [EC2](/posts/aws-ec2) - AMI as a backup of system configurations

- [RDS](/posts/rds) - replication between AZ or from on-prem

## Warm Standby
Live data; services small
- [Route53](/posts/route53) - Route53 - Weighted

- [RDS](/posts/rds) - Multi-AZ - synchronous

## Multi-Site
Live data; live services load balanced between sites 
- [Route53](/posts/route53) - latency based routing with health checks; 
    
- [DynamoDB](/posts/aws-dynamodb) - Cross region Replication

- [RDS](/posts/rds) - cross region read Replicas - async; No Oracle or MS; encryption, options sets, and parameter sets challenging

- [Redshift](/posts/redshift) - automated cross region snapshot copy

# Elastic Disaster Recovery (DRS)
Very much like Application Migration Server (MGN) except for DR - used to be called _CloudEndure Disaster Recovery_. DRS minimizes downtime and data loss with fast, reliable recovery of on-premises and cloud-based applications using affordable storage, minimal compute, and point-in-time recovery. Uses what looks like the same AWS Replication Agent as Application Migration Server to do block-level replication for servers; failover happens in minutes.

# Key Resources

- [Disaster Recovery in the Cloud](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html)