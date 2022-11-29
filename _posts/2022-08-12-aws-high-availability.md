---
layout: post
title: "AWS - High Availability"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Demonstrate ability to implement DR for systems based on RPO and RTO
  
- Recovery point objective (RPO) - amount of data, based on time, the business can loose; the shorter then more expensive 

- Recovery time objective (RTO) - The time it takes after a disruption to restore a business process to its service level including time to restore data

AWS is great for DR because it is flexible, Opex model, automation is easy.

Four scenarios: Backup and restore, pilot light, warm standby, and multi-site

## backup and restore

- [RDS](/posts/aws-rds) - auto backup with max 35 days retention; no backup of read-replicas; On-premise RDS replication

- [Elasticache](/posts/aws-elasticache) - redis can be backed up (very similiar to RDS); not memecached

- [Redshift](/posts/aws-redshift) - snapshots with continuous S3 backup for nodes; snapshot cross-region copy feature; restore = new cluster complete with configuration

- [EBS](/posts/aws-elastic-block-storage) - NOT automatic; encrypted volumnes = encrypted snapshot

- [S3/Glacier](/posts/aws-s3) - S3 is a great target for backup; Glacier has a long RTO metric

- [Storage Gateway](/posts/aws-storage-gateway)

- [Snowball &amp; Import/Export Snowball](/posts/aws-snowball)

## Pilot light
    
- [Route53](/posts/route53) - health checks

- [ASG](/posts/aws-autoscaling) - Autoscaling min/max adjustment & stored launch Configuration

- [EC2](/posts/aws-ec2) - AMI as a backup of system configurations

- [RDS](/posts/rds) - replication between AZ or from on-prem

## Warm Standby

- [Route53](/posts/route53) - Route53 - Weighted

- [RDS](/posts/rds) - Multi-AZ - synchronous

## Multi-Site

- [Route53](/posts/route53) - latency based routing with health checks; 
    
- [DynamoDB](/posts/aws-dynamodb) - Cross region Replication

- [RDS](/posts/rds) - cross region read Replicas - async; No Oracle or MS; encryption, options sets, and parameter sets challenging

- [Redshift](/posts/redshift) - automated cross region snapshot copy

# Determine appropriate use of multi-Availability Zones vs. multi-Region architectures

nothing really...

# Demonstrate ability to implement self-healing capabilities

- [RDS](/posts/rds) - Multi-AZ

- [ASG](/posts/aws-autoscaling) - Autoscaling min/max adjustment & stored launch Configuration

# Key Resources

- [Disaster Recovery in the Cloud](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html)