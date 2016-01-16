---
layout: post
title: "Random Quote Machine Project"
modified: 2014-05-04 18:49:32 +0200
tags: [angularjs, tdd, frontend, jasmine, basicfront]
categories: projects
---

This project is the second of many projects I'll be doing as part of the [Free Code Camp Front End Certification](http://www.freecodecamp.com/challenges/build-a-random-quote-machine)...

The use cases are pretty straightforward:

* As a user, I can click a button to show me a new random quote.

* As a user, I can press a button to tweet out a quote.

So, yeah, my app does that.

It is very cool that FCC lets you use what ever tools you want to create these projects. And seeing that there almost no value in creating simple javascript or jQuery apps these days, my versions of these projects are developed with AngularJS 1.4 using professional, real-world tool chain and TDD. In my world project scaffolding, webpack, sass and gulp, which is packaged nicely together in the amazing [gulp-angular](https://github.com/Swiip/generator-gulp-angular)  yeoman generator, is critical towards developing something maintainable. This projects uses [gulp](http://gulpjs.com/), [nginject](https://www.npmjs.com/package/gulp-ng-inject), [ui.router](https://github.com/angular-ui/ui-router), es2015, protractor, jasmine, babel, browserify, browsersync, Phantomjs and a bunch more stuff. 

For this project I didn't go full force TDD. I implemented tests for the controller and service but didn't implement any e2e tests. I'll be doing that on future projects. 

Interesting tid-bit I learned on this project:

1. If you try to lighten up up and use Angular 1.4 jqLite instead of regular jQuery and try to add the regular tweet javascript inline using <code>script</code> tags or in it's own .js file, you get a nice Content Security Policy in Chrome. So I implemented a nifty sharing directive from [Jason Watmore](http://jasonwatmore.com/post/2014/08/01/AngularJS-directives-for-social-sharing-buttons-Facebook-Like-GooglePlus-Twitter-and-Pinterest.aspx) that works like a champ... but requires jQuery. 

1. There are lots of APIs around for quotes and they all suck. The one used by the majority of campers is XX and it doesn't emit proper JSON. So, I embbedded the quotes required for this project in an angular service.

1. ui.router is the coolest routing solution I have seen. The idea of a state machine applied to router is nifty.