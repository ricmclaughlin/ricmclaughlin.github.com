---
layout: post
title: "AngularFire + Angular = Goodness"
description: ""
category: 
tags: [angularjs]
---
{% include JB/setup %}

I love Angular. It is by far my favoriate front end JavaScript framework. In this project, I used Angular 1.5.x with [Firebase](https://www.firebase.com/), an awesome database-as-a-service sort of thing, from our friends at [Google](https://www.google.com/). 

Where this solution really shines is speed - of implementation, of data sync AND it is super easy to get up and running. The Firebase folks have created a client side library called AngularFire which enables you to write data to and from their real time database which is very, very close to MongoDB. You also get a simple static hosting solution with Firebase as well - I'm not as fired up about that. Perhaps it as the capital letters in my paths or perhaps it was just Firebase hosting being a bit off. Most of Google services have been having small up time issues this week so perhaps that is somehow related to the deployment problems I saw. 

This application does include what I think is the architecture of the future. The front end application "talks" directly to the backend database which is a service NOT a server. Other services, like text messaging, email and the like that are required to make up the application functionality are integrated into the database service and not called from the front end. In this case, I wrote a small Node.js service that integrated [Twilio](https://www.twilio.com/) and [Mailgun](https://mailgun.com) with Firebase. Firebase emits `child added` events which makes it very straightforward to integrate services together using this sort of architecture.

Never the less, this project turned out well. I like the results and getting up on gh-pages was easy as well.

* Live Project URL: [Wait-n-Eat](http://ric.mclaughlin.today/wait-n-eat/#/)

* Github Repository: [Source](https://github.com/ricmclaughlin/wait-n-eat)