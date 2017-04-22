---
layout: post
title: "Dev Ops Pro - Deployments"
description: ""
category: posts
tags: [aws, devopspro]
---
{% include JB/setup %}

# Deployment types
There are numerous different types of deployments including

## All at Once
all instances at one time
    
    Downtime: yes
    Failed deployment: downtime
    DNS: no change
    Speed: fast

## Rolling Deployment
some instances at a time

    Downtime: none
    Failed Deployment: Rollback affected instances
    DNS: none
    Speed: longer

## Blue-Green
spin up new identical environment

    Downtime: None
    Failed Deployment: None
    DNS: Yes, sometimes
    Speed: longer; costs more

## Immutable Environment
Rolling deployment with new resources; two autoscaling groups

    Downtime: None
    Failed Deployment: Rollback new resources
    DNS: none
    Speed: longer; costs more
##  In-place vs Disposable

### In-place 
Upgrade existing instance

    Generally faster
    better for session-less apps
    OpsWorks and Puppet can be used

### Disposable
create new instance

    Better with immutable or Green/Blue
    Elastic Beanstalk and CloudFormation are better; 