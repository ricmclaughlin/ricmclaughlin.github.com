---
layout: post
title: "AWS - High Availability"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Demonstrate ability to architect the appropriate level of availability based on stakeholder requirements
  
  RDS, S3, EBS 

# Demonstrate ability to implement DR for systems based on RPO and RTO
  
  RPO
  RTO

## backup and restore
    
    RDS - auto backup; no backup of read-replicas
    Elasticache - redis only
    Redshift
    EBS - NOT automated
    glacier
    Direct Connect
    storage gateway - 4 types of gateways
    Snowball
    Import/Export

## Pilot light
    
    Route53
    ASG/Launch Configuration
    RDS - 

## Warm Standby
    
    Route53 - Weighted
    Multi-AZ - syncronous

## Multi-Site
    
    Cross region Replication
      DynamoDB
      RDS cross region read Replicas - async; No Oracle or MS
      Redshift Snapshot

# Determine appropriate use of multi-Availability Zones vs. multi-Region architectures

# Demonstrate ability to implement self-healing capabilities
  
  ASG, RDS, EC2

# Key Resources

- [Using AWS for Disaster Recovery](http://d36cz9buwru1tt.cloudfront.net/AWS_Disaster_Recovery.pdf)