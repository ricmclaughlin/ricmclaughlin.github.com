---
layout: post
title: "AWS - Migration"
description: ""
category: posts
tags: [aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Guidance
- [Migration Hub](https://docs.aws.amazon.com/migrationhub/latest/ug/whatishub.html) - AWS Migration Hub provides a single place to discover existing servers, plan migrations, and track the status of each application migration using data from SMS, DMS, and the Application Migration service. 

- [Application Discovery Services (ADS)](https://docs.aws.amazon.com/application-discovery/) - gathers information about on-premises data centers creating dependency maps and server utilization data. Two types of discovery: 1/ _Application Discovery Agentless Connector_ which is a VMware host that completes a VM inventory with CPU, memory and disk usage regardless of OS. 2/ 
_Application Discovery Agent_ figures out server configuration, system performance, running processes and details of network configurations between systems on Microsoft Server, and most Linux distros. Results can be exported as CSV, viewed in the Migration Hub, or analyzed by Athena. Athena has pre-defined queries, and the ability to create custom queries AND use Configuration Management Database exports.

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
Many of the compute migration services do run on-prem: DMS, Server Migration Service, Application Discovery Service, VM Import/export. Sort of obvious but you can download Amazon Linux 2 as an ISO to use on-prem.

- [Application Migration Service (MGN)](https://docs.aws.amazon.com/mgn/latest/ug/what-is-application-migration-service.html) - does *lift and shift* migrations for VMware vSphere, Microsoft Hyper-V, and EC2 services to run natively on AWS. MGN used to be CloudEndure and replaces S[Server Migration Service](https://docs.aws.amazon.com/server-migration-service/latest/userguide/server-migration.html); an on-prem application called _AWS Replication Agent_ collects data and configures a test VM in a staging environment enabling testing. If it works, launch it!

- [VM Import/export](https://docs.aws.amazon.com/vm-import/latest/userguide/how-vm-import-export-works.html) - moves VM-based server images to EC2 instances via a bucket - a manual tool which results in downtime

- [App2Container](https://docs.aws.amazon.com/app2container/latest/UserGuide/what-is-a2c.html) - enables replatforming of .NET Windows/Linux and Java Linux based application into OCI containers that can be run by ECS, EKS or App Runner. Does inventory, lists dependencies, extracts artifacts and creates Dockerfile, builds container, generates CFN (EKS/App Runner, Task definition for ECS, ECR Image) and deploys it, and optionally creates a CodePipeline. It does inventory and containerization remotely or on the app server.

# Databases
- [Database Migration Service (DMS)](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html) - enables migration to and from lots of different databases while running on an EC2 instance.

*From*: all RDS engine databases and MongoDB/DocumentDB both in AWS and on-prem, SAP, DB2, and S3

*To*: RDS engines running on an EC2, in RDS, and on-prem, DynamoDB, S3, OpenSearch, Kinesis, and DocumentDB

DMS runs well over VPC Peering, VPN, Direct Connect. Supports Full Load, Full Load + Change Data Capture (CDC), or CDC only. For Oracle, DMS supports Transparent Data Encryption (TDE) using AWS DMS [_BinaryReader_](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html#CHAP_Source.Oracle.CDC) and can output blobs provided there is a primary key on the table. For OpenSearch, this is not a supported source but can be used for a relational to search migration. DMS can't be used to replicate OpenSearch data.... because OpenSearch isn't supported as a source.

- Schema Conversion Tool - enables schema translation for OLTP (SQL Server/Oracle to MySQL/ PostgreSQL) OR OLAP (Teradata/Oracle to Redshift); technically part of DMS

# Triage 
- Heterogeneous migration? SCT
- Homogeneous migration? DMS
- Replicate data after migration? DMS
- Greater than 10 TB? Use SCT to extract the data and put it the SnowBall, AWS loads the data to a bucket, use SCT to insert data, use DMS to apply CDC updates to target data store.
- Migration TCO? App Discovery Service -> S3 -> Athena OR App Discovery Service -> Migration Evaluator
- Figure out what apps should be migrated? ADS
- Inventory applications? ADS
- Discover dependencies? ADS
- Inventory &amp; report on database and application migration? Migration Hub
- Business case for migration? Migration Evaluator
