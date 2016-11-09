---
layout: post
title: "Data in a NoSQL world"
description: ""
category: 
tags: [aws, solutionsarch]
---
{% include JB/setup %}

SQL vs NoSQL

SQL is mostly standardized, schema, datatype and business rules defined a head of time
relationships
[ACID](https://en.wikipedia.org/wiki/ACID) - safety guarantee
mostly vertical scaling

NoSQL is very, very diverse
no standardized schema, data or business rules
no relationships
[BASE](https://en.wikipedia.org/wiki/Eventual_consistency) - liveness
mostly horizonal scaling

OLTP - constant transactions, operational data, short term data with lots of users and GB of data; robust backups 
OLAP - large periodic updates, complex queries, long term data with few users and TB/PB amounts of data; periodic backups

Data Normalization
1NF
2NF
3NF

Aggregate Models
Key Value
  Redis, DynamoDB, Oracle BDB, Riak 
  Key - key is unique
  Value - contains attributes

Document Database
  Couch and DynamoDB
  CAP Theorem (consistency, availability, partition tolerance)
  Key Value store
    structure in the value using JSON, XML, or BSON

Wide Column Store
  HBase, Apach Cassandra
  Data is grouped into Column, column family and Super Column family
  High Performance

Graph DB
  Neo4J, InfoGrid, Infinite Graph
  Unlike other NoSQL relationships are important
  Many to Many relationships are easy
  Relationships are fluid and persistent - ideal for modeling real-life social links

Column vs Row Oriented Data
  Column Aggregation, improved compression, parallel processing
  Interation or single record processing

## Use Cases

Need ACID? use RDS transactional engine (InnoDB not myISAM); 

Huge OLTP with no ACID? Use DynamoDB


