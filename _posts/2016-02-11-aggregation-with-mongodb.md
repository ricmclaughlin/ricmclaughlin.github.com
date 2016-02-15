---
layout: post
title: "Aggregation with MongoDB"
description: ""
category: posts
tags: [mongodb, node, backend, javascript]
---
{% include JB/setup %}

Week 6 in the [M101JS: MongoDB for Node.js Developers](https://university.mongodb.com/courses/M101JS/about) class is all about the aggregation pipeline first built into MongoDB [version 2.1](http://blog.mongodb.org/post/16015854270/operations-in-the-new-aggregation-framework). At first I can't say I really understood why MongoDB had this feature.

Then I saw this page in the [MongoDB manual](https://docs.mongodb.org/manual/) and I got the value in it. Can't quite figure out if it is unique to MongoDB - in other words, do other database systems have this sort of feature and I have't seen it? OR is MongoDB out in front of the pack with this feature? I've been asking some folks and doing some reading about this and I'll report back when I figure this out. 

Here are a couple of the things I have learned about the MongoDB agregation pipeline (AP) this week:

1. Say [MongoDB query operators](https://docs.mongodb.org/manual/reference/operator/query/) and [bash pipes](http://www.gnu.org/software/bash/manual/html_node/Pipelines.html) got it on and had a baby - that is exactly what AP is. Something cute. Something sweet. Something not totally fimiliar yet useful.

2. From a code perspective a pipeline is a series of steps that match, project, sort, skip and limit a set of records from a MongoDB collection. I'm thinking you could write a pipeline in any MongoDB supported language and that would include JavaScript. 

3. [$unwind](https://docs.mongodb.org/manual/reference/operator/aggregation/unwind/) has no real equivalent in the SQL world - it "denormalizes" embedded arrays.

4. Accumulators - Just like agregate functions in [PostgreSQL](http://www.postgresql.org/docs/9.5/static/functions-aggregate.html) MongoDB has an entire group of [group accumulators](https://docs.mongodb.org/manual/reference/operator/aggregation-group/) such as the typical <code>$sum</code> $avg plus the very cool `$push` which pushes the value onto an array.

Overall, another great class on MongoDB. Up next is a project and this class is done. Can't wait to knock that out and be done!




