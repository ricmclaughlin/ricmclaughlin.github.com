---
layout: post
title: "AWS SysOps - OpsWorks"
description: ""
category: posts
tags: [aws, sysops, opsworks]
---
{% include JB/setup %}

Opsworks is [Chef](https://www.chef.io/chef/) a popular devops tool used by Amazon - which makes sense because Chef & Amazon are both based in Seattle. Opsworks enable you to automate, monitor and maintain deployments.

Currently you can choose from a Chef 12 or Chef 11 Stack.

## OpsWorks Components

Stacks - a set of resources you want to manage as a group = dev, stage, pro

Layers - A blueprint for a set of instances like web servers, app servers or DB servers

- Instances - EC2 Instances

- Apps - code someplace you want to run on instances
 
Recipes
  setup - setup new instance before first boot
  configure - fired when instance leave or enters online state
  deploy - occurs when an app is deployed 
  undeploy - occurs when an app is deleted from a set of instances
  shutdown - occurs when instance starts to shut down

# Hostname Themes
Just when you thought AWS was hurting... there was no soul, I found something good. Hostname Themes!! Bad news? No cheese theme. No Pokemon theme. Scottish Island theme? lame.

## Resources
[OpsWorks Guide](http://docs.aws.amazon.com/opsworks/latest/userguide/welcome.html)

## QwikLab 
[Working with AWS OpsWorks](https://qwiklabs.com/focuses/2868?search=170864)