---
layout: post
title: "AWS - OpsWorks"
description: ""
category: posts 
tags: [aws, aws-services, aws-dev-ops-pro, dev-tools, aws-solutions-arch-pro]
---
{% include JB/setup %}

Opsworks is [Chef](https://www.chef.io/chef/) a popular devops tool used by Amazon - which makes sense because Chef &amp; Amazon are both based in Seattle. Opsworks enable you to automate, monitor, and maintain deployments.

Currently you can choose from a Chef 12 or Chef 11 Stack. There are two main parts to Chef; the Agent that sit on the managed servers and the AWS automation engine that make this all happen.

## Stacks
Stacks are a set of resources you want to logically manage as a group. Restricted to a single OS; config changes apply to new instances; can't change region or AZ after creation; You can add additional resources such as ElasticIPs, Volumes or RDS instances to a stack.

Stacks must be created in an existing VPC because OpsWorks does not have the ability to create one. Using VPC endpoints are painful and require additional permissions to access other the Agent, Asset, Log and DNA buckets on S3 in the associated region from the instance.

An RDS instance can only be associated with one Opsworks stack; a stack clone does not copy the RDS instance

Updates to the stack don't propagate to existing instances. Cloning works great except instances are not started by default and you can't change regions. Stack commands allow you to run commands against the entire stack doing things like execute recipes, run a lifecycle event like `Setup` or `Configure` among other things. 

Some resource can be attached to a stack including EIP, volumes, and RDS instances. You can only associate a volume with a stopped instance.

## Windows Stacks
Of course there are numerous limitations to running windows based stack; a 64 bit US English version of Windows server 2012 based with chef-zero & Chef Version 12.2 is required.

No patch updates; no additional EBS volumes for instances;  versions of windows only; standard metrics only; no CLI support on the instance; no external, outside of AWS, instances

## Layers
A Layer is a blueprint for a set of instances like web servers, app servers or DB servers; each layer has a series of associated recipes based on lifecycle; AWS services can also be layers; an instance must be in a layer; instances can be in more than one layer. If an instance is a member of multiple layers, you must add it to all of them before you start the instance. You cannot add an online instance to a layer.

Layers define which recipes are installed, networking (ELB, Public IP address/EIP), EBS Volumes, security groups, EC2 instance profile and tags. There is also a shutdown timeout associated to layers.

Opsworks can't create an ELB you have to attach an existing ELB to a layer but when you add one it triggers a `Configure` lifecycle event; only one ELB V1 per layer and one layer per ELB; ALB is not supported; can support connection draining if enable on the layer and ELB; Can use custom layers to support more than one ELB connecting to instances

To enable load-balanced instances, the layer must first be setup with a scaling configuration. There are numerous redundant settings for scaling - metrics, batch size, alarm condition time, after scaling ignore metric. Each of these configurations is on a per-layer basis but applies to instances.

### Special "Layers"
RDS - attach an existing RDS instance to the layer; RDS instances can only be in a single stack at a time; removing a RDS server from a layer does NOT delete it; cloning a stack does not copy the RDS instance

ECS - Create an ECS cluster and associated it with a stack; Stacks can have only one cluster and a cluster can only be associated with one stack; Opsworks does NOT manage containers.

## Recipes
Recipes are small chunks of reuseable configuration that are run in Lifecycle Events. Resources are the building blocks of recipes and take the form of package (like HA proxy), a service (something that needs to be turned on or off) and templates (which are files, typically to configure resources).

### Cookbooks
There are tons of [built in recipes](https://github.com/aws/opsworks-cookbooks) and you also have the ability to create custom bookselves. Berkshelf is the package manager for cookbooks.

[BerkShelf](https://docs.chef.io/berkshelf.html) over comes a signifigant short coming in older versions of Chef; only allows one cookbook so community recipes had to be copied into the local repository. This feature was added in the 11.10 version of Chef. Components of berkshelf include the Chef Supermarket, the Berksfile, and the berks package manager. To enable berkshelf enable custom Chef cookbooks on the stack and create a `Berksfile`. The `Berksfile` format is something like:

`ruby
# sets the default cookbook source
source "https://supermarket.chef.io"

cookbook 'apt' # loaded from supermarket 
cookbook 'yeah', git: `git://my-awesome-location` # and could include other options
`
### Databags
[Databags](https://docs.chef.io/data_bags.html) are global JSON objects which can be defined on the stack, layer, app, or instance levels.  Because there is no Chef server they are defined at the command line OR using the CUSTOM_JSON field. References to the databags are in the format of `aws_opsworks_app`, `aws_opsworks_layer`, and `aws_opsworks_*`...

## Instances
EC2 Instances; can run 7/24, be load or time based; must have Internet access; can be part of more than one stack; Instances are stopped when added and can not be editing when running. You can attach volumes - including RAID 0, 1 and 10 - to all instances in a layer.

Instance Auto-Healing happens when the Chef agent loses communication with the automation engine and the engine tries to fix it. EBS backed instances are stopped and started; instance store backed instances are terminated and relaunched. In both scenarios, the `configure` event is run. If the auto-healing process does not recover it gets marked `start_failed`, which requires manual intervention to fix. If autohealed, the OS of the instances will not be changed even if the default OS is has been changed at the stack level. To make this feature work properly, use OpsWorks to stop instances else they get auto healed and only use EBS volume types.

### Lifecycle Events
Recipes can execute in each of the following lifecycle stages:

* `Setup` - setup new instance after boot; if manually triggered the `Setup` command takes the instance out of service 
* `Configure` - fires on all instance when *any instance* leaves or enters online state, change associated elastic IP, or change the associated ELB; this event is commonly used for service discovery and often gets executed many times; when this occurs older `Configure` events are marked **superceded** and are not processed.
* `Deploy` - occurs when an app is deployed 
* `Undeploy` - occurs when an app is deleted from a set of instances
* `Shutdown` - occurs when instance starts to shut down

### Load Based Instances Scaling
Load based scaling is based on setting Up and Down layer averages for CPU, memory or load and ONLY works for load based instances. There is an Up and Down setting section with options for:
* batch size
* threshold exceeded timeframe 
* after scaling ignore timeframe (cooldown) 

Alternately, you can create up to 5 CloudWatch alarms as up/down scaling thresholds.

One gotcha is that load based scaling does not create new instances; it starts and stops instances that you have created so understanding load is a key aspect of Opsworks.

## Applications
Apps are code someplace you want to run on instances that define the name, root document setting, datasource (like RDS), source code repo, environment variables, domain names and SSL config. They can be deployed manually or automatically. 

Deployment is as expected: app parameters, including application-id, are passed into the environment from Databags the the app is installed and a total of 5 version of the application are kept around - the current version and 4 older versions of the app. Blue/green and rolling deployments are not natively supported.

## create-deployment command
The [`create-deployment`](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_DeploymentCommand.html) command can be used to create deployments, roll back to previous versions (sequentally one at a time) and issue stack commands. The commands are JSON formatted and use the `--command` argument. Other commands include:
- `update_custom_cookbooks` - this updates but does not execute cookbooks
- `execute_recipes` - runs individual recipes
- `setup` & `configure` - run lifecycle actions
- `update_dependencies` - linux only

### Deployment
**Kicking off Deploy** - This takes the form of the `deploy` command for apps and "Update Custom Cookbooks" for cookbooks. This is super fast, but updates all instances at the same time, and a failed deployment is difficult to recover from. Rolling back can be done with the last four versions using the `Rollback` command, or you can `undeploy` which will remove the entire application. 

**Rolling Deployments** - by instance groups, proceeds with more instances after success, continues until all instances are complete. To make this work, deregister the instance from ELB, deploy app; IF successful according to monitoring and health checks, re-register with ELB, ELSE rollback; continue until done. Connection draining is a good idea in this scenario.

**Blue/Green** - not natively supported. To make it work, clone the pro stack then deploy; DNS change makes it all work; prewarming the ELB and using a Route53 weighted routing policy might make sense too

## Best Practices
**Updates to master branch** - assuming (lots of stuff), when the master branch of the source repo is updated, Opsworks uses the new version of the source for new instances but does NOT automatically update instances in service. To avoid this:
- use tagging instead of deploying the master branch
- package the source and place it on s3; connect that to the deploy model

**Database Updates** - need to ensure we don't end up with current and last versions are present at the same time and the transition does not impact performance and cause downtime. This situation is complicated by the fact that RDS instances can only be in a single stack at a time; not such an issue because a stack CAN have multiple RDS instances registered to it and there is NO requirement that stacks include a DB at all. There are several approaches to managing this issue:
- upgrade DB - in this approach both versions access the same db; old app is denied access?
- separate DB - in this approach data is sync'd from the old schema to the new schema

**Use [EBS based volumes](http://docs.aws.amazon.com/opsworks/latest/userguide/best-practices-storage.html)** for instances instead of instance storage (no duh)
**[Mix 24/7, time based & load based instances](http://docs.aws.amazon.com/opsworks/latest/userguide/best-practices-autoscale.html)** to achieve max cost performance.
**[Stack level permissions](http://docs.aws.amazon.com/opsworks/latest/userguide/best-practices-permissions.html)** first before getting all wacky and granular.
**[Custom JSON](http://docs.aws.amazon.com/opsworks/latest/userguide/workinglayers-basics-edit.html)**, which is limited to 80KB, enables the ability to pass chunks of well organized arguments to an instance when ever a recipe is run, can be defined at the deployment, layer and stack level. Deployment level Custom JSON overrides layer and stack level settings.

### Hostname Themes
Just when you thought AWS was hurting... there was no soul, I found something good. Hostname Themes!! Bad news? No cheese theme. No Pokemon theme. Scottish Island theme? lame.

## Monitoring
There is a super nice special monitoring control panel that allows metrics by instance, layer or stacks. There are 13 custom metrics for Linux instances.


## Triage
application? Goes in a stack.
More than one type of server config? One layer per type.
AWS resource? custom recipe. (really??)
