---
layout: post
title: "Super Easy REST Demo Server"
modified: 2015-09-30 11:13:13 -0800
tags: [express, backend]
categories: projects
---
You can't write a single page app without an API of some sorton the backend. However, getting a simple backend up for demo purposes up and running should not require busting out [Expressjs](http://expressjs.com/) or [Sails.js](http://sailsjs.org/). Or that would be the hope.

When faced with this situation I have started using the super cool [JSON-server](https://github.com/typicode/json-server) in conjunction with [faker](https://github.com/marak/Faker.js/)
 &amp; [lodash](https://lodash.com/).

 I can't add much to this [great video](https://egghead.io/lessons/nodejs-creating-demo-apis-with-json-server) by John Linquist over at egghead.io except to point you to my already packaged solution I made based on his and several other solutions like this. My version is setup to generate blog posts but can be changed to serve up any sort of data in about 30 seconds... [check it out](https://github.com/ricmclaughlin/demo-rest-server).