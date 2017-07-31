---
layout: post
title: "AWS Solutions Arch - ECS"
description: ""
category: posts
tags: [aws, solutionsarch]
---
{% include JB/setup %}


[Amazon EC2 Container Service (ECS)](https://aws.amazon.com/ecs/) is the container management service that supports [Docker](https://aws.amazon.com/docker/) containers. 

## Components

Docker Image - read only; 

Docker Container - an image plus an application

Layers - each container is full of layers combined on a Union File System

DockerFile - contains Instructions that get run to turn a base image into a Container

Docker Engine - runs the container on the Host OS

Docker Client - the management app

Docker Registry/Docker Hub - contains images; public or private

## Tasks