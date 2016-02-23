---
layout: post
title: "MongoDB Application Engineering"
description: "Week 7 in the [M101JS: MongoDB for Node.js Developers](https://university.mongodb.com/courses/M101JS/about) class is all about application engineering on MongoDB."
category: posts
tags: [mongodb, node, backend, javascript]
---
{% include JB/setup %}

Week 7 in the [M101JS: MongoDB for Node.js Developers](https://university.mongodb.com/courses/M101JS/about) class is all about application engineering on MongoDB. This includes information on drivers and the communication with MongoDB from the buiness logic layer or from the front end and the impact replication and sharding have on application design and development.

This week's lesson continues to add to my troubling thoughts about MongoDB from an application development perspective. The idea that you would have to worry about what is happening with writes to the database from the the frontend, besides from getting `true` it worked and an `err` that it didn't feels like way to much control and danger in the hands of the typical app developer. Perhaps the typical app developer these days can handle this power - if so then things have changed quite a bit from when I last wrote applications - last week.

1. Write concerns and Network Errors - When you setup a write operation to MongoDB you can configure how you want mongo to respond to your write request using the `w`, `j`, and the `wtimeout` values. The `w` value sets how many of the nodes in the replica set are written to prior to returning although setting to `majority` is ideal, `j` sets the request acknowledgement for a journal write, and `wtimeout` sets the write timeout. You could set these value in the driver, the collection or ideally at the replica set level.

2. Replication - Replica Set Elections - A group of MongoDB servers can be configured into a replication set which improves availablity and fault tolerance. In a simple replication set of three servers, writes occur to the the primary node and data is asyncronously replicated to the 2 secondary nodes. If the primary server is removed from the set or fails, the remaining two nodes will elect a new primary and that all happens without action on the front end. At a more granular level, there are 3 additional types of replica set nodes: Arbiter nodes which helps vote a new replica set primary node, a delayed node which keeps a warm backup of data written to the replica set and can't become a primary node, and hidden nodes are pretty much the same thing - they can't become primary.

2. Write Consistency - So we know that writes must go to the primary MongoDB replication server. But reads can occur from any server yet the secondary servers may or probably will contain stale data. 

2. Creating a Replica Set & Replica Set Internals - Reading from a secondary server must be enabled using the `rs.slaveOK()` command. You can run a combination of mmapv1 and wiredTiger in a replica set. The oplog is another capped collection that is replicated from one node to another.

3. Failover and Rollback - Yet another scary set of use cases for MongoDB in a transactional system - in a failover scenario, if the oplog is not full sync'd across the replica set there can be a data rollback and that rollback must be manually applied. Yikes. Just another reason to use MongoDB as a read centric database where data integrity isn't soo important. 

3.  The nodejs driver - Connecting to a replica set from the node.js driver is super easy. Just list one of the nodes in the set in the seedlist and the other nodes get discovered automajically. Not sure why that simple statement would require a 5 minutes video but it did. If your app writes to a primary node and we get a failover, the node.js driver buffers the operations and the callback will be called after success of the operation. No additional code required on the app side.

4. Read Preferences - There are 5 read preferences that are apptly named: Primary, Primary Preferred, Secondary, Secendary Preferred and Nearest.

2. Building a Sharded Environment - Sharding is the ability to horizonally scale a MongoDB instances using a range of `_id` and shard key and uses a different router called `mongoS`. If you don't include a shard key in your request, mongoS must communicate with all the nodes in the replica set.

5. Sharding + Replication - This is the holy grail of mongoDB deployments. Quite complicate but requires little addition



