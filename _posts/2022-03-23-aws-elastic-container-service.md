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

## ALB integration
Dynamic port mapping enables running the same application on the same EC2 instance. Dynamic Port Mapping assigns a different port to each task which enables increased resiliency, better utilization, and ability to perform better rolling upgrades.

## Roles

- container instance profile - the EC2 role

- Task IAM role - the task role

- service linked role - ECS uses the service-linked role named `AWSServiceRoleForECS` to enable Amazon ECS to call AWS APIs on your behalf.

## Networking

Three different modes:
- none
- Bridge - uses docker's virtual container-based network
- host - use host network interface
- awsvpc - (default for Fargate) each task has it's own ENI & private address which simplifies SG, monitoring

## Spot Instances

ECS classic - can run cluster on Spot instances managed by ASG

Fargate - run tasks as `FARGATE_SPOT`

## ECS Anywhere

Simple to use: install ECS Container Agent and SSM Agent on instance; specify EXTERNAL launch type; use console/CLI/SDK to create and manage cluster/tasks. This set up requires a stable connection to the AWS region.

# ECR

Repository with private and public container images supporting versioning, image tags, image lifecycle, and vulnerability scanning. It's an AWS service so you need an IAM permission to access it. Supports cross-region and cross-account replication. 

Scanning for vulnerabilities can be configured manually or on push to the repository. Findings fire an EventBridge event and are accessible via the console. Two scanning types: Basic scanning using the Common Vulnerabilities and Exposures (CVE) from the Clair project. Enhanced Scanning uses Amazon Inspector, a product dedicated to ECR (why is this a separate service?).