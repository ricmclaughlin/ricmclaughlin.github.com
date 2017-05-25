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

## OpsWorks Components

- Stacks - a set of resources you want to manage as a group = dev, stage, pro

- Layers - A blueprint for a set of instances like web servers, app servers or DB servers; each layer has a series of associated recipes 

- Instances - EC2 Instances; can run 7/24, be load or time based

- Apps - code someplace you want to run on instances

## Recipes

Written in ruby based on chef so flow control, and variable and all that sort of stuff are easily implemented. Resources are the building blocks of recipes and take the form of package (like HA proxy), a service (something that needs to be turned on or off) and templates (which are files, typically to configure resources).

Recipes can execute in each of the following lifecycle stages:

* setup - setup new instance before first boot
  
* configure - fired when instance leave or enters online state

* deploy - occurs when an app is deployed 

* undeploy - occurs when an app is deleted from a set of instances

* shutdown - occurs when instance starts to shut down

## Cookbooks

There are tons of [built in recipes](https://github.com/aws/opsworks-cookbooks) and you also have the ability to create custom bookselves. Berkshelf is the package manager for cookbooks.

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

# Hostname Themes

Just when you thought AWS was hurting... there was no soul, I found something good. Hostname Themes!! Bad news? No cheese theme. No Pokemon theme. Scottish Island theme? lame.

## Resources

[OpsWorks Guide](http://docs.aws.amazon.com/opsworks/latest/userguide/welcome.html)

[Getting Started with a Sample Stack](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-intro.html)

[Getting Started with Cookbooks in AWS OpsWorks Stacks](http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-cookbooks.html)

## QwikLab 

[Working with AWS OpsWorks](https://qwiklabs.com/focuses/2868?search=170864)