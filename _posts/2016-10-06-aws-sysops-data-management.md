---
layout: post
title: "AWS SysOps - Data Management"
description: ""
category: posts
tags: [aws, sysops, soluarch]
---
{% include JB/setup %}

# Services with Automatic Backup

## ElastiCache
ElastiCache only backs up Redis clusters. Snapshots backup the data for the entire cluster at a specific time and probably cause a performance degradation. Try to backup read replicas!

## Redshift
Free backup storage up to the size of the cluster! Backup can be automatic or manual and is incremental. Only 1 day retention period by default.

## RDS
Yes, by default RDS supports automated backup... checkout this post for [more info on RDS](/posts/aws-solutions-arch-rds). Need a transactional engine; 

# Services WITHOUT Automated Backup

EC2 does not have automated backup... snapshots of EBS volumes are incremental but when restored snapshots are complete. A python or bash script is required.
