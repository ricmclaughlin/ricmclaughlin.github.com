---
layout: post
title: "AWS - Elastic Kubernetes Service"
description: ""
category: posts
tags: [compute, serverless aws-solutions-arch-pro]
---
{% include JB/setup %}

# EKS
[Amazon Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/) is the container management service that supports [Docker](https://aws.amazon.com/docker/) on top of clusters of EC2 instances or serverless using the Fargate launch type. 

## Components
Like ECS, EKS is a cluster made up of nodes (EC2 instances). Storage for nodes, which leverages a Container Storage Interface (CSI) driver, requires the specification of the `StorageClass` on the EKS cluster and supports, EBS, EFS (which is the only storage option for Fargate), FSx for Lustre or FSx for NetApp ONTAP.

### Node Types
Managed nodes are part of an ASG managed by EKS. Self-Managed nodes are self-created ASG registered to the EKS cluster; there are specialized EKS AMI including Docker, `kubelet` and AWS IAM Authenticator (which replaces native Kubernetes authN). Both of these modes support On-Demand or Spot Instances. Fargate is also supported.

## EKS Anywhere
Basically takes the Amazon EKS Distro and enables it to run locally with no connection to the Cloud. If you wanted to connect it to AWS you would use the EKS Connector in either a Fully Connected or partially Disconnected mode. If you want no connection to the cloud that works too - use Fully Disconnected mode.
