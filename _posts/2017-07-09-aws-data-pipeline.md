---
layout: post
title: "AWS - Data Pipeline"
description: ""
category: posts
tags: [aws, data-pipeline]
---
{% include JB/setup %}

AWS [Data Pipeline](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/what-is-datapipeline.html) is a web service that you can use to automate the movement and transformation of data. With AWS Data Pipeline, you can define data-driven workflows, so that tasks can be dependent on the successful completion of previous tasks. In essence, the Data Pipeline is an ETL tool with orchestration... sort and filled much of the functions that Lambda and [Glue](https://aws.amazon.com/glue/) now fills. 

A Pipeline includes:

* DataNode - The location and type of data the pipeline uses as input and output that include `DynamoDBDataNodes`, `MySqlDataNodes`, `RedshiftDataNodes` and `S3DataNodes`

* Activity - what is supposed to be done by the pipeline; some are pre-built (called a template) or can be custom built; includes stuff like `CopyActivity`, `SqlActivity` or Hadoop-centric stuff like `PigActivity` and `HiveActivity`

* Precondition - an assertion that must be true for an activity to run; includes things like `DynamoDBDataExists` or `S3KeyExists` or `S3PrefixNotEmpty` or custom things like `ShellCommandPrecondition` or `Exists`

* Schedule - defines when the activity runs; might include end date

## Data Pipeline Application

There are several components to the Data Pipeline application: 

* TaskRunners - an app that polls the pipeline then executes tasks; pre-built or custom built; can run on EC2 or an _on-prem server_; essentially and agent that receives a task, reports progress and reports completion or failure.

* Resources - the resources that does the work like an `Ec2Resource` or `EmrCluster`

* Actions - Oddly named - perhaps Results would be a better term for the concept - When certain event occur, like failure, an action can either can set an alarm using `SnsAlarm` or `Terminate`

Because the use of EC2 resources is common, using reserve instances, spot instances, in addition to on-demand resources is possible. And spot instances come with the associated problems of task interuption and task switching - the pipeline might need to be re-run.

## Use Cases

- Export Data from DynamoDB for cross region replication

- Import Data into Redshift - run SQL on data to extract to S3, transform and clean using an EMR hive job, outputing it to S3, then insert it into Redshift

- Backup or Dump data from an on-prem server to the cloud

## Troubleshooting

* Errors in pipeline? error info in console or look at output log

* EMR errors? Locate cluster in console; troubleshoot

* Pipeline stuck in `Pending`? Probably a problem in the pipeline definition

* Component stuck in `WAITING_FOR_RUNNER`? Missing `runsOn` or `workerGroup` field

* Component run in wrong order? Data Pipeline is as async as possible; use `dependsOn`

* `400 Error - PipelineNotFoundException`? Wrong permissions