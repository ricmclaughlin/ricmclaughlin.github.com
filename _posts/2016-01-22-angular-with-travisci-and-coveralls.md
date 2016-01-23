---
layout: post
title: "Angularjs with TravisCI and Coveralls"
description: ""
category: projects
tags: [devops-ci, travis, jasmine]
---
{% include JB/setup %}

I'm intrigued with continuous integration. Back in the day builds took hours and a dedicated machine and about half the time the build was broken. And test coverage was just a dream. There just weren’t any tools available.

But here in the modern age of amazingness [TravisCI](https://travis-ci.org/profile/ricmclaughlin) is the bomb. Add a simple <code>.travis.yml</code> file to your project and you are building after every github commit. 

More amazing is how quickly code coverage can be added by piping the output of the project’s Istanbul code coverage in the form of the <code>lcov.info</code> file to the coveralls script and you have it done. 

Most importantly you can now add the build and code coverages badges!
[![Build Status](https://travis-ci.org/ricmclaughlin/angTravis.svg?branch=master)](https://travis-ci.org/ricmclaughlin/angTravis)
[![Coverage Status](https://coveralls.io/repos/github/ricmclaughlin/angTravis/badge.svg?branch=master)](https://coveralls.io/github/ricmclaughlin/angTravis?branch=master) 

<ul>
  <li>Github Repository: <a href="https://github.com/ricmclaughlin/angTravis">https://github.com/ricmclaughlin/angTravis</a></li>
</ul>