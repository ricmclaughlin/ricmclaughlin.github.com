---
layout: post
title: "A MongoDB Project Direct From the Mongo Folks"
description: ""
category: projects
tags: [mongodb, node, backend, javascript, express]
---
{% include JB/setup %}

While week 7 in the [M101JS: MongoDB for Node.js Developers](https://university.mongodb.com/courses/M101JS/about) class is all about application engineering on MongoDB, in week 8 &amp; 9 which are odd seeing that the class is 7 weeks long, the end of the class project is all about implementing [MongoDB](https://www.mongodb.com/) in an ecommerce application based on [node](https://nodejs.org/en/), [Express](http://expressjs.com/) and [nunjucks](https://mozilla.github.io/nunjucks/). This project includes 6 labs that begin with a simple npm based setup and get the app running and progress through creating the entire data access layer of the application. Specifically, this is what I had to:

1. Get mongod running and the app sample data loaded, the NodeJS/Express application installed, and making sure the app was functioning. Easy.

2. Implement a set of simple aggregation queries against MongoDB by implmenting `getCategories()`, `getItems()`, and `getNumItems()`.

3. Implement a set of search features against MongoDB by creating a `searchItems()`, and `getNumSearchItems()` functions.

4. Implement a single item view by creating a `getItem()` function.

5. Implement adding and updating item reviews by creating a `addReview()` function.

6. Implement a `getCart()` function that retrieves the contents of the shopping cart.

So, it would great if I could actually post this project and show a live version of the work I had implemented but, if I did, I'd be giving future students of this class all the answers to the lab. I'm here to help those that help themselves, but that is a bit much. So, imagine I posted the completed lab and you thought it rocked.

Here is the template of the project out on [github but without the lab portions filled in](https://github.com/ricmclaughlin/mongomart).

