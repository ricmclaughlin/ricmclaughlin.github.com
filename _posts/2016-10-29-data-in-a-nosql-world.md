---
layout: post
title: "Data in a NoSQL world"
description: ""
category: 
tags: []
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


  

Data Normalization
1NF
2NF
3NF

Aggregate Models
Key Value 
  Key - key is unique
  Value - contains attributes

Document Database
  Couch and DynamoDB
  Key Value store
    structure in the value using JSON, XML, or BSON

Column
  Column Agregation.

Graph DB
  Unlike other NoSQL relationships are important
  Many to Many relationships are easy
  Relationships are fluid and persistent - ideal for modeling real-life social links

Column vs Row Oriented Data
  Column Aggregation, improved compression, parallel processing
  Interation or single record processing

