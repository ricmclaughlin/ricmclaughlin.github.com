---
layout: post
title: "AWS Elastic Beanstalk CLI Exploration"
description: ""
category: 
tags: [aws, elasticbeanstalk, aws-dev-ops-pro, aws-solutions-arch-pro, cli-exploration]
---
{% include JB/setup %}

This is a very simple exploration of [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) from the [AWS CLI](https://aws.amazon.com/cli/). To get started with the CLI first [install it](http://docs.aws.amazon.com/cli/latest/userguide/installing.html), then use `aws configure` to [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for your account and region. 

There are numerous areas to explore with Elastic Beanstalk... during this scenario we are going to create a Docker Application and deploy it in us-west-2.

## eb Command Line Tool

To setup an Elastic Bean Stalk environment from the command line:

`eb init` - this configures the environment for use with the eb including region, credentials, platform, and keypair. Use a `.ebignore` file like a `.gitignore` file


