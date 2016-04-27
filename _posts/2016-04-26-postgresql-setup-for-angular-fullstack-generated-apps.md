---
layout: post
title: "PostgreSQL Setup for Angular FullStack Generated Apps"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Getting an app connected to a PostgreSQL database is a tricky error prone thing and a requirement for each separate environment you deploy code to. Here is how I connect my [Angular-Fullstack](https://github.com/angular-fullstack/generator-angular-fullstack) generated application to PostgreSQL.

Before we get to the how, lets spend a minute on what we should be doing here. First, you need to use Postgres in all environments - including development. You should be using a Postgres user login that does NOT have superuser or Admin priveleges because you... should. Yet, the way these apps nuke the database and rebuild and seed a new version of sed database requires superuse priveleges so the DBA of many years ago that still lurks with me is getting over this security issues. You should be doing this sort of thing in `psql` not some sort of silly GUI front end. It is way, way easier to test without mocking the database and assuming that the test data IS in the databases so using the `seed.js` file is handy way to make this happen. That said, lets get to it.

1. create two databases

2. add create user

3. add superuser privileges

4. 




adding an end point