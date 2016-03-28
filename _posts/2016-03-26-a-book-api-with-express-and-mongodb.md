---
layout: post
title: "A Book API with Express and MongoDB"
description: ""
category: 
tags: [node, backend, javascript, express, mongodb]
---
{% include JB/setup %}


This project is a very straightforward API for books implemented on [node v4.2.3](https://nodejs.org/en/), [Express 4](http://expressjs.com/) and [mongoDB]() with a project I scaffold myself using [gulp.js](http://gulpjs.com/). 

The API itself is pretty straightforward and as most things go was a little tricky for all the wrong reasons. The user stories are:

1. I can pass a string as a parameter, and it will check to see whether that string contains either a unix timestamp or a natural language date (example: January 1, 2016)

2. If it does, it returns both the Unix timestamp and the natural language form of that date.

3. If it does not contain a date or Unix timestamp, it returns null for those properties.

And the return values look something like this: `{ "unix": 1450137600, "natural": "December 15, 2015" }`

I implemented integration tests using [supertest](https://github.com/visionmedia/supertest), [should](https://shouldjs.github.io/) and [gulp-mocha](https://github.com/sindresorhus/gulp-mocha)hero. 


I implemented unit tests using 

I deployed the API to [heroku](https://www.heroku.com/) which is still ridiculously easy. 

* The source code for this application is at [https://github.com/ricmclaughlin/fcc_express_time](https://github.com/ricmclaughlin/fcc_express_time).

* The live running version of this application is at [https://tranquil-harbor-71589.herokuapp.com/](https://tranquil-harbor-71589.herokuapp.com/).

