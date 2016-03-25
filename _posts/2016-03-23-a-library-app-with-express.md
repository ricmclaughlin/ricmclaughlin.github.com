---
layout: post
title: "A Library App with Express"
description: ""
category: projects
tags: [node, backend, javascript, express]
---
{% include JB/setup %}

This app was anti-climatic. I set out to create an app that uses [express](http://expressjs.com/) against [PostgresSQL](http://www.postgresql.org/) but ended up playing with the router and templating functions and looking for something more interesting to develop. I just can't make my self make an HTML based application anymore. Guess that is a sign of the times. 

I implemented a very, very basic gulp file for this app.

The highlight of this application I used several different templating engines along the way. First, I used jade to create a few pages. And came to the conclusion that if you like python you love jade. It's all about the indentation. Second, I used  handlebars which wasn't the the best, I'm an angular guy so the curly brackets threw me. Then I settled on [ejs](http://www.embeddedjs.com/) which is really easy to use and just requires a lot of typing `%`. 

This project is on git repo at [https://github.com/ricmclaughlin/basic_express](https://github.com/ricmclaughlin/basic_express).
