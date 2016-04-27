---
layout: post
title: "A Node URL Shortener MicroService"
description: ""
category: projects
tags: [node, backend, javascript, express, basicback, gulp]
---
{% include JB/setup %}

This is another FCC project that develops an API - this time the API models a straightforward URL shortener. The user stories are:

1. User Story: I can pass a URL as a parameter and I will receive a shortened URL in the JSON response.

1. User Story: If I pass an invalid URL that doesn't follow the valid http://www.example.com format, the JSON response will contain an error instead.

1. User Story: When I visit that shortened URL, it will redirect me to my original link.

I used [node v4.2.3](https://nodejs.org/en/), [Express 4](http://expressjs.com/) with a project I scaffold myself using [gulp.js](http://gulpjs.com/) to implement this API.

The example app our friends at FCC produced was missing lots of error handling and scalability issues. For instance, the "shortURL" was an integer value which isn't going to scale well after a while. There are a limited number of integer values, right? So I implemented the "shortURL" as a 5 character randomly generated string. When the user tries to access shortened URL that can not be found the application generates and error as well.

The goofy part of these API projects is the lack of CRUD logic that is so common in most applications and that is what makes them so one-offish, if that is a word. Sure I spend a bit of time creating code but the code doesn't follow the established application patterns I've spent so much time creating in the past. And we are not just talking sync vs async here - resources in REST APIs are a well known pattern and these APIs don't really implement those patterns... I guess that is all up to me to learn and demonstrate!

I implemented integration tests using [supertest](https://github.com/visionmedia/supertest), [should](https://shouldjs.github.io/) and [gulp-mocha](https://github.com/sindresorhus/gulp-mocha). I deployed the API to [heroku](https://www.heroku.com/). 

* The source code for this application is at [https://github.com/ricmclaughlin/fcc_url_shortener](https://github.com/ricmclaughlin/fcc_url_shortener).

* The live running version of this application is at [https://radiant-scrubland-18842.herokuapp.com/](https://radiant-scrubland-18842.herokuapp.com/).
