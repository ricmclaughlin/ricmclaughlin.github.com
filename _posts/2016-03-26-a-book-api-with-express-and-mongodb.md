---
layout: post
title: "A Book API with Express and MongoDB"
description: ""
category: projects
tags: [node, backend, javascript, express, mongodb, heroku]
---
{% include JB/setup %}


This project is a very straightforward API for books implemented on [node v4.2.3](https://nodejs.org/en/), [Express 4](http://expressjs.com/), [mongoose](http://mongoosejs.com/) and [mongoDB](https://www.mongodb.com/) with a project I scaffold myself using [gulp.js](http://gulpjs.com/). 

The API, which returns JSON with some embedded HATEOS, itself is pretty straightforward and as most things go was a little tricky for all the wrong reasons. The user stories are:

1. As an API user, I can submit a book with a title, author, genre and a "read" boolean value. 

1. As an API user, I can submit a book only if it has a title.

3. As an API user, I can filter books based on genre.

4. As an API user, I can edit book's title, author, genre and read values but not the `_id`

3. As an API user, I can request a single book's details by `_id`

I implemented integration tests using [supertest](https://github.com/visionmedia/supertest), [should](https://shouldjs.github.io/) and [gulp-mocha](https://github.com/sindresorhus/gulp-mocha) and unit tests using [should](https://shouldjs.github.io/) and [sinon](http://sinonjs.org/).

I deployed the API to [heroku](https://www.heroku.com/) using the which is still ridiculously easy and integrated mongodb using [mLab](https://www.mlab.com). mLab had availablity issue for me during development - never a good sign. 

* The source code for this application is at [https://github.com/ricmclaughlin/bookapi](https://github.com/ricmclaughlin/bookapi).

* The live running version of this application is at [https://tranquil-harbor-71589.herokuapp.com/](https://tranquil-harbor-71589.herokuapp.com/).

