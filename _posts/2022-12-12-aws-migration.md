---
layout: post
title: "AWS - Migration"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Guidance

- [Migration Hub](https://docs.aws.amazon.com/migrationhub/latest/ug/whatishub.html) - AWS Migration Hub provides a single place to discover  existing servers, plan migrations, and track the status of each application migration using data from DMS and the Application Migration service. 

- [Application Discovery Services (ADS)](https://docs.aws.amazon.com/application-discovery/) - gathers information about on-premises data centers creating dependency maps and server utilization data. Two types of discovery: 1/ Agentless Connector which is a VMware host that completes a VM inventory with CPU, memory and disk usage regardless of OS. 2/ 
Agent-based discovery figures out server configuration, system performance, running processes and details of network configurations between systems on Microsoft Server, and most linux distros. Results can be exported as CSV, viewed in the Migration Hub, or analyzed by Athena. Athena has pre-defined queries, and the ability to create custom queries AND use Configuration Management Database exports.

- AWS Cloud Adoption Readiness Tool
Tool that helps orgs develop plans for cloud adoption and migration across business, people, process, platform, operations and security perspectives.

- [AWS Migration Evaluator](https://aws.amazon.com/migration-evaluator/) - helps build data-driven business case for migration; has it's own agent-less data collector OR can import data from Application Discovery Service. Delivers a report of projected cloud costs and technical data about licensing and dependencies.

## Approach 6R
- rehosting - lift and shift
- replatforming - lift, tinker, and shift
- repurchasing - drop and shop
- refactoring/rearchitecting - change in move
- retire - drop it
- retain - revisit

# Storage

- [Storage Gateway](/posts/aws-storage-gateway) - many different configurations (S3 File Gateway, FSx for Windows File Gateway, Volume Gateway, Tape Gateway) can play a part in a migration.

- [Snow[cone, ball, mobile]](/posts/aws-snow-family) - great for small, medium, and giant data transfer via a hardened enclosure or semi trailer.

- DataSync

# Compute
Many of the compute migration services do run on-prem: DMS, Server Migration Server, Application Discovery Service, VM Import/export. 

- Application Migration Service (MGN) - used to be CloudEndure and replaces Server Migration Service; does lift and shift migrations for physical, virtualized, and cloud-based services to run natively on AWS. An on-prem application called AWS Replication Agent continously replicates 

- [vmware vCenter plugin](/posts/aws-vmware) - for migration to EC2 and extend reach of vCenter to new geos without procurement

- [VM Import/export](https://docs.aws.amazon.com/vm-import/latest/userguide/how-vm-import-export-works.html) - moves VM based server images to EC2 instances; looks like this isn't being actively developed

# Databases

- [Database Migration Service (DMS)](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html) - enables migration to and from lots of different databases while running on an EC2 instance.

*From*: all RDS engine databases and MongoDB/DocumentDB both in AWS and on-prem, SAP, DB2, and S3

*To*: RDS engines running on an EC2, in RDS, and on-prem, DynamoDB, S3, OpenSearch, Kinesis, and DocumentDB

DMS runs well over VPC Peering, VPN, Direct Connect. Supports Full Load, Full Load + Change Data Capture (CDC), or CDC only. For Oracle, DMS supports Transparent Data Encryption (TDE) using AWS DMS [_BinaryReader_](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html#CHAP_Source.Oracle.CDC) and can output blobs provided there is a primary key on the table. For OpenSearch, this is not a supported source but can be used for a relational to search migration. DMS can't be used to replicate OpenSearch data.... because OpenSearch isn't supported as a source.

Snowball can be used as a transport from on-prem. Use SCT to extract the data and put it the SnowBall, AWS loads the data to a bucket, DMS applies CDC updates to target data store.

- Schema Conversion Tool - enables schema translation for OLTP (SQL Server/Oracle to MySQL/ PostgreSQL) OR OLAP (Teradata/Oracle to Redshift)


