---
layout: post
title: "Why JavaScript Data Structures with Commander.js"
description: ""
category: posts
tags: [javascriptclass]
---
{% include JB/setup %}

Automation of daily tasks is the responsibility of developers that are being... responsible. Why waste time when you can code a little and end up with more time to be productive? And when you code something up, you improve the accuracy of the task as well. But that isn't the hook - this is: [A guy named Narkoz automates a work from home email when hungover](http://slides.com/ricmclaughlin/deck-11#/1) - Brilliant. 

If I drank, I'd be all about being like this guy! Regardless, this guy gives me something to shoot for. I've been automating any task I do more than 3 times a day for years - Narkoz automates anything that takes longer than 90 seconds - so his bar for automation adds a new dimension to mine.

The question is how to do this automation on JavaScript. I've used [commander](https://github.com/commander-rb/commander), a excellent tool for ruby automation, and like its combination of simplicity and power so when I found the JavaScript adaptation [commander.js ](https://github.com/tj/commander.js/) I was quickly onboard.

As I implemented several tasks with commander.js, I found myself using the lowly JavaScript array in almost every app I wrote. Then came to a little bit of an epiphany - JavaScript doesn't really offer support of advanced data stucture or even simple ones - it simply uses the array. Then I got thinking, "Most JavaScript programmers are not fimiliar with data structures yet we use them all the time, especially in command line apps. I should write a class about that."

And the rest is history. I wrote a class on using Commander.js and included an introduction to the common array based datastructures including the list, ordered list and array. Check out the [deck](http://slides.com/ricmclaughlin/deck-11#/), the [repo](https://github.com/ricmclaughlin/data_struct_commander_S1) and the [class]({{ BASE_PATH }}/projects/intro-to-javascript-data-structures-with-commanderjs).
