---
layout: post
title: "Express Request Header Parsing API"
description: ""
category: projects
tags: [node, backend, javascript, express, basicback, gulp, heroku]
---
{% include JB/setup %}

This is the second FCC project that develops an API. I used [node v4.2.3](https://nodejs.org/en/), [Express 4](http://expressjs.com/) with a project I scaffold myself using [gulp.js](http://gulpjs.com/). 

The API itself is very straightforward and as most things go was a little tricky for all the wrong reasons. The user stories are:

1. I can get the IP address, language and operating system for my browser.

And the return values look something like this: `{"ipaddress":"23.252.51.236","language":"en-US","software":"Macintosh; Intel Mac OS X 10_11_4"}`

I implemented integration tests using [supertest](https://github.com/visionmedia/supertest), [should](https://shouldjs.github.io/) and [gulp-mocha](https://github.com/sindresorhus/gulp-mocha)hero. I deployed the API to [heroku](https://www.heroku.com/).

* The source code for this application is at [https://github.com/ricmclaughlin/fcc_header_parser](https://github.com/ricmclaughlin/fcc_header_parser).

* The live running version of this application is at [https://lit-depths-88158.herokuapp.com/](https://lit-depths-88158.herokuapp.com/).

* The live API is at [https://lit-depths-88158.herokuapp.com/api](https://lit-depths-88158.herokuapp.com/api).