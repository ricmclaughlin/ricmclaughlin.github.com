---
layout: post
title: "Travis Setup with Jasmine, Mocha and Istanbul"
modified: 2014-05-04 18:49:32 +0200
tags: [angularjs, tdd, frontend, jasmine, travis]
categories: projects
---

After conquering a good alternative setup with {Link to alternative pos} with chai and mocha, I took a first stab at my preferred TDD/CI/code coverage setup. Or at least what I think will be my preferred setup.

First I setup Travis which is straightforward. Get setup with Travis by connecting your github account. Select which github repositories you want to build with travis.  

The next few steps might be easy depending on your project config. Seeing that most front end project use bower, you'll want to add a npm install command and  run bower commans to the <code>before_script:</code> section of your <code>.travis.yml</code> file. 


instanbul setup
protractor setup. 

travis setup page
link to project
link to live project