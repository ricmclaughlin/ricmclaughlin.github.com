---
layout: post
title: "A Vanilla JS es6 Twitch Channel Stream Viewer"
description: ""
category: projects
tags: [basicfront, frontend]
---
{% include JB/setup %}
#   Free Code Camp Project - Twitch Streamers

This is another in a series of projects as part of the Free Code Camp Front End Development Certification.

This project is designed to be a [jQuery front end for the Twitch API](https://www.freecodecamp.com/en/challenges/intermediate-front-end-development-projects/use-the-twitchtv-json-api) which displays a list of Twitch streaming channels and the streams associated with those channels.

The Official User Stories for this project are:

* User Story: I can see whether Free Code Camp is currently streaming on Twitch.tv.

* User Story: I can click the status output and be sent directly to the Free Code Camp's Twitch.tv channel.

* User Story: if a Twitch user is currently streaming, I can see additional details about what they are streaming.

* User Story: I will see a placeholder notification if a streamer has closed their Twitch account (or the account never existed).

My User Stories are more technical in nature:

* User Story: I will see all application information in a responsive way on a laptop, high res display, iPhone, iPad and Galaxy S5.

* User Story: The page weight inclusive of JS, CSS, images, fonts will be less than 500kb. (Kicked ass on this one - total average page weight is 394kb!)

Technology Choices:

1. Vanilla JS instead of jQuery. After looking at jQuery again a couple of weeks ago... well, it isn't any better than it was before. Feels like a manky set of libraries built on an old version of C... where as Vanilla JS is starting to feel like C++ with a lightweight library. 

2. [Skeleton](http://getskeleton.com/) instead of... something big. Page weight was a driving requirement here and for all my projects moving forward.

3. ES6 instead of ES5 - It's time. Time for me to make the switch to the next version of JS. I just completed bunch of studying back in April and took a great [Wes Bos ES6 for everyone](http://wesbos.com/es6-for-everyone/) and the always great stuff from Mark Zamoyta in this case [Rapid ES6 Training](https://app.pluralsight.com/library/courses/rapid-es6-training/table-of-contents). So it was time to implement some great new front end data access API like `.fetch` and use [generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator) my fav new es6 feature. Add `.map()` and `.reduce() and you have yourself a good old fashion geek fest.

Lessons Learned:
1. Thinking in async is still hard. Even though I've written a crap ton of async code it just never gets easy to figure out how to debug it... or what is going wrong. In this case, there is a list of things that need more data sprinkled on top of them... and each bit of sprinkling requires a `.fetch()` then some logic then a `.fetch()` and a bit of logic. Using es6 generators was a huge big deal on this project.

2. Less CSS is starting to be more. In the past I have hidden my weak CSS skills by using a giant CSS framework like Bootstrap. This feels like it starting to change... 

[The Project Source on GitHub](https://github.com/ricmclaughlin/fcc_twitchtv)
[Link to Live Project](http://ric.mclaughlin.today/fcc_twitchtv)
