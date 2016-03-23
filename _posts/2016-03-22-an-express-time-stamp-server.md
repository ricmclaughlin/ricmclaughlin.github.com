---
layout: post
title: "An Express Time Stamp Server"
description: ""
category: projects
tags: [basicback]
---
{% include JB/setup %}
This is the first FCC project

User stories:
1) I can pass a string as a parameter, and it will check to see whether that string contains either a unix timestamp or a natural language date (example: January 1, 2016)
2) If it does, it returns both the Unix timestamp and the natural language form of that date.
3) If it does not contain a date or Unix timestamp, it returns null for those properties.

{ "unix": 1450137600, "natural": "December 15, 2015" }