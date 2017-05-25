---
layout: post
title: "AWS - ElasticBeanstalk"
description: ""
category: posts
tags: [aws, developercert, vpc, elasticbeanstalk, devopspro]
---
{% include JB/setup %}

[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) is the way to get started with AWS with a website or backend process. Like Cloud Formation, you don't pay to use the tool BUT you do pay for the resources it provisions. In fact, beanstalk is CloudFormation... except with a thin layer of goo on top.

Elastic Beanstalk supports a webserver or worker environments. On the webserver side it supports IIS, Nginx, tomcat, PHP, python, node.js or Ruby. It also includes Docker support.

EB supports Web Server Environment Tiers and Working Environments as well with a SQS queue between the tiers. These are simplified environments yet look quite functional for small to medium sized systems.

EB is appropriate to use when creating quick applications including prototypes that require minimal control and flexibility to power the application. EB is not appropriate when there are lots of additional dependency software and the existing application that does not fit into the EB model.

## EB components

An Application is the logical collection of environments, versions, environment configurations similiar to a folder that serves as the basic concept behind EB. A configuration template is the starting spot for configuring an application.

updates vs deployments - the word "update" is associated with configuration change applied to an environment. The word "deployment" is associated with versions. Both updates and deployments use healthchecks.

platforms - the underlying OS, middleware and server software  You can not change platforms; Linux based platforms use symantic versioning and can be updated; Windows does not and can not be updated. A custom platform can be created using a platform.yaml file and setting the "flavor" attribute.

### Versions

Each source code upload is a version and each upload must be a single zip file or WAR file less than 512 with no top level directory. You can include multiple WAR files in a single zip file as well. 

Tons of different app platforms are supported including java (gradle)

Application Version Lifecycle settings controls how many version of the app to keep around. Once enabled (it is an optional setting), the policy can be configured to keep a number of versions around OR to delete after the version is a particular age and can also 

### Environment Types 

There are two environment types - load balancing and autoscaling and single instance. You can change between the two. A worker environment tier for a web application processes background tasks and does not include a load balancer.

### Deployment Policies

There are five different [deployment policies](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html).

- all at once - fast with no DNS change; causes downtime 
  
- rolling - deploy in batches based on number or percentage; upon re-attachment to ELB health

- rolling with additional batch - like rolling; Except that EB adds more new capacity into ASG to seed deployment (not available for Windows platforms)
  
- immutable - no downtime; add ASG with new version to test; easy rollback; doubles number of instances temporarily which may bump up against EC2 region limits and costs money; updates during maintenance window use this method (not available for Windows platforms)

- Blue/Green - no downtime; just like immutable but with a new ELB as well; switch DNS to change from blue to green; Database layer gets replaced too so you will need to run RDS outside of Elastic Beanstalk.

Each deployment has an ID that increments each deployment and instance configuration change. You can ignore healthchecks which determines if the deployment wait until the instance comes up healthy or simply continue... If an app is created from console or eb cli it will default to a rolling deployment but if created from SDK or AWS CLI it will default to an all-at-once deployment

## EB Configuration Settings

The EB environment has many settings that are evaluated from higher to lower in precendent. Config changes are processed separate from deployments. If you modify the underlying resource between environment changes they get overwritten.

- Direct changes to the environment - via CLI, SDK or API calls

- Saved configurations - wanna save the settings we've applied? make a saved configuration! This includes an environment platform configuration, the tier, configuration settings and resource tags.

- Configuration files (.ebextensions) are placed in /.ebextension folder 

- Default values

### .ebextensions 
and evaluated in alphabetical order and can set environment variables, autoscaling, and custom resources as well. In addition, you can add CloudFormation to the mix as well. There is a elastic Beanstalk specific intrinsic function called "GetOptionSetting" which references a variable from the configuration options in the main
.ebextensions
  create a listener is an example task
  map host port to container port

  option_settings:

In configuration files you can setup many sections
  Container Commands, leader only commands, Elastic Beanstalk worker environments


### Configuration Changes


rolling update - based on health or delay

if based on health based on health AND enhanced health reporting is enabled every instance must pass

### Platform Updates

[Managed platform](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environment-platform-update-managed.html#environment-platform-update-managed-window) updates enable minor and patch to during the weekly scheduled maintenance window.

Major updates must be done manually.


### Docker with EB

Docker containers are a good option on EB when the application has many funky dependencies which makes packaging it on Docker a good choice. Docker comes in three flavors:

- single container - one container per instance; does not require a ```Dockerrun.aws.json```

- multicontainer - several containers per instance; during deployment you can't build a custom image, ya gotta build it beforehand; requires a ```Dockerrun.aws.json```

- preconfigured - generic configured container for Java with Glassfish 

```Dockerfile``` - describes the images to build with instructions; EB uses the dockerfile in this configuration if it exists in both the dockerfile and dockerrun.aws.json

```Dockerrun.aws.json``` file contains multicontainer [setup configuration](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html) and has 
"AWSEBDockerrunVersion": 1 = single container; 2 = multicontainer image

# Resources

## Qwik Labs

* [Introduction to AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2935) - This is a low cost (1 credit) lab that simply walks through the basic stuff of ElasticBeanStalk.

* [Working with AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2559)

* [Building Scalable Web Applications with AWS Elastic Beanstalk](https://qwiklabs.com/focuses/2597)

## Reading
[AWS Elastic Beanstalk Developers Guide](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)