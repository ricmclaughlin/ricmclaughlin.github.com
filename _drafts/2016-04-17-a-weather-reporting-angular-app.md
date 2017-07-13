---
layout: post
title: "A Weather Reporting Angular App"
description: ""
category: projects
tags: [angularjs, tdd, frontend, jasmine, basicfront, gulp]
---
{% include JB/setup %}
This is another FCC project that develops a light-weight front end only application. I used [node v4.2.3](https://nodejs.org/en/), [Express 4](http://expressjs.com/) with a project I scaffold myself using [gulp.js](http://gulpjs.com/). 

The app itself is very straightforward. The user stories are:

* User Story: I can see the weather in my current location.

* User Story: I can see a different icon or background image (e.g. snowy mountain, hot desert) depending on the weather.

* User Story: I can push a button to toggle between Fahrenheit and Celsius.

It is very cool that FCC lets you use what ever tools you want to create these projects. And seeing that there almost no value in creating simple javascript or jQuery apps these days, my versions of these projects are developed with AngularJS 1.5 using professional, real-world tool chain and TDD. This projects uses [gulp](http://gulpjs.com/), [nginject](https://www.npmjs.com/package/gulp-ng-inject), [ui.router](https://github.com/angular-ui/ui-router), [protractor](https://angular.github.io/protractor/#/), jasmine, browserify, [browsersync spa](https://github.com/shakyShane/browser-sync-spa), Phantomjs and a ton more good stuff. 

I used a couple of external APIs for this app - the [ip-api](http://ip-api.com) for the geolocation info and then the [open weather map](http://openweathermap.org/) to obtain weather information.

* Live Project URL: [http://ric.mclaughlin.today/fcc_local_weather/](http://ric.mclaughlin.today/fcc_local_weather/)

* Github Repository: [https://github.com/ricmclaughlin/fcc_local_weather](https://github.com/ricmclaughlin/fcc_local_weather)



