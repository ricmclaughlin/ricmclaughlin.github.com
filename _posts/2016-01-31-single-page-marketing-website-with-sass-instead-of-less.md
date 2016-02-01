---
layout: post
title: "Single Page Marketing Website with Sass instead of Less"
description: ""
category: projects
tags: [jquery, gulp, sass, htmlcss]
---
{% include JB/setup %}

This project is a walk through of the Agency Landing Page Using LESS of [Udemy's Projects in Bootstrap](https://www.udemy.com/learn-bootstrap-development-by-building-10-projects/learn/#/). 

This project was supposed to be created using non-tooled starter template but I used the [gulp-webapp](https://github.com/yeoman/generator-gulp-webapp) generator via [Yeoman](http://yeoman.io/) and lots of [Bower]() packages.

Seeing that Less is on the way out I rewrote this project in Sass. Every time I use Sass I am struck with how it really helps clean up and organize CSS... the way it should be. Specifically, I have come to appreciate:

1. Sass Variables - Creating variables for commonly used constants that you are likely to change is a variable - isn't that the definition of a variable?? Colors, fonts, sizing, columns and anything else you might define then slightly modify for different use cases are all excellent candidates for variables. Because they are variables you can do mathematic, string maniupluation and data types specific changes to them. Operations like lighten or darken colors can really transform CSS into a much more descriptive language.

2. Nested rules - Akin to subclassing in traditional object oriented programming languages to keep code nice and [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), nested rules work remove duplication in CSS. 

3. Autoprefixer - Yet another reason to use a task runner for building your website, [gulp-autoprefixer](https://github.com/sindresorhus/gulp-autoprefixer) pretty much eliminates the need to remember what you need to write a vendor prefix for and luckily eliminates the need to look at [canIUse.com](http://caniuse.com/) ever again! I love [gulp](http://gulpjs.com/).

Just because I love jQuery so much, I implemented the jQuery [Easing plugin](http://gsgd.co.uk/sandbox/jquery/easing/) to do a nice little scrolling technique from the menu. Nothing to amazing but fun to implement!

This project is part of my [portfolio](http://ric.mclaughlin.today/prj_btstrp_agency).

The source code is on [Github](https://github.com/ricmclaughlin/prj_btstrp_agency).