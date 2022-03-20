---
layout: post
title: "AWS - EMR"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

## EMR Overview

The big idea is distribute high volocity, high volume and lots of variety of data across tons of servers... using Hadoop. Tasks include Log analysis, web indexing, machine learning, financial analysis, scientific simulation, and bioinformatics. Hadoop is a data storage and batch processing framework that is comprized of four major components:

- Hadoop Common - the utilities and libraries part

- Hadoop HDFS - three copies of the data is written here... not a file system (can't mount it) = more of a data structure storage device

- Hadoop YARN - Yet Another Resource Negoiator; new in Hadoop 2; enables applications (Pig, Hive, HBase) to be plugged in

- Hadoop Map Reduce - the engine - an implementation of the MapReduce programming model for large scale data processing.


## EMR

EMR includes the following components:

- Master node - Manages data distribution to core and slave nodes; backs up log files to S3 every 5 minutes if configured upon creation

- Core Nodes - store data on HDFS; managed by master node

- Task Nodes - Performs data tasks; no HDFS

Storage can use regular good old fashioned HDFS or EMRFS which is storage on S3.

## Map Reduce

Parallel processing framework that works with Hadoop. It moves data out to processing location which essentially scales-out the architecture; fault tolerant; runs on commodity hardware; many different langauges are supported; works with Hive in V2 and Pig (V1 & V2)

## Hive

Data warehouse based on Hadoop (Petabyte scale; distributed on commodity hardware; highly latent queries) Hive uses HQL so you can leverage your SQL skills and works on structed and unstructured data and has the nifty feature of defining a schema on read.
Components of Hive include:

- Databases - named bunch of data

- Tables - files in subdirectories; can be managed internally in HDFS or external of Hive.

- Partitions - a directory used to reduce the size of a table which makes data reads/filters faster

- Buckets - a file in a directory table used to manage tables; more efficient that 1000s of small partitions

- Views & indexes - same as RDBDS

- Hive Metastore - stores information about partitions, schemas, tables & locations, and columns/column types

## Pig

Pig is a language for analyzing large datasets and is more efficient than Java and similiar to SQL. Pig Latin is easily controlled, data agnostic, flexible, and fast. Use Pig when external services are needed for ETL, and you need to debug it... and you don't like Java. It has all the basic data types, complex data types and relations.

## Other Hadoop Projects

* HBase - an open source, non-relational, distributed database modeled after Google's BigTable and is written in Java. It is developed as part of Apache Software Foundation's Apache Hadoop project and runs on top of HDFS (Hadoop Distributed File System), providing BigTable-like capabilities for Hadoop. 

* Phoenix - an open source, massively parallel, relational database engine supporting OLTP for Hadoop using Apache HBase as its backing store.

* Apache Cassandra - a database management system designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure. 

## Use Cases

* Get Data in realtime? Kinesis

* Import Data? Snowball or Import export

* Visualize Data? Quicksight

* On Demand Analytics = S3 => EMR (sort, aggregate, join) => Redshift (data storage) => Quicksight

* Realtime Analytics = Kinesis => DynamoDB

* Data Warehousing = S3 => EMR (ETL) => S3 => Redshift => Quicksight

* Clickstream = Kinesis => Hadoop/Kinesis Client Library -> Personalized recommendation

* Long batch processing jobs(log parsing, ETL)? Hive

* Real-time analytics? Redshift

* Spot instances? Can it handle termination in the middle of the job, is it critical, is cost important are big questions.

## Resources

[All about Hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop)