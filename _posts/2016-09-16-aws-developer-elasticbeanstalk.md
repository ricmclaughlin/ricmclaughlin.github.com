---
layout: post
title: "AWS - ElasticBeanstalk"
description: ""
category: posts
tags: [aws, developercert, vpc, elasticbeanstalk]
---
{% include JB/setup %}

[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) is the way to get started with AWS with a website. Like Cloud Formation, you don't pay to use the tool BUT you do pay for the resources it provisions.

Elastic Beanstalk supports a webserver or worker environments. On the webserver side it supports IIS, nginx, tomcat, PHP, python, node.js or Ruby. It also includes Docker support.

EB supports Web Server Environment Tiers and Working Environments as well with a SQS queue between the tiers. These are simplified environments yet look quite functional for small to medium sized systems.

# Resources

## Qwik Labs

* [Introduction to AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2935) - This is a low cost (1 credit) lab that simply walks through the basic stuff of ElasticBeanStalk.

* [Working with AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2559)

* [Building Scalable Web Applications with AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2597)

## Reading
[AWS Elastic Beanstalk Developers Guide](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)