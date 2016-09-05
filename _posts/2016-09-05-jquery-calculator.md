---
layout: post
title: "Free Code Camp Project - jQuery Calculator"
description: ""
category: projects
tags: [basicfront, frontend]
---
{% include JB/setup %}
This project is one of many projects I'll be doing as part of the [Free Code Camp Front End Certification](http://www.freecodecamp.com/challenges/build-a-random-quote-machine)...

The use cases are pretty straightforward:

* User Story: I can add, subtract, multiply and divide two numbers.

* User Story: I can clear the input field with a clear button.

* User Story: I can keep chaining mathematical operations together until I hit the equal button, and the calculator will tell me the correct output.

So, yeah, my app does that.

But more importantly I played around with the implementation some common features present in many calculators.

* The `CE` feature. Without the ability for the user to understand if the `CE` feature worked or not from the UI, I did not implement this feature.

* `ESC` on your keyboard should result in the clear operation on the calculator.

* `Enter` or `Return` on your keyboard should result in the `=` operation.

It is very cool that FCC lets you use what ever tools you want to create these projects. In this case, I used [jQuery 3.1](https://blog.jquery.com/2016/07/07/jquery-3-1-0-released-no-more-silent-errors/) with no css framework at all.

* Live Project URL: [http://ric.mclaughlin.today/fcc_calc/](http://ric.mclaughlin.today/fcc_calc/)

* Github Repository: [https://github.com/ricmclaughlin/fcc_calc](https://github.com/ricmclaughlin/fcc_calc)
