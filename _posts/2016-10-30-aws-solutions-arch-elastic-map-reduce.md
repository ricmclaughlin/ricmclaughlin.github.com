---
layout: post
title: "AWS Solutions Arch - EMR"
description: ""
category: 
tags: [aws, soluarch]
---
{% include JB/setup %}

The big idea is distribute high volocity, high volume and lots of variety of data across tons of servers... generally using Hadoop. Tasks include Log analysis, web indexing, machine learning, financial analysis, scientific simulation, and bioinformatics. Hadoop is a data storage and batch processing framework that is comprized of four major components:

Hadoop Common - the utilities and libraries part
Hadoop HDFS - three copies of the data is written here... not a file system (can't mount it) = more of a data structure storage device
Hadoop YARN - Yet Another Resource Negoiator; new in Hadoop 2; enables applications (Pig, Hive, HBase) to be plugged in
Hadoop Map Reduce


## HDFS Version 2
Name Node - copies data to passive name node; not a single point of failure; manage container lifecycle
Passive Name Node
Secondary Name Node
Data Nodes

## Map Reduce
Parallel processing framework that works with Hadoop. moves data out to processing location which essentially scales-out the architecture; fault tolerant; commodity hardware; many different langauges are supported; works with Hive and Pig

## Hive
Data warehouse based on Hadoop (Petabyte scale; distributed on commodity hardware; highly latent queries)
Uses HQL so you can leverage your SQL skills
Unstructed and unstructured data (schema on read) 

Databases - named bunch of data
Tables - files in subdirectories; can be managed internally in HDFS or external of Hive.
Partitions - a directory used to reduce the size of a table which makes data reads/filters faster
Buckets - a file in a directory table used to manage tables; more efficient that 1000s of small partitions


## Pig


## Use Cases
Long batch processing jobs(log parsing, ETL)? Hive

Real-time analytics? Redshift

## Resources
[All about Hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop)