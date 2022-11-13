---
layout: post
title: "AWS - Elastic Container Service"
description: ""
category: posts
tags: [ECS, aws, solutionsarch]
---
{% include JB/setup %}

# ECS
[Amazon Elastic Container Service (ECS)](https://aws.amazon.com/ecs/) is the container management service that supports [Docker](https://aws.amazon.com/docker/) containers on top of clusters of EC2 instances (ECS on EC2) or serverless using the Fargate launch type. 

## Components

Cluster - A cluster is a set of EC2 instances used when the EC2 launch type is selected.

Service - defines how the service gets run including the Launch type, capacity provider (ASG, FARGATE or FARGATE_SPOT, capacity, placement, tags, client tokenm and networking (health checks, load balancers, service linked role).

Task definition - defines an application that is to be deployed across a cluster or serverless using the Fargate launch type. Contains one or more container definitions, memory/CPU, container launch script, and data volumes.

Task - the manifestation of a task definition running on ECS

ECS Service Endpoint - this can manifest as an Interface VPC endpoint in the VPC OR containers can access the public IP via an IGW and requires the instance IAM role to have the `ecs:Poll` permission.

# EKS
Elastic Kubernetes Service

# ECR

# ECS Security

container instance role - the EC2 role

Task role - the task role

service linked role - ECS uses the service-linked role named AWSServiceRoleForECS to enable Amazon ECS to call AWS APIs on your behalf.