---
layout: post
title: "Dev Ops Pro - Deployments"
description: ""
category: posts
tags: [aws, devopspro]
---
{% include JB/setup %}

##  In-place vs Disposable

One way to classify deployments is in-place, where instances are upgraded, or disposable, where instances are treated as immutable and new instances are created and old instances are destroyed. 

In-place deployments are generally faster, good or maybe better for session-less apps and ideal for OpsWorks or Puppet. Disposable is generally slower because new instances must be created and tools that have great support for Green/Blue deployments like Elastic Beanstalk and CloudFormation are better DevOps tool choices.


# Deployment types

There are numerous different types of deployments including all instances at one time, rolling, immutable environment, and blue/green. 

## All at Once

In this scenario, all instances at one time regardless of how many instances there are..
    
 * Downtime: yes

 * Failed deployment: downtime

 * DNS: no change

 * Speed: fast


## Rolling Deployment

In a Rolling Deployment deployment only some instances at a time are upgraded in place. In many scenarios, a minimium number of instances stays in service.

* Downtime: none

* Failed Deployment: Rollback affected instances

* DNS: none

* Speed: longer


## Immutable Environment

An Immutable Environment deployment is similiar to a Rolling deployment except with new resources. For instance two autoscaling groups would be used.

* Downtime: None

* Failed Deployment: Rollback new resources

* DNS: none

* Speed: longer; costs more


## Blue-Green

In a Blue-Green deployment a new identical environment is created then tested and DNS is changed to send the new environment live. 

* Downtime: None

* Failed Deployment: None

* DNS: Yes, sometimes; this can be a disadvantage because of DNS caching

* Speed: longer; costs more

A variation of this is when a portion of traffic is sent to the green environment to test it out; this is called A/B testing. A/B might not be for deployment; it might be just for testing.





