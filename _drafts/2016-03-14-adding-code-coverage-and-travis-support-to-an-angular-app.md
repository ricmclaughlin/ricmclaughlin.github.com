---
layout: post
title: "Adding Code Coverage and Travis Support to an Angular App"
description: ""
category: 
tags: [angularjs, tdd, chai, protractor, frontend, bootstrap, coveralls]
---
{% include JB/setup %}

I find myself adding the same plugins to several projects right in row - I'm developing angular.js apps, hosting the source on github then on success I'm pushing the built app to the gh-pages branch also on github.

To do that I start with the [generator-gulp-angular](https://github.com/Swiip/generator-gulp-angular).

Then I add

coveralls
instanbul
gulp-gh-pages


