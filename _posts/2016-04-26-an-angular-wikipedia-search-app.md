---
layout: post
title: "An Angular Wikipedia Search App"
description: ""
category: projects
tags: [angularjs, frontend, basicfront]
---
{% include JB/setup %}
This is another FCC project that develops a light-weight front end only application. Checkout the [project page](https://www.freecodecamp.com/challenges/build-a-wikipedia-viewer).

The app itself is very straightforward. The user stories are:

* User Story: I can search Wikipedia entries in a search box and see the resulting Wikipedia entries.

* User Story: I can click a button to see a random Wikipedia entry.

It is very cool that FCC lets you use what ever tools you want to create these projects. And seeing that there almost no value in creating simple javascript or jQuery apps these days, my versions of these projects are developed with AngularJS 1.5 using professional, real-world tool chain and TDD. This projects uses [gulp](http://gulpjs.com/), [nginject](https://www.npmjs.com/package/gulp-ng-inject), [ui.router](https://github.com/angular-ui/ui-router), [protractor](https://angular.github.io/protractor/#/), jasmine, browserify, [browsersync spa](https://github.com/shakyShane/browser-sync-spa), Phantomjs and a ton more good stuff. 

I used a couple of external APIs for this app - the [Wikipedia Random Article Redirect](https://en.wikipedia.org/wiki/Special:Random) for finding a random article and then the [MediaWiki Search API](ttps://www.mediawiki.org/wiki/API:Main_page) to obtain weather information.

* Live Project URL: [https://github.com/ricmclaughlin/fcc_wikipedia](https://github.com/ricmclaughlin/fcc_wikipedia)

* Github Repository: [http://ric.mclaughlin.today/fcc_wikipedia](http://ric.mclaughlin.today/fcc_wikipedia)
