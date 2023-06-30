---
layout: post
title: "AWS - Lambda"
description: ""
category: posts
tags: [compute, serverless, aws-solutions-arch-pro, integration]
---
{% include JB/setup %}

# WTF is Lambda?
Lambda is an abstract compute service where all you need to do is upload your code... AWS manages the servers for you. Lambda works well for for event driven &amp; orchestration workloads and as a result of a request via API gateway or an ALB. Lambda has broad language support and the ability to run Lambda Container images.

Lambda functions can be called by specifying a version or alias (qualified ARN) OR without a version (unqualified ARN).

## Serverless Application Model (SAM)
SAM is a CloudFormation extension optimized for serverless application. It's basically macro library that enables serverless applications to use a more concise syntax for common patterns like functions, APIs, and tables.

## Lambda@edge
With Amazon CloudFront, you can write your own code to customize how your CloudFront distributions process HTTP requests and responses instead of using CloudFront functions. Lambda@edge functions can be run when on viewer/origin request and on the viewer/origin response.

## Interaction with other AWS services
- SQS: max messages per batch = 10 
- CloudWatch: can send matched event, part of the matched, or a constant chunk of JSON

## Concurrency
By default 100 units of concurrency are reserved for all functions that don't have a concurrency limit set. There are two types of concurrency available: 
- _Reserved Concurrency_ - creates a pool of requests that can only be used by it's function establishing an upper bound
- _Provisioned Concurrency_ - initializes a requested number of execution environments so that they are prepared to respond 

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
   * CloudWatch Logs &amp; Events (EventBridge)
   * AWS CodeCommit &amp; CodePipeline
   * AWS Config
   * AWS IoT services 

- Process failed async invocations? Dead letter queue items with SQS or SNS
- Configure number of retries? Sync = caller configured; async = twice
- The old version of the function is served? there is a brief window of time, typically less than a minute, when the requests could be served by the older version.
- Lambda to poll queue? Kinesis, DynamoDB (streams), SQS
- Promote new version to `LATEST`? Update alias to new version (using an alias ARN - best practice!)
- Memory, deployment size, timeout ranges? 128Mg-10G, 50 MB compressed deployment (250 uncompressed including layers)
- Change permissions of Lambda? must use CLI/SDK; can't use console
- More than 1000 concurrent executions? Call support; 10K hard limit
- Need traffic shift during deployment? CodeDeploy Linear, Canary or all at once deployment
- Check with traffic before and after deployment? Pre and Post traffic hooks
- `TooManyRequestsException` errors? SQS (non-FIFO because of rate limits) OR Kinesis (and batch data out of API Gateway)
