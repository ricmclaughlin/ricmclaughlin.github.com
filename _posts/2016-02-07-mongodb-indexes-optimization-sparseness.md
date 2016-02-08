---
layout: post
title: "MongoDB Indexes, Optimization and Sparseness"
description: ""
category: posts
tags: [mongodb, node, backend, javascript]
---
{% include JB/setup %}

First, I'm not sure sparseness is a word. If it isn't then how would you talk about a "sparse index"?

Not sure. But I am sure that the M101JS: MongoDB for Node.js Developers class gets into the details of optimization of indexes &amp; queries is deep. Here is a bit of what I've learned this week about sparseness and other fun MongoDB stuff:

1. MongoDB 3.0, which now is a few versions old now, is the first version to feature pluggable storage engines. MMAP has been the default engine since MongoDB 1.0, and through acquistion of the company [WiredTiger Inc](https://www.mongodb.com/press/wired-tiger), MongoDB now supports the Wired Tiger storage engine. This new storage features document level concurrency instead of optimistic concurrency model locking in MMAP and compression of document and indexes, and no in placed editing like MMAP. All update operations using Wired Tiger are a delete/write new record operations. I can see why this new engine was implemented - it has to scale much better than MMAP. 

2. Indexes have been around for as long as relational databases have been around forever it seems. Yet the ability to index embedded arrays including arrays that might not be present in a document (this is a [sparse index](https://docs.mongodb.org/v3.0/core/index-sparse/) but now superceeded by the new [partial index](https://docs.mongodb.org/manual/core/index-sparse/) in MongoDB 3.2) and the ablility to index adds fun new concepts to the world of indexes. And oh, they are so fun. 

3. I've been pretty open about my thoughts on the use of MongoDB in production systems. It seems like MongoDB takes lots of the concepts we don't have to worry about as application developers and given them to us to chanage around in the guise of improved flexibility of implementation. Covered queries are another one of those issues. To really optimize the use of an index a developer has specify which fields are returned, called a [projection](https://docs.mongodb.org/manual/reference/operator/projection/positional/) in MongoDB terms to keep the database engine from doing a table scan EVEN THOUGH the query is done executed using an index.

4. Like relational databases or SQL systems it is super important to have your indexes in memory else things are slow. Regular indexes end up with an index entry per record or something similiar to a 1:1 ratio; a sparse index ends up with less than 1:1 because NULL values are not indexed; multi-key indexes have an index entry PER array element indexed so these indexes could be massively huge. 

5. [Geospatial indexes](https://docs.mongodb.org/manual/core/geospatial-indexes/) are an easy way to calculate distances on a cartesian grid using the <code>$near</code> find keyword. A spherical geospatial index uses [GeoJSON](http://geojson.org/) to describe a [2dsphere](https://docs.mongodb.org/manual/core/2dsphere/) - It is less clear why they call it a 2dsphere?? Isn't a circle 3 dimensional?

6. A [full text index](https://docs.mongodb.org/manual/core/index-text/) isn't to amazing but is super useful. Doing a mongoDB full text index requires specifing the index <code>find({$text:{$search:'dog'}})</code>instead of using the regular find syntax.

7. Efficiency of indexes, which is simply a simple way of saying how selective the index is, is somehow in a developers class but hey it's MongoDB. There are three basic rules of thumb when creating compound indexes in MongoDB:
* Equality fields first
* Sort fields second
* Range fields last

8. The simple case of poor performance for the 

