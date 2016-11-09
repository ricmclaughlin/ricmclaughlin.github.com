---
layout: post
title: "AWS Solutions Arch - EMR"
description: ""
category: posts
tags: [aws, soluarch]
---
{% include JB/setup %}

The big idea is distribute high volocity, high volume and lots of variety of data across tons of servers... using Hadoop. Tasks include Log analysis, web indexing, machine learning, financial analysis, scientific simulation, and bioinformatics. Hadoop is a data storage and batch processing framework that is comprized of four major components:

- Hadoop Common - the utilities and libraries part

- Hadoop HDFS - three copies of the data is written here... not a file system (can't mount it) = more of a data structure storage device

- Hadoop YARN - Yet Another Resource Negoiator; new in Hadoop 2; enables applications (Pig, Hive, HBase) to be plugged in

- Hadoop Map Reduce - the engine


## HDFS Version 2

HDFS includes the following components:

- Name Node - copies data to passive name node; not a single point of failure; manage container lifecycle

- Passive Name Node - backup name node; passive as the names suggests

- Secondary Name Node - store another copy of the name node data for redundancy.

- Data Nodes - where the data lives

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

## Resources
[All about Hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop)