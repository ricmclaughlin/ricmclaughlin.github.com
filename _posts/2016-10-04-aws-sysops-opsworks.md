---
layout: post
title: "AWS SysOps - OpsWorks"
description: ""
category: posts 
tags: [aws, sysops, opsworks, solutionsarch, devopspro]
---
{% include JB/setup %}

Opsworks is [Chef](https://www.chef.io/chef/) a popular devops tool used by Amazon - which makes sense because Chef & Amazon are both based in Seattle. Opsworks enable you to automate, monitor, and maintain deployments.

Currently you can choose from a Chef 12 or Chef 11 Stack.

## OpsWorks Glossary

### Stacks

Stacks are a set of resources you want to manage as a group = dev, stage, pro; restricted to a single OS; config changes apply to new instances; can't change region or AZ after creation

You can add additional resources such as ElasticIPs, Volumes or RDS instances

### Layers

A Layer is a blueprint for a set of instances like web servers, app servers or DB servers; each layer has a series of associated recipes based on lifecycle

The three types of layers supported are Opsworks layers, ECS stacks, RDS. An RDS instance can only be associated with one Opsworks stack; a stack clone does not copy the RDS instance

### Instances

EC2 Instances; can run 7/24, be load or time based; must have Internet access

To enable load based instances the layer must first be setup to work the scaling configuration. There are numerous redundant settings for scaling - metrics, batch size, alarm condition time, after scaling ignore metric. Each of these configurations is on a per-layer basis but applies to instances.

### Applications

Apps are code someplace you want to run on instances that define the name, root, datasource (like RDS), source, environment variables, domain names and SSL config.

Deployment is as expected: app parameters are passed into the environment from Databags, the app is installed and 4 versions of the app are maintained.

### Recipes

Recipes are small chunks of reuseable configuration that are run in Lifecycle Events. Resources are the building blocks of recipes and take the form of package (like HA proxy), a service (something that needs to be turned on or off) and templates (which are files, typically to configure resources).

### Lifecycle Events

Recipes can execute in each of the following lifecycle stages:

* setup - setup new instance before first boot
  
* configure - fires on all instance when any instance leave or enters online state, change associated elastic IP, change associated ELB; this event is commonly used for service discovery

* deploy - occurs when an app is deployed 

* undeploy - occurs when an app is deleted from a set of instances

* shutdown - occurs when instance starts to shut down

### Cookbooks

There are tons of [built in recipes](https://github.com/aws/opsworks-cookbooks) and you also have the ability to create custom bookselves. Berkshelf is the package manager for cookbooks.

### Databags

[Databags](https://docs.chef.io/data_bags.html) are global JSON objects which can be defined on the stack, layer, app and instance.  Because there is no Chef server they are defined at the commandline OR using the CUSTOM_JSON field.

### BerkShelf

[BerkShelf](https://docs.chef.io/berkshelf.html) over comes a signifigant short coming in older versions of Chef; only allows one cookbook so community recipes had to be copied into the local repository. This feature was added in the 11.10 version of Chef. Components of berkshelf include the Chef Supermarket, the Berksfile, and the berks package manager. To enable berkshelf enable custom Chef cookbooks on the stack and create a `Berksfile`. 

### create-deployment command

The [`create-deployment`](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_DeploymentCommand.html) command can be used to create deployments, roll back to previous versions (sequentally one at a time) and issue stack commands. The commands are JSON formatted. Other commands include:

- `update_custom_cookbooks` - this updates but does not execute cookbooks

- `execute_recipes` - runs individual recipes

- `setup` & `configure` - run lifecycle actions

- `update_dependencies` - linux only

## Strategies and Work-arounds

**Updates to master branch** - assuming (lots of stuff), when the master branch of the source repo is updated, Opsworks uses the new version of the source for new instances but does NOT automatically update instances in service. To avoid this:

- use tagging instead of deploying the master branch

- package the source and place it on s3; connect that to the deploy model

**Manual Deploy** - manual deployment takes the form of the ```deploy``` command for apps and "Update Custom Cookbooks" for cookbooks. This is super fast, but updates all instances at the same time, and a failed deployment is difficult to recover from. Rolling back can be done with the last four versions using the ```Rollback``` command, or you can ```undeploy``` which will remove the entire application. 

**Rolling Deployments** - by instance groups, proceeds with more instances after success, continues until all instances are complete. To make this work, deregister the instance from ELB, deploy app; IF successful according to monitoring and health checks, re-register with ELB, ELSE rollback; continue until done. Connection draining is a good idea in this scenario.

**Blue/Green** - create separate stacks for A &amp; B; might be useful to clone the dev stack then ramp; DNS change makes it all work; prewarming the ELB and using a Route53 weighted routing policy might make sense too

**Database Updates** - need to ensure we don't end up with current and last versions are present at the same time and the transition does not impact performance and cause downtime. This situation is complicated by the fact that RDS instances can only be in a single stack at a time; not such an issue because a stack CAN have multiple RDS instances registerd to it and there is NO requirement that stacks include a DB at all. There are several approaches to managing this issue:

- upgrade DB - in this approach both versions access the same db; old app is denied access?

- separate DB - in this approach data is sync'd from the old schema to the new schema

**Instance Auto-Healing** - when the Chef agent loses communication with the automation engine, the engine tries to fix it. EBS backed instances are stopped and started; instance store backed instances are terminated and relaunched. In both scenarios, the configure event is run. If the auto-healing process does not recover it gets marked `start_failed`, which requires manual intervention to fix. The OS of the instances will not be changed even if the default OS is has been changed at the stack level. Auto-heal is not performance based, only agent->automation engine based.

### Hostname Themes

Just when you thought AWS was hurting... there was no soul, I found something good. Hostname Themes!! Bad news? No cheese theme. No Pokemon theme. Scottish Island theme? lame.

## Technical Components

Agent - the components that sit on the managed servers

automation engine - the AWS engines that make this all happen

## Resources

[OpsWorks Guide](http://docs.aws.amazon.com/opsworks/latest/userguide/welcome.html)

[Getting Started with a Sample Stack](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-intro.html)

[Getting Started with Cookbooks in AWS OpsWorks Stacks](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-cookbooks.html)

## QwikLab 

[Working with AWS OpsWorks](https://qwiklabs.com/focuses/2868?search=170864)