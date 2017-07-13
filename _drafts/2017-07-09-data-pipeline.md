---
layout: post
title: "Data Pipeline"
description: ""
category: posts
tags: [aws, datapipeline]
---
{% include JB/setup %}

AWS [Data Pipeline](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/what-is-datapipeline.html) is a web service that you can use to automate the movement and transformation of data. With AWS Data Pipeline, you can define data-driven workflows, so that tasks can be dependent on the successful completion of previous tasks. In essence, the Data Pipeline is an ETL tool with orchestration.

There are several components to the Data Pipeline application: 

* TaskRunners - an app that executes tasks found when it polls the pipeline; pre-built or custom built; can run on EC2 or on-prem server

* DataNodes - The location and type of data the pipeline uses as input and output that include DynamoDBDataNodes, MySqlDataNodes, RedshiftDataNodes and S3DataNodes

* Activities - what is supposed to be done by the pipeline; some pre-built or can be custom built

* Precondition - an assertion that must be true to trigger an activity to include things like DynamoDBDataExists or S3KeyExists or S3PrefixNotEmpty or custom things like ShellCommandPrecondition

* Resources - the resources that does the work like an Ec2Resource or EmrCluster

Because EC2 resources are common, the use of reserve instances, spot instances and on-demand resources are very possible. And spot instances come with the associated problems of task interuption and task switching - the pipeline might need to be re-run. 

## Use Cases

- Export Data from DynamoDB for cross region replication

 - Import Data into Redshift - run SQL on data to extract to S3, transform and clean using an EMR hive job, outputing it to S3, then insert it into Redshift