---
layout: post
title: "AWS - ElasticBeanstalk"
description: ""
category: posts
tags: [aws, developercert, vpc, elasticbeanstalk, devopspro]
---
{% include JB/setup %}

[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) is the way to get started with AWS with a website or backend process. Like Cloud Formation, you don't pay to use the tool BUT you do pay for the resources it provisions.

Elastic Beanstalk supports a webserver or worker environments. On the webserver side it supports IIS, Nginx, tomcat, PHP, python, node.js or Ruby. It also includes Docker support.

EB supports Web Server Environment Tiers and Working Environments as well with a SQS queue between the tiers. These are simplified environments yet look quite functional for small to medium sized systems.

EB is appropriate to use when creating quick applications including prototypes that require minimal control and flexibility to power the application. EB is not appropriate when there are lots of additional dependency software and the existing application that does not fit into the EB model.

## EB components

An Application is the logical collection of environments, versions, environment configurations similiar to a folder that serves as the basic concept behind EB. A configuration template is the starting spot for configuring an application.

managed update - new features that enables updates during a scheduled maintenance window

environment types - can be changed
http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features-managing-env-types.html
  load balancing and autoscaling
  single instance

deployment types
  http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html
  all at once - fast with no DNS change; causes downtime; 
  rolling - deploy in batches based on number or percentage; upon re-attachment to ELB health

  what is the command timeout associated with? where does the health check come from??
  if deployment fails partially, manual termination of new version instances revert to last version that is good.
  connection draining..

  rolling with additional batch - like rolling but adds more new capacity into ASG to seed deployment
  
  immutable - no downtime; add ASG with new version to test; easy rollback; doubles number of instances temporarily which may bump up against EC2 region limits and costs money; updates during maintenance window use this method

  Blue/Green - no downtime; just like immutable but with a new ELB as well; switch DNS to change from blue to green; Database layer gets replaced too so lots of times folks provision RDS outside of Elastic Beanstalk.

### Update and Deploy types
http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html
healthy

ignore healthchecks - should the deployment wait until the instance comes up healthy or simply continue...

batch size - 

App created from console or eb cli? rolling deployment
App created from SDK or AWS CLI? all at once deployment
rolling 

## EB Environment Settings

The EB environment has many settings that are evaluated from higher to lower in precendent 

Direct changes to the environment - via CLI, SDK or API calls

Saved configurations - wanna save the settings we've applied? make a saved configuration! This includes an environment platform configuration, the tier, configuration settings and resource tags.

Configuration files (.ebextensions) are placed in /.ebextension folder and evaluated in alphabetical order and can set environment variables, autoscaling, and custom resources as well. In addition, you can add CloudFormation to the mix as well. There is a elastic Beanstalk specific intrinsic function called "GetOptionSetting" which references a variable from the configuration options in the main

In configuration files you can setup many sections
  Container Commands, leader only commands, Elastic Beanstalk worker environments

Default values



### Docker with EB

single container - one container per instance

multicontainer - different 

dockerfile - describes the images to build with instructions; EB uses the dockerfile in this configuration if it exists in both the dockerfile and dockerrun.aws.json

http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html

dockerrun.aws.json file 
"AWSEBDockerrunVersion": 1 = single container; 2 = multicontainer
image
ports
volumes
logging

.ebextensions
  create a listener is an example task
  map host port to container port

  option_settings:


# Resources

## Qwik Labs

* [Introduction to AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2935) - This is a low cost (1 credit) lab that simply walks through the basic stuff of ElasticBeanStalk.

* [Working with AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2559)

* [Building Scalable Web Applications with AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2597)

## Reading
[AWS Elastic Beanstalk Developers Guide](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)