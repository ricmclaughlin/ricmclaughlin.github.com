---
layout: post
title: "AWS - EMR"
description: ""
category: posts
tags: [analytics, aws-solutions-arch-pro, aws-spec-ml]
---
{% include JB/setup %}

# EMR
EMR supports many different big data products including Spark, HBASE, Presto, Flink. Storage can use regular good old fashioned HDFS or EMRFS which is storage on S3. EMR runs in a VPC but within a single AZ.

## Components
EMR includes the following components:
- Master node - Manages data distribution to core and slave nodes; backs up log files to S3 every 5 minutes if configured upon creation; long running (so good candidates for RI)
- Core Nodes - store data on HDFS; managed by master node; long running (so good candidates for RI)
- Task Nodes - Performs data tasks; no HDFS; short running and good for Spot instances
- EMR Studio - managed Jupyter notebooks used as an integrated development environment; notebooks are backed up to S3 not on cluster with availability exclusively on the console

It's also common to run EMR in a transient cluster (temporary usage). Capacity is related to instances configuration; uniform instances groups have single instances types, purchasing options, and autoscaling while instances fleets mix instance types and don't have autoscaling (like Spot for EMR).

## Hadoop Background
The big idea is distribute high velocity, high volume and lots of variety of data across tons of servers... using [an Apache project, Hadoop](https://hadoop.apache.org/). Tasks include log analysis, web indexing, machine learning, financial analysis, scientific simulation, and bioinformatics. Hadoop is a data storage and batch processing framework that is compromised of four major components:
- Hadoop Common - the utilities and libraries part
- Hadoop HDFS - three copies of the data is written here... not a file system (can't mount it) = more of a data structure storage device
- Hadoop YARN - Yet Another Resource Negotiator; new in Hadoop 2; enables applications (Pig, Hive, HBase) to be plugged in
- Hadoop Map Reduce - the engine - an implementation of the MapReduce programming model for large scale data processing.

## Map Reduce
Parallel processing framework that works with Hadoop. It moves data out to processing location which essentially scales-out the architecture; fault tolerant; runs on commodity hardware; many different languages are supported; works with Hive in V2 and Pig (V1 & V2)

## Hive
Data warehouse based on Hadoop (Petabyte scale; distributed on commodity hardware; highly latent queries) Hive uses HQL so you can leverage your SQL skills and works on structured and unstructured data and has the nifty feature of defining a schema on read. Hive can be configured to read data from DynamoDB.

Components of Hive include:
- Databases - named bunch of data
- Tables - files in subdirectories; can be managed internally in HDFS or external of Hive.
- Partitions - a directory used to reduce the size of a table which makes data reads/filters faster
- Buckets - a file in a directory table used to manage tables; more efficient that 1000s of small partitions
- Views &amp; indexes - same as RDBmS
- Hive Metastore - stores information about partitions, schemas, tables & locations, and columns/column types

## Pig
Pig is a language for analyzing large datasets and is more efficient than Java and similar to SQL. Pig Latin is easily controlled, data agnostic, flexible, and fast. Use Pig when external services are needed for ETL, and you need to debug it... and you don't like Java. It has all the basic data types, complex data types and relations.

## Other Hadoop Projects
* [HBase](https://hbase.apache.org/) - an open source, non-relational, distributed database modeled after Google's BigTable and is written in Java. It is developed as part of Apache Software Foundation's Apache Hadoop project and runs on top of HDFS (Hadoop Distributed File System), providing BigTable-like capabilities for Hadoop. 
* [Cassandra](https://cassandra.apache.org/_/index.html) - a database management system designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure. 
* [Spark](https://spark.apache.org/) is super quick because it's an in-memory solution and can be used with the Spark MLLib, the defacto machine learning library for this platform. MLLib enables pretty much all ML functions including modeling, ETL/feature transformations, model evaluation, and ML persistence.

## Use Cases
* Get Data in realtime? Kinesis
* Import Data? Snowball or Import export
* Visualize Data? Quicksight
* On Demand Analytics = S3 => EMR (sort, aggregate, join) => Redshift (data storage) => Quicksight
* Realtime Analytics = Kinesis => DynamoDB
* Data Warehousing = S3 => EMR (ETL) => S3 => Redshift => Quicksight
* Clickstream = Kinesis => Hadoop/Kinesis Client Library -> Personalized recommendation
* Long batch processing jobs (log parsing, ETL)? Hive
* Real-time analytics? Redshift
* Spot instances long running/data-critical? master, core using on-demand/resource; task using spot
* Spot instances cost sensitive/app testing? spot

