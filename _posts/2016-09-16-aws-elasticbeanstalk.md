---
layout: post
title: "AWS - Elastic Beanstalk"
description: ""
category: posts
tags: [aws, devops, elastic-beanstalk, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) is the way to get started with AWS with a website or backend process. Like Cloud Formation, you don't pay to use the tool BUT you do pay for the resources it provisions. In fact, Beanstalk is CloudFormation... except with a thin layer of goo on top.

EB is appropriate to use when creating quick applications including prototypes that require minimal control and flexibility to power the application. EB is not appropriate when there are lots of additional dependency software and the existing application that does not fit into the EB model. EB supports Web Server Environments and Worker Environments as well with a SQS queue between the tiers. These are simplified environments yet look quite functional for small to medium sized systems and up to 75 applications and 1,000 application versions can be created. 

# Applications

An Application is the logical collection of environments, versions, environment configurations similiar to a folder that serves as the basic concept behind EB. A configuration template is the starting spot for configuring an application. User applications are not in a VPC by default.

## Versions

Versions are distinct releases into an environment and is associated with a Version Label. Within the environment you can set configure four different types of application deployments using [deployment policies](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html). 

- all at once - fast with no DNS change; causes downtime 
  
- rolling - deploy in batches based on number or percentage; upon re-attachment to ELB health

- rolling with additional batch - like rolling; Except that EB adds more new capacity into ASG to seed deployment (not available for Windows platforms)
  
- immutable - no downtime; add ASG with new version to test; easy rollback; doubles number of instances temporarily which may bump up against EC2 region limits and costs money; updates during maintenance window use this method (not available for Windows platforms)

## Environments 

Elastic Beanstalk supports a webserver or worker environments. On the webserver side, EB supports IIS, Nginx, tomcat, PHP, python, node.js or Ruby plaforms. It also includes Docker support. There are two environment types - load balancing and autoscaling and single instance (using an EIP) and you can change between the two. A worker environment tier for a web application processes background tasks and does not include a load balancer.

Environment variables can be set - which is a lot like heroku.

Overall, it is a bad idea to create a RDS instance in an Elastic Beanstalk environment.... a RDS instance can not be used in more than one environment and a clone of an environment does NOT clone the database.

### Configuration Updates

The EB environment has many settings that can be changed after the environment has been setup. The word "update" is associated with configuration change applied to an environment. Config updates are processed separate from deployments and probably require replacing/restarting the instances. 

If there is a change to health reporting, from standard to enhanced, a full refresh of the instances is likely required... just like you need to restart an instance to change this in the EC2 console. 

If you modify the underlying resource between environment changes they get overwritten. Changing from a single instance to load balancing replaces all your current instances. 

Configuration updates can be implemented several different ways. If an app is created from console or eb cli it will default to a rolling deployment but if created from SDK or AWS CLI it will default to an all-at-once deployment. Both rolling updates can set a minimum batch size and number of instances in service. Types of configuration updates include:

- [Rolling update based on health](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rollingupdates.html?icmpid=docs_elasticbeanstalk_console) - based on health; if based on health based on health AND enhanced health reporting is enabled every instance must pass. Health checks are part of the configuration rollout too. You can ignore healthchecks which determines if the deployment wait until the instance comes up healthy or simply continue... 

- [Rolling update based on time](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rollingupdates.html?icmpid=docs_elasticbeanstalk_console) - you can set the pause time to wait between batches 

- Immutable - Immutable deployments perform an immutable update to launch a full set of new instances running the new version of the application in a separate Auto Scaling group alongside the instances running the old version. Immutable deployments can prevent issues caused by partially completed rolling deployments. If the new instances don't pass health checks, Elastic Beanstalk terminates them, leaving the original instances untouched.
 
Because there are many different ways of configuring EB, these different types of ways are evaluated from higher to lower in precendent. The order of precendent is as follows:

- Direct changes to the environment - via CLI, SDK or API calls

- Saved configurations - wanna save the settings we've applied? make a saved configuration! This includes an environment platform configuration, the tier, configuration settings and resource tags.

- Configuration files (.ebextensions) 

- Default values

### .ebextensions 

[ebextensions](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html) are basically repackaged Cloudformation. 

.ebextension are placed in /.ebextension folder; YAML formatted with a .config extension. Use cases include creating a listener or mapping a host port to container port or even setting up a custom resource just like with CloudFormation.

`.ebextensions` are evaluated in alphabetical order and can set environment variables, autoscaling, and custom resources as well. In configuration files you can setup many sections that are about the envionrment including `option_settings`, `users`, `sources` (where the app is), and the `commands` section. In addition, you can add CloudFormation to the mix as well. 

Some configuration elements like `container_commands` setup the EC2 instance after the servers are setup and source has been extracted but before deployment. The `leader_only` directive can be used within the `container_comments` section to run a section only once. This is the eb version of `cfn-init` pretty much.

There is an Elastic Beanstalk specific intrinsic function called `Fn::GetOptionSetting` which references a variable from the configuration options in the main environment. Both `Ref` and `Fn::GetAtt` from CloudFormation also work in .ebextensions.

### Blue/Green Deployments

Swap Environment URLs - no downtime; just like immutable updates but these happen outside the context of an environment because you are switching between environments. This changes the DNS to switch from blue to green; Database layer gets replaced to so you will need to run RDS outside of Elastic Beanstalk. To accomplish this, clone the current environment, deploy the new app version to the cloned environment, test, then do a `Switch environment URLs`

### Platforms

The environment components run on platforms - the underlying OS, middleware and server software  You can not change platforms; Linux based platforms use semantic versioning and can be updated; Windows does not and can not be updated. A custom platform can be created using a platform.yaml file and setting the "flavor" attribute.

Opt-in to [managed platform updates](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environment-platform-update-managed.html#environment-platform-update-managed-window) which enable minor and patch updates during the weekly scheduled maintenance window using an immutable deployment mechanism. Major updates must be done manually.

### Docker with EB

Docker containers are a good option on EB when the application has many funky dependencies which makes packaging it on Docker a good choice. All the config files are stored in the application source bundle. Docker comes in three flavors:

- Preconfigured - generic configured container for Java Glassfish/JavaSE/Tomcat, IIS, node, php, python, ruby; does not require a `Dockerfile`;

- Single container - one container per instance; requires a `Dockerfile`; does not require a `Dockerrun.aws.json`

- Multicontainer - several containers per instance; during deployment you can't build a custom image, ya gotta build it beforehand; requires a `Dockerrun.aws.json` which contains either how to use the container [setup configuration](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html) using "AWSEBDockerrunVersion": 1 = single container; 2 = multicontainer image OR where the public or private registry is. A `.dockercfg` file includes authentication info for the private docker registry store on S3 in the SAME region. Use the `docker login registry-url` to generate.


