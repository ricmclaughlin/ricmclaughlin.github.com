---
layout: post
title: "AWS - Lambda"
description: ""
category: posts
tags: [lambda, aws, devops, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# WTF is Lambda?

Lambda is an abstract compute service where all you need to do is upload your code... AWS manages the servers for you. Lambda works well for for event driven &amp; orchestration workloads and as a result of a request via API gateway or an ALB. Lambda has broad language support and the ability to run Lambda Container images.

Lambda functions can be called by specifying a version or alias (qualified ARN) OR without a version (unqualified ARN).

## Serverless Application Model (SAM)

SAM is a CloudFormation extention optimized for serverless application. It's basically macro library that enables serverless applications to use a more consise syntax for common patterns like functions, APIs, and tables.

## Lambda@edge
With Amazon CloudFront, you can write your own code to customize how your CloudFront distributions process HTTP requests and responses instead of using CloudFront functions. Lambda@edge functions can be run when on viewer/origin request and on the viewer/origin response.

## Interaction with other AWS services

- SQS: max messages per batch = 10; 

- CloudWatch: can send matched event, part of the matched, or a constant chunk of JSON

## Networking


## Triage

- Need Internet access? Deploy in private subnet and add NAT 

- Need Internet access with fixed IP? Deploy in private subnet, add NAT, &amp; Elastic IP

- Function termination? out of memory

- Synchronously call? (anything that requires a response to proceed and error handling)
    * ELB
    * Cognito
    * Lex
    * Alexa
    * API Gateway
    * CloudFront
    * Kinesis Firehouse
    * Step Functions
    * S3 Batch Functions
    * SQS
    * SDK/CLI

- Asynchronously call? (anything that architecturally could be modeled as a queue)
   * S3
   * SNS
   * SES
   * CloudFormation
   * CloudWatch Logs & Events (EventBridge)
   * AWS CodeCommit & CodePipeline
   * AWS Config
   * AWS IoT services 

- Process failed async innvocations? dead letter queue items with SQS or SNS

- Configure number of retries? Sync = caller configured; async = twice

- The old version of the function is served? there is a brief window of time, typically less than a minute, when the requests could be served by the older veersion.

- Lambda to poll queue? Kinesis, DynamoDB (streams), SQS

- Promote new version to `LATEST`? Update alias to new version (assuming you are following best practice by using an alias ARN)
 
- Memory, deployment size, timeout ranges? 128Mg-10G, 50 MB compressed deployment (250 uncompress including layers)

- Change permissions of Lambda? must use CLI/SDK; can't use console

- More than 1000 concurrent executions? Call support; 10K hard limit

- need traffic shift during deployment? CodeDeploy Linear, Canary or all at once deployment

- check with traffic before and after deployment? Pre and Post traffic hooks

# API Gateway

Integration sources? Lambda functions (in account or other account), Step Functions, HTTP endpoints (EB), EC2, non-AWS HTTP/S endpoints, or private end-points via VPC links/Direct Connect. 

Caching using cache keys made from headers, query strings & URL; can set size and over-ride cached elements; when resized the cache is flushed.

Access control can be implemented using resource policies, IAM roles/policies, CORS, lambda authorizers, Cognito pools, SSL Certs, and usage plans.

Uses CloudWatch execution logging to capture user requests and response payloads.

- 