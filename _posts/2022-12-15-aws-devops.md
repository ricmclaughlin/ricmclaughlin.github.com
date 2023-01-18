---
layout: post
title: "AWS - Devops"
description: ""
category: posts
tags: [aws, dev-tools, aws-dev-ops-pro, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

There are several different deployment models and tools to implement those models... 

One important way to classify deployments is in-place, where instances are upgraded making for a faster upgrade, or disposable, where instances are treated as immutable and new instances are created and old instances are destroyed which makes it safer, slower, and a bit more expensive. AZ &amp; regional capacity issues also make immutable infrastructure difficult at scale.

Another way to classify deployments is bootstrapping, where code is installed to a running instance at launch, versus pre-baking where the application is loaded on the image. 

# Deployment types
There are numerous different types of deployments including all instances at one time, rolling, immutable environment, and blue/green. 

## All-at-Once
In this scenario, all instances at one time regardless of how many instances there are..
* Downtime: yes; because all instance get the new deployment at the same time
* Failed deployment: downtime; redeploy the last known good configuration
* DNS: no change
* Speed: fast

## Rolling Deployment
In a Rolling Deployment deployment only some instances at a time are upgraded in place. In many scenarios, a minimum number of instances stays in service.
* Downtime: none
* Failed Deployment: Rollback affected instances
* DNS: none
* Speed: longer

## Immutable Environment
An Immutable Environment deployment is similar to a Rolling deployment except with new resources. For instance, two autoscaling groups would be used.
* Downtime: None
* Failed Deployment: Rollback new resources; slow
* DNS: none
* Speed: longer; costs more

## Blue-Green
In a Blue-Green deployment a new identical environment is created then tested and something, perhaps DNS or other setting is changed to send the new environment live. The Blue environment is the live environment; the green environment is new, not-yet-live version. Works best when implemented on loosely coupled applications; doesn't work well when schema is tightly couple to application, deployment aware application using feature flags, or is COTS app that isn't blue/green friendly.
* Downtime: None
* Failed Deployment: The fastest; simply change networking configuration back
* DNS: Yes, sometimes; this can be a disadvantage because of DNS caching
* Speed: longer; costs more

A/B testing or canary testing is a variation where a portion of traffic is sent to the green environment to test it out. A/B might not be for deployment; it might be just for testing. 

### Blue-Green Implementation
* Update DNS Routing - works well for single instance, or an ELB in conjunction with ASG, ECS or EB; DNS changes are problematic; ELB prewarming may be required; the higher costs or AWS service limits might be an issues because of two concurrent online systems during deployment
* Swap ASG behind ELB - startup new ASG with new configuration; canary test green systems; put some blue instances in `StandBy` Don't wanna mess with DNS; very fast rollback with no DNS change; swapping ECS Service behind ELB is a similar idea
* Swap Launch Config in ASG - Update launch config; double ASG Size; put old config in standby; OK? reduce ASG size terminating old configuration first (Barely blue/green); difficult canary analysis -> hard to detect errors with two launch configs in play and schema changes must be independent of app changes (ideally); switching ECS Task definitions is a similar idea
* Swap EB Environments - Create new environment with a new version; Do a "Swap Environment URL"
* Clone OpsWorks Stack - Clone stack; deploy version using `Deploy` command; change DNS

Schema change is a giant problem with blue/green deployment; decoupling the schema change from the code change which can be be implemented as schema change first or schema change last. Schema change first is preferred because rolling back a schema change in production is a high risk manuever. 

Schema change can also be handled by creating a centralized write process either through a queue used for writes during the deployment OR the green environment writes to both databases OR each environment writes to there own DB and uses an async process to synchronize the data.

## Triage
In-place deployments are traditional method of application deployment. Both deployment and rollback cause downtime so it is hard to have time to debug a problem in the production environment with the new version. In-place deployments are generally faster, good or maybe better for session-less apps and ideal for OpsWorks or Puppet. 

Disposable deployment types are generally slower because new resources must be created and provisioned. Tools that have great support for Green/Blue deployments like Elastic Beanstalk, CodeDeploy/ALB, APIGateway, and CloudFormation are better DevOps tool choices in these scenarios.

### Product Strengths and Weaknesses
* EB - fastest and simplest way to deploy code; Beanstalk doesn’t allow as much flexibility for some configurations and deployments and is more suitable for shorter application lifecycles where an environment can be thrown away with each deploy; not good for lots of dependencies with more than apt-get functionality or heavy configuration; can provision RDS but don't want to; only changes to git repos are moved to pro; source generally from S3

* Opsworks - more focused on application management; can't do rolling batch with Opsworks...

* Cloudformation - control; can combine with EB or OpsWorks

### Hybrid Deployment Models
* CloudFormation with Puppet - puppet master holds instruction and definitions; clients connect and follow instructions

* CloudFormation with Chef - better for longer deployments

* CloudFormation with Elastic BeanStalk - less flexible than OpsWorks; better with immutable deployments

### Scenario -> Product
- maintain capacity; Speed not issue = rolling with batch ; Opsworks? Cloudformation
- in-place? Opsworks &amp; CodeDeploy
- Super fast? in-place; all-at-once faster than rolling
- Consistent or lots of deployments? in-place
- No DNS change? all-at-once, rolling, immutable, blue/green with launch config change OR ASG swap
- No downtime? Blue/Green or rolling
- Pre-baked AMI? Cloudformation &amp; OpsWorks
- Canary = Route53 or an ELB; not well supported with Elastic Beanstalk



