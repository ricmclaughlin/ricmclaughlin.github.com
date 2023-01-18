---
layout: post
title: "AWS - Disaster Recovery"
description: ""
category: posts
tags: [disaster-recovery, aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Disaster recovery basics RPO and RTO
Why AWS? AWS is great for DR because it is flexible, Opex model, automation is easy.

- Recovery point objective (RPO) - amount of data, based on time, the business can loose; the shorter then more expensive 
- Recovery time objective (RTO) - The time it takes after a disruption to restore a business process to its service level including time to restore data

There are four main scenarios: Backup and restore, pilot light, warm standby, and multi-site; but how does this map to RPO and RTO?

## RPO/RTO in hours = Backup and restore 
Back up your data and applications into the recovery Region. 
- Using automated or continuous backups will enable point in time recovery, which can lower RPO to as low as 5 minutes in some cases. In the event of a disaster, you will deploy your infrastructure (using infrastructure as code to reduce RTO), deploy your code, and restore the backed-up data to recover from a disaster in the recovery Region.

### Implementation
Just backup the data and restore it later; Backup data; services idle.. 
- [RDS](/posts/aws-rds) - auto backup with max 35 days retention; no backup of read-replicas; On-premise RDS replication
- [Elasticache](/posts/aws-elasticache) - redis can be backed up (very similar to RDS); not memecached
- [Redshift](/posts/aws-redshift) - snapshots with continuous S3 backup for nodes; snapshot cross-region copy feature; restore = new cluster complete with configuration
- [EBS](/posts/aws-elastic-block-storage) - NOT automatic; encrypted volumes = encrypted snapshot
- [S3/Glacier](/posts/aws-s3) - S3 is a great target for backup; Glacier has a long RTO metric
- [Storage Gateway](/posts/aws-storage-gateway) - continuous backup
- [Snowball &amp; Import/Export Snowball](/posts/aws-snowball) - this gets the initial chunk of data, OR big chunks of data every once and a while, to S3

## RPO/RTO in tens of minutes = Pilot light
- Provision a copy of your core workload infrastructure in the recovery Region. 
- Replicate your data into the recovery Region and create backups of it there. 
- Resources required to support data replication and backup, such as databases and object storage, are always on. 
- Application servers or serverless compute are not deployed, but can be created when needed with the necessary configuration and application code.

### Pilot light Implementation
Live data; services idle 
- [Route53](/posts/route53) - health checks
- [ASG](/posts/aws-autoscaling) - Autoscaling min/max adjustment & stored launch Configuration
- [EC2](/posts/aws-ec2) - AMI as a backup of system configurations
- [RDS](/posts/rds) - Multi-AZ - synchronous
- Possible use of DRS

## RPO/RTO in minutes = Warm standby
Maintain a scaled-down but fully functional version of your workload always running in the recovery Region. When fully scales this is known as Hot Standby. The more scaled-up the Warm Standby is, the lower RTO and control plane reliance will be. 
- Data is replicated and live in the recovery Region. 
- Business-critical systems are fully duplicated and are always on, but with a scaled down fleet. 
- When the time comes for recovery, the system is scaled up quickly to handle the production load. 

### Warm Standby Implementation
- Live data; services small
- [Route53](/posts/route53) - Route53 - Weighted; healthchecks
- [RDS](/posts/rds) - cross region read Replicas - async; No Oracle or MS; encryption, options sets, and parameter sets challenging
- Possible use of DRS

## RPO/RTO near zero = Multi-Region 
(multi-site) active-active  
- Synchronize data across Regions. Possible conflicts caused by writes to the same record in two different regional replicas must be avoided or handled, which can be complex. Data replication is useful for data synchronization and will protect you against some types of disaster, but it will not protect you against data corruption or destruction unless your solution also includes options for point-in-time recovery.
- Your workload is deployed to, and actively serving traffic from, multiple AWS Regions.

## Multi-Site Implementation
Live data; live services load balanced between sites 
- [Route53](/posts/route53) - latency based routing with health checks; 
- [DynamoDB](/posts/aws-dynamodb) - Global Tables
- [Aurora](/posts/rds) - Global Database
- [Redshift](/posts/redshift) - automated cross region snapshot copy

# Elastic Disaster Recovery (DRS)
Very much like Application Migration Server (MGN) except for DR - used to be called _CloudEndure Disaster Recovery_. DRS minimizes downtime and data loss with fast, reliable recovery of on-prem and cloud-based applications using affordable storage, minimal compute, and point-in-time recovery. Uses what looks like the same "AWS Replication Agent" in Application Migration Server to do block-level replication for servers; failover happens in minutes.

DRS does not handle the failover from a networking perspective but enables failover and failback. 

# Triage
"Disaster resilient"? multi-region using S3 CRR

# Resources

[Reliability Pillar of the Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/plan-for-disaster-recovery-dr.html)
