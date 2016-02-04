---
layout: post
title: "My Gulpfile.js Recipe - Part 2"
description: ""
category: projects
tags: [JavaScript, gulp, angularjs, travis, coveralls, devops-ci]
---
{% include JB/setup %}

## Angular Gulp Recipe

Continued from <a href={{ BASE_PATH }}"/projects/my-start-on-gulpfiles.js-recipe">part 1</a>

6. Building a production distribution with [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin), [del](https://github.com/sindresorhus/del) & vanilla [gulp](http://gulpjs.com/) - Squishing the size of images is worthy of some build code and that is exactly what [gulp-imagemin](https://github.com/sindresorhus/gulp-imagemin) does for us. No need for any additional gulp plugins to copy files around but the npm module <code>del</code> works like a champ to delete files using globbing patterns.

7. Angular Template Caching with [gulp-angular-templatecache](https://github.com/miickel/gulp-angular-templatecache) & [gulp-minify-html](https://www.npmjs.com/package/gulp-minify-html) - Template caching, which is simply the ability to cache chunks of HTML client side to reduce the number of HTTP requests, is a serious pain without a tool to help you manage it. These two plugins do great job of doing just that - make things faster in your AngularJS app.

8. Optimizing a production build with [gulp-useref](https://www.npmjs.com/package/gulp-useref) - Reducing the number of HTTP requests to load included files is easily accomplished using <code>gulp-useref</code> - version 3 of this plugin has greatly simplified implementation but most examples using gulp-useref haven't been updated to the newer syntax. Be warned! 