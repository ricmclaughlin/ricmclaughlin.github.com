---
layout: post
title: "MongoDB Application Engineering"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Week 7 in the [M101JS: MongoDB for Node.js Developers](https://university.mongodb.com/courses/M101JS/about) class is all about application engineering on MongoDB. This includes information on drivers and the communication with MongoDB from the buiness logic layer or from the front end and the impact replication and sharding have on application design and development.

This week's lesson continues to add to my troubling thoughts about MongoDB from an application development perspective. The idea that you would have to worry about what is happening with writes to the database from the the frontend, besides from getting true it worked and an error that it didn't feels like way to much control and danger in the hands of the typical app developer. Perhas the typical app developer these days can handle this power - if so then things have changed quite a bit from when I last wrote applications - last week.

Write concerns and 

Network Errors 


Replication - Replica Set Elections 


Write Consistency - in replica set or generically


Creating a Replica Set 


Replica Set Internals 


Failover and Rollback 
Review of Implications of Replication 

The nodejs driver - Connecting to a Replica Set from the Node.js Driver 
Failover in the Node.js Driver
Write Concern Revisited 
Read Preferences 





Introduction to Sharding 


Building a Sharded Environment 


Implications of Sharding 


Sharding + Replication 


Choosing a Shard Key 
