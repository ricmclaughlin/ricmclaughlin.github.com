---
layout: post
title: "Gulpjs Helpers, Organization and Best Practices"
description: ""
category: project
tags: [javascript, gulpjs, devops-ci, travis]
---
{% include JB/setup %}

Lots of folks dislike gulp because it gets unweldy as the tasks pile up. Welcome to development. There is a reason it is complex. It's the nature of software. That said, there are a couple of things I have figured out to simplify were you can:

1.  It's a bit of a obvious statement but gulpjs is a Javascript tool so you can create reusable functions inside your <code>gulpfile.js</code>. Logging application progress and debugging information &amp; deleting files are operations that happen all over so why not DRY those aspects of your gulpfile up by creating reusable functions?
1.  Extracting the configuration including files locations and options passed to plugins can be extracted into a seperate <code>gulp.config.js</code> file; extracting related gulp tasks TODO
1.  Loading all of the important gulp plugins ends up being quite a bit of dependency management - why not load the plugins dynamically, when needed, using the super handy [gulp-load-plugins](https://www.npmjs.com/package/gulp-load-plugins)? Plus when you use the gulp-load-plugins plugin you get to refer to plugins generically as <code>$</code> and after so much jQuery I kinda miss typing <code>$</code> and this really helps me feel better! 
1.  [Gulpif](https://github.com/robrich/gulp-if) allows you to conditionally control the low of vinyl objects - which translates into tons of fantastic filtering in your gulp tasks. Don't use filtering use gulp if 
1. [Gulp-plumber](https://github.com/floatdrop/gulp-plumber) is a [monkey patch](https://en.wikipedia.org/wiki/Monkey_patch) that continues the <code>gulp.pipe</code> if an error occurs in the execution of a <code>.pipe</code>'d command. Handy all over gulp files. Not convinced? This is what sealed the ["convincing to for me"](https://gist.github.com/floatdrop/8269868)
1.  Listing tasks in your gulpfile should be built in - You are likely to be the only person using your gulpfile and other flat don't want to look into the file to figure out what the hell it does. Please do us all a favor by implementing the [gulp-task-listing](https://www.npmjs.com/package/gulp-task-listing) plugin and making it your default gulp task!