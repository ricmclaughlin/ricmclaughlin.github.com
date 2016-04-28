---
layout: post
title: "Gulpjs Helpers, Organization and Best Practices"
description: ""
category: projects
tags: [javascript, gulp, devops-ci, travis]
---
{% include JB/setup %}

Lots of folks dislike gulp because it gets unweldy as the tasks pile up. Welcome to development. There is a reason it is complex. It's the nature of software. That said, there are a couple of things I have figured out to simplify were you can:

1.  It's a bit of a obvious statement but gulpjs is a Javascript tool so you can create reusable functions inside your <code>gulpfile.js</code>. Logging application progress and debugging information &amp; deleting files are operations that happen all over so why not DRY up those aspects of your gulpfile up by creating reusable functions?

1. Extracting the configuration including files locations and options passed to plugins can be extracted into a seperate <code>gulp.config.js</code> file. I'm also working an approach that allows file paths specific to a single task to be defined in the separate gulp task files.

1. Extracting related gulp tasks into seperate files using [wrench](https://www.npmjs.com/package/wrench) is a handy way of organizing your code without relying on a defined list of dependencies. 
1.  Loading all of the important gulp plugins ends up being quite a bit of dependency management - I like the approach of loading the plugins dynamically, when needed, using the super handy [gulp-load-plugins](https://www.npmjs.com/package/gulp-load-plugins)? Plus when you use the gulp-load-plugins plugin you get to refer to plugins generically as <code>$</code> and after so much jQuery I kinda miss typing <code>$</code>. This technical approach really helps me feel better! 

1.  [Gulpif](https://github.com/robrich/gulp-if) allows you to conditionally control the low of vinyl objects - which translates into tons of fantastic filtering in your gulp tasks. Yet after using [gulpif](https://github.com/robrich/gulp-if) a bit aren't you violating the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)? I am going in the direction of creating a task per file type per operation instead of processing files on a task per operation basis. This would isolate changes in a more flexible way. Or at least that is what it feels like now.

1. [Gulp-plumber](https://github.com/floatdrop/gulp-plumber) is a [monkey patch](https://en.wikipedia.org/wiki/Monkey_patch) that continues the <code>gulp.pipe</code> if an error occurs in the execution of a <code>.pipe</code>'d command. Handy all over gulp files. Not convinced? I couldn't NOT implement this plugin after reading [this article](https://gist.github.com/floatdrop/8269868).

1.  Listing tasks in your gulpfile should be built in - You are likely to be the only person using your gulpfile and other flat don't want to look into the file to figure out what the hell it does. Please do us all a favor by implementing the [gulp-task-listing](https://www.npmjs.com/package/gulp-task-listing) plugin and making it your default gulp task!