---
layout: post
title: "Thoughts on MongoDB Use"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I've been working a really great free course on MongoDB specifically for [node.js developers](https://university.mongodb.com/courses/M101JS/about). A great class that has been improved here recently, the datamodeling section has been carried over from the last version of the class but the challenges are new. 

The data modeling section, which week 4 of 7 week class, focuses on understanding how data modeling in relational database compares to data modeling in a document oriented database. Here is a quick of what I got from this session:

1. MongoDB doesn't offer constraints and this drive the desire to embed relationships in a document. For example in a blog post collection embedding comments inside the blog post document. Embedding eliminates the need to cascade deletes or updates to related data in related tables - something your app would have to do on its own. 

2. MongoDB doesn't have ACID level transactions; it supports atomic transactions. And atomic transactions are a bit of a misnomer in my book. The course describes atomic transactions to the ability to save all of a single document or multiple documents within a collection OR none of them. In our blog example above, if your app said "save this blog document with embedded comments and authors" MongoDB would save the record in its entirety - it can not write just the blog post and not the embedded comments and authors. I'm a little unclear how that is transaction - you are writing a single document which just another saying you are saving a record to the database. Atomic transactions seems a lot like someone at MongoDB said "Hey we need transactions, let's name this basic feature atomic transactions and no one will figure it out."

3. One to one relationship should probably always be embedded.

4. One to few should also probably be embedded.

5. Many to many relationships should be modeled in seperate collections. An array of related document in the other collection should go one way and perhaps both ways. In other words, if we are modeling students and teachers and want find which teachers have which students we would create a "student array" in the teacher collection and store the <code>_id</code> of the student in the array. If we also want to find which students have which teachers we would also model a "teacher array" of teachers in the students collection as well.

As a related note, I got a huge amount of insight into MongoDB's use in a production system reading this blog post and all the amazing number comments. Really got me wondering if I would attempt using MongoDB in a production system. [Checkout the blog post](http://www.sarahmei.com/blog/2013/11/11/why-you-should-never-use-mongodb/) 
