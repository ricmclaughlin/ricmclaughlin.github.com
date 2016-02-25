---
layout: post
title: "A mongoDB Project Direct From the Mongo Folks"
description: ""
category: projects
tags: [mongodb, node, backend, javascript]
---
{% include JB/setup %}

While week 7 in the [M101JS: MongoDB for Node.js Developers](https://university.mongodb.com/courses/M101JS/about) class is all about application engineering on MongoDB the project is all about implementing MongoDB in a simple ecommerce application based on MongoDB and Express. This project includes 6 labs that begin with a simple npm based setup and get the app running and progress through the entire data access layer of the application. Specifically, this is what I had to implement:

1. Get mongod running and the app sample data loaded, the NodeJS/Express application installed, and making sure the app was functioning. Easy.

2. Implement a set of simple `find()` and aggregation queries against MongoDB by implmenting `getCategories()`, `getItems()`, and `getNumItems()`.