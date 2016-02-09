---
layout: post
title: "Another Task List App Except with HTML5 Local Storage"
description: ""
category: projects
tags: [javascript, gulp, sass, htmlcss]
---
{% include JB/setup %}

This project is a walk through of the Stickys Project of [Udemy's Projects in HTML5](https://www.udemy.com/projects-in-html5/learn/#/). 

This project was supposed to be created using non-tooled starter template but I used the [gulp-webapp](https://github.com/yeoman/generator-gulp-webapp) generator via [Yeoman](http://yeoman.io/) and lots of [Bower]() packages. And yes, it uses lots of my gulp best practices outlined on my gulp recipe and 

This project feels so low level and free of cruft. Nothing like writing directly against the bare-metal [Document Object](http://www.w3schools.com/jsref/dom_obj_document.asp) to get in touch with the power of good old-fashioned js. Features I learned something from include:

Offline support using a cache manifest - Although not rocket science the [technique of creating an application manifest](http://www.html5rocks.com/en/tutorials/appcache/beginner/) enables the user to save the application and use it off line. 

[Web SQL](https://en.wikipedia.org/wiki/Web_SQL_Database) - The ability to store local data and access it using SQL is rocket science. Where has this been all my life. Super easy to implement.

This project is part of my [portfolio](http://ric.mclaughlin.today/prj_html5_stickys).

My version of this project, which has a lot of lint errors, is on [github at prj_html5_stickys](https://github.com/ricmclaughlin/prj_html5_stickys).

