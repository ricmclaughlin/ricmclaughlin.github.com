---
layout: post
title: "A Solid Jekyll Bootstrap Starter Project"
description: ""
category: projects
tags: [git, devops-ci, ruby, jekyll, bootstrap]
---
{% include JB/setup %}
For the first serveral sessions of the [Github Pages Portfolio Development with Jekyll, Yeoman, and Gulpjs](projects/gh-pages-portfolio-with-jekyllv1.5), I've been using the best generic boostrap theme I could find - a fork of [jekyll-bootstrap-3](https://github.com/dbtek/jekyll-bootstrap-3)  which is a fork of [jekyll-bootstrap](https://github.com/plusjade/jekyll-bootstrap). 

But this project isn't ready to rock. I've found there are too many mods to the `_config.yml` required to get this project ready for production and why should we waste time making these mods to get things straightened away during the class? Especially considering the mods are all one time things! 

The idea behind this starter kit is to be up and running in just a few minutes and have sensible defaults preset. Here is what I would envision this repo would change from [jekyll-bootstrap-3](https://github.com/dbtek/jekyll-bootstrap-3).

* Up to date bootstrap (currently v3.3.6)

* `_config.yml` updates - Configure Asset path setup properly, pretty permalinks, turn off comments by default

* `.ruby-gemset` & `.ruby-version` - added these files to support good ruby tool chain management 

* up to date code highlighting using `rouge`

* various other updates - fix the `Build Warning: Layout 'nil'` build bug 

This project is on [github](https://github.com/ricmclaughlin/jekyll-bootstrap-3).