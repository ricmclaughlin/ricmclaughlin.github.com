---
layout: post
title: "My Gulpfile.js Recipe - Part 1"
description: ""
category: projects
tags: [javascript, gulp, angularjs, travis, coveralls, devops-ci]
---
{% include JB/setup %}

## Angular Gulp Recipe

In the quest to build the ultimate Angular Gulpfile, I worked through a couple of gulp classes and then set out to make my own using smatterings of everything I have learned. This is the result. Technics I have implemented here include:

0. Helpers & Organization - Lots of folks dislike gulp because it gets unweldy as the tasks pile up. Welcome to development. There is a reason it is complex. It's the nature of software. That said, there are a couple of things I have figured out to simplify were you can. And there are so many hints and bits of sunshine that I made a new post on [gulpjs helpers, organization and best practices]({{ BASE_PATH }}/project/gulpjs-helpers-organization-best-practices).

1. Syntax checking with [JSHint](https://github.com/spalger/gulp-jshint) & [JSCS](https://github.com/jscs-dev/gulp-jscs) - I'm not a super big fan of syntax error checking in my dev tool. I like it at the command line so some of the functionality of JSHine and JSCS are lost on me. But that does NOT mean I don't like it at compile time. The ability to keep good style and syntax is great when integrated into the build task. 

2. CSS Compilation with [gulp-less](https://www.npmjs.com/package/gulp-lessgulp) & [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer) - The website I did this project on uses [less](http://lesscss.org/) instead of [Sass](http://sass-lang.com/) so this app isn't an exact recipe of how I do things - less is fading and Sass is on the rise. So lets talk Sass instead of less - the speed advantage of [libsass](http://sass-lang.com/libsass) over [Compass](http://compass-style.org/) makes the automation of these processes worth the weight of implementation and when you add CSS vendor prefixing using [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer) to the mix... well, you have a ton of goodness.

3. HTML, CSS & JS Injection with [wiredep](https://www.npmjs.com/package/wiredep) & [gulp-inject](https://www.npmjs.com/package/gulp-inject) - Automajically adding [bower](http://bower.io/) dependencies using an already-compatible-with-gulp [wiredep](https://www.npmjs.com/package/wiredep) makes this problem disappear. Combining and "injecting" CSS dependencies and JavaScript dependencies that YOU authored or added outside of bower in tools like [npm](https://www.npmjs.com/) is easily accomplished with [gulp-inject](https://github.com/klei/gulp-inject). The big advantage is that you no longer have to worry about wiring up all the dependencies in your base template - add your files to correct directory using the right tools and everything happens for you... easily.

4. Serving a dev build with [gulp-nodemon](https://www.npmjs.com/package/gulp-nodemon) - All this building and transformation of your files makes it very important to preview the dev, qa AND production app locally. Luckily there is a nice little package that allows you to integrate a live server with automatic reloads when you change to source files. 

5. Keeping your view in sync with your code using [BrowserSync](https://www.browsersync.io/) - Any tools that says "It's wicked-fast and totally free" gets my attention, wicked-fast regardless of what it does. But BrowserSync is useful - it updates your browser with the most up-to-date code that probably just modified. It also has a "ghost-mode" which enables you to do things like scroll in one browser and have all the other browsers connected to the BrowserSync session to do the same scrolling at the same time. Perhaps this is a bit gimmicky at first but when doing the holy grail of remote cross browser testing this really comes in handy!

Continued in <a href={{ BASE_PATH }}"/projects/my-start-on-gulpfiles.js-recipe-part2">part 2</a>