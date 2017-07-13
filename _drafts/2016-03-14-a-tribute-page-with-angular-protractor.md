---
layout: post
title: "A Tribute Page using Angular & Protractor"
description: ""
category: projects
tags: [angularjs, tdd, frontend, jasmine, basicfront, gulp, protractor]
---
{% include JB/setup %}




This project is the third of many projects I'll be doing as part of the [Free Code Camp Front End Certification](http://www.freecodecamp.com/challenges/build-a-random-quote-machine)...

The use cases are pretty straightforward:

* As a user, I can view a tribute page with an image and text.

* As a user, I can click on a link that will take me to an external website with further information on the topic.

So, yeah, my app does that. 

In fact, my test suite I developed in [protractor](https://angular.github.io/protractor/#/) proves it. Grab the source, run `npm install`, then `bower install`, then `gulp protractor:src` and check it out!

It is very cool that FCC lets you use what ever tools you want to create these projects. And seeing that there almost no value in creating simple javascript or jQuery apps these days, my versions of these projects are developed with AngularJS 1.4 using professional, real-world tool chain and TDD. In my world, project scaffolding, webpack, sass and gulp, which is packaged nicely together in the amazing [gulp-angular](https://github.com/Swiip/generator-gulp-angular) yeoman generator, is critical towards developing something maintainable. This projects uses [gulp](http://gulpjs.com/), [nginject](https://www.npmjs.com/package/gulp-ng-inject), [ui.router](https://github.com/angular-ui/ui-router), es2015, [protractor](https://angular.github.io/protractor/#/), jasmine, babel, browserify, [browsersync spa](https://github.com/shakyShane/browser-sync-spa), Phantomjs and a ton more good stuff. 

Interesting tid-bits I learned on this project:

1.  After scaffolding the project with `yo` protractor fails with a `Could not find chromedriver ...` error. A simple `webdriver-manager update` solves this problem - actually this is bug in the gulp file. If you run `protractor:src` first protractor runs without issue.

1. The best way to simplify your protractor tests is to use the [page object](https://docs.google.com/presentation/d/1B6manhG0zEXkC-H-tPo2vwU06JhL8w9-XCF9oehXzAQ/edit#slide=id.p) model to define the UI in a file with a `.po.js` file and create the tests in a `.spec` file.

* Live Project URL: [http://ric.mclaughlin.today/fcc_tribute/](http://ric.mclaughlin.today/fcc_tribute/)

* Github Repository: [https://github.com/ricmclaughlin/fcc_tribute](https://github.com/ricmclaughlin/fcc_tribute)
