---
layout: post
title: "AWS Developer Certification - SQS"
description: ""
category: posts
tags: [AWS, developercert, sqs]
---
{% include JB/setup %}

[Simple Queue Service](https://aws.amazon.com/sqs/) is a queuing product much like the [Tuxedo](http://www.oracle.com/us/products/middleware/cloud-app-foundation/tuxedo/message-queue/overview/index.html) product now owned by Oracle. Together with AWS [Simple Notification Service](http://aws.amazon.com/sns/) SQS enables distributed, fault tolerate applications to be easily created.  

1. long polling vs short polling - using long polling the pulling process will wait and listen to the queue for as long as 20 seconds before it times out or retrieves a message.  Default timeout for visibility queue is 30 seconds.

2. Enabling long polling - Increase the ReceiveMessageWaitTimeSeconds attribute of the queue to a value greater then 0 then long polling is enabled; a ReceiveMessageWaitTimeSeconds attribute of 0 enables short polling.

2. SQS guarantees each message will be delivered *at least* once; the only issues is that the mesage might be delivered more than once. Message delivery order is not guaranteed either.

3. Each 64kb of SQS message is billed as a message for a max of 256kb per message total. If the message payload is larger than 256kb it is best practice to store the message in ElasticCache, DynamoDB or on S3.

VisibilityTimeout defines how long a message is INVISIBLE to other workers after being accessed by a worker. It is invisible so the worker who retrieved the message has the opportunity to process the message and remove it from the queue. If the worker is not successfully in processing the message, the VisibilityTimout then expires and the message is again available to be accessed by another worker. This ensures that if part of your application fails the message is not lost. 

What is the answer to: Default timeout for visibility queue is ____ seconds.

