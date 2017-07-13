---
layout: post
title: "AWS - ElasticBeanstalk"
description: ""
category: posts
tags: [aws, developercert, vpc, elasticbeanstalk, devopspro]
---
{% include JB/setup %}

[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) is the way to get started with AWS with a website or backend process. Like Cloud Formation, you don't pay to use the tool BUT you do pay for the resources it provisions. In fact, Beanstalk is CloudFormation... except with a thin layer of goo on top.

EB is appropriate to use when creating quick applications including prototypes that require minimal control and flexibility to power the application. EB is not appropriate when there are lots of additional dependency software and the existing application that does not fit into the EB model. EB supports Web Server Environment Tiers and Working Environments as well with a SQS queue between the tiers. These are simplified environments yet look quite functional for small to medium sized systems.

## EB components

An Application is the logical collection of environments, versions, environment configurations similiar to a folder that serves as the basic concept behind EB. A configuration template is the starting spot for configuring an application.

The components run on platforms - the underlying OS, middleware and server software  You can not change platforms; Linux based platforms use semantic versioning and can be updated; Windows does not and can not be updated. A custom platform can be created using a platform.yaml file and setting the "flavor" attribute.

### Environments 

Elastic Beanstalk supports a webserver or worker environments. On the webserver side, EB supports IIS, Nginx, tomcat, PHP, python, node.js or Ruby. It also includes Docker support. There are two environment types - load balancing and autoscaling and single instance and you can change between the two. A worker environment tier for a web application processes background tasks and does not include a load balancer.

Overall, it is a bad idea to create a RDS instance in Elastic Bean stalk.... a RDS instance can not be used in more than one environment and a clone of an environment does NOT clone the database.

### Versions

Each source code upload is a version and each upload must be a single zip file or WAR file less than 512 with no top level directory. You can include multiple WAR files in a single zip file as well. The word "deployment" is associated with versions. Both updates, which are changes to teh configuration of an environment, and deployments use healthchecks.

Application Version Lifecycle settings controls how many version of the app to keep around. Once enabled (it is an optional setting), the policy can be configured to keep a number of versions around OR to delete after the version is a particular age.

## Deployment Policies

Each deployment has an ID that increments each deployment and instance configuration change. You can ignore healthchecks which determines if the deployment wait until the instance comes up healthy or simply continue... If an app is created from console or eb cli it will default to a rolling deployment but if created from SDK or AWS CLI it will default to an all-at-once deployment.

There are five different [deployment policies](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html). 

- all at once - fast with no DNS change; causes downtime 
  
- rolling - deploy in batches based on number or percentage; upon re-attachment to ELB health

- rolling with additional batch - like rolling; Except that EB adds more new capacity into ASG to seed deployment (not available for Windows platforms)
  
- immutable - no downtime; add ASG with new version to test; easy rollback; doubles number of instances temporarily which may bump up against EC2 region limits and costs money; updates during maintenance window use this method (not available for Windows platforms)

- Blue/Green - no downtime; just like immutable but with a new ELB as well; switch DNS to change from blue to green; Database layer gets replaced to so you will need to run RDS outside of Elastic Beanstalk.

## Environment Configurations

The EB environment has many settings that are evaluated from higher to lower in precendent. Config changes are processed separate from deployments. If you modify the underlying resource between environment changes they get overwritten. The word "update" is associated with configuration change applied to an environment. 

- Direct changes to the environment - via CLI, SDK or API calls

- Saved configurations - wanna save the settings we've applied? make a saved configuration! This includes an environment platform configuration, the tier, configuration settings and resource tags.

- Configuration files (.ebextensions) 

- Default values

Health checks are part of the configuration too. If there is a change to health reporting a full refresh of the instances is likely required... just like you need to restart an instance to change this in the EC2 console.

### .ebextensions 

are placed in /.ebextension folder; YAML formatted with a .config extension. Use cases include creating a listener or mapping a host port to container port.

`.ebextensions` are evaluated in alphabetical order and can set environment variables, autoscaling, and custom resources as well. In configuration files you can setup many sections that are about the envionrment including `option_settings`, `users`, `sources` (where the app is), and the `commands` section. In addition, you can add CloudFormation to the mix as well. 

Some configuration elements like `container_commands` setup the EC2 instance. The `leader_only` directive can be used within the `container_comments` section to run a section only once. 

There is an Elastic Beanstalk specific intrinsic function called "Fn::GetOptionSetting" which references a variable from the configuration options in the main environment. Both `Ref` and `Fn::GetAtt` from CloudFormation also work in .ebextensions.

### Configuration Changes

Configuration changes can be implemented as a rolling update - based on health or delay; if based on health based on health AND enhanced health reporting is enabled every instance must pass.

### Platform Updates

[Managed platform](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environment-platform-update-managed.html#environment-platform-update-managed-window) updates enable minor and patch to during the weekly scheduled maintenance window. Major updates must be done manually.

### Docker with EB

Docker containers are a good option on EB when the application has many funky dependencies which makes packaging it on Docker a good choice. All the config files are stored in the application source bundle. Docker comes in three flavors:

- single container - one container per instance; requires a `dockerfile`; does not require a `Dockerrun.aws.json`

- multicontainer - several containers per instance; during deployment you can't build a custom image, ya gotta build it beforehand; requires a `Dockerrun.aws.json` which contains either how to use the container [setup configuration](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html) using "AWSEBDockerrunVersion": 1 = single container; 2 = multicontainer image OR where the public or private registry is. 

- preconfigured - generic configured container for Java with Glassfish 

`.dockercfg` - includes authentication info for the private docker registry store on S3 in the SAME region. Use the `docker login registry-url` to generate.


## command line setup


## Reading

[AWS Elastic Beanstalk Developers Guide](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)