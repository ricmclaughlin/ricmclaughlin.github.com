---
layout: post
title: "AWS - Step Functions"
description: ""
category: posts
tags: [stepfunctions, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

[Step Functions](hhttps://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) is a serverless orchestration service that integrates Lambda functions and other AWS services to build  workflows modeled as a state machine in JSON.

The max execution time is 1 year with the ability to implement human interaction and approval but does NOT integrate with Mechanical Turk.

Step Function has several optimized integrations with Lambda, Batch, ECS, DynamoDB, SNS/SQS, EMA/Glue/SageMaker, and the SDK and can be invoked using the `StartExcution` call (SDK, API, Lambda), API Gateway, EventBridge, CodePipeline and StepFunctions.

## Task types
There are X types of Tasks:
0. Lambda tasks - invoke a function
0. Activity task - set up an activity worker then distribute a task via call back once active
0. Service task - connection to a supported AWS service
0. Wait task - wait for a duration or until a timestamp.

## Workflow types
There are two types of workflows:

|                            | Standard             | Express                      |
|----------------------------|----------------------|------------------------------|
| Max duration               | 1 year               | 5 minutes                    |
| Execution start per second | 2000                 | >100k                        |
| Transition rate per second | 4000                 | nearly unlimited             |
| Price                      | per state transition | executions, duration, memory |
| Execution Semantics        | exactly once         | at least once                |

Express workflows can be synchronous or asynchronous. 

## Error handling
You can enable error handling, retries (retry interval, MaxAttempts, backoffRate) and add alerting to State Machines. 