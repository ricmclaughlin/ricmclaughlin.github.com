---
layout: post
title: "AWS Developer - SQS"
description: ""
category: posts
tags: [aws, developercert, sqs, solutionsarch, solutionsarchpro]
---
{% include JB/setup %}

[Simple Queue Service](https://aws.amazon.com/sqs/) is a queuing product much like the [Tuxedo](http://www.oracle.com/us/products/middleware/cloud-app-foundation/tuxedo/message-queue/overview/index.html) product now owned by Oracle. Together with AWS [Simple Notification Service](http://aws.amazon.com/sns/), SQS enables distributed, fault tolerate applications to be easily created. 

1. Long polling vs short polling - using long polling, the `ReceiveMessage` call issued from the worker of the queue will wait and listen to the queue for as long as 20 seconds before it times out or retrieves a message. To enable long polling increase the `ReceiveMessageWaitTimeSeconds` attribute of the queue to a value greater then 0; a `ReceiveMessageWaitTimeSeconds` attribute of 0 enables short polling.

2. SQS guarantees each message will be delivered *at least* once; the only issues is that the message might be delivered more than once. Message delivery order is not guaranteed either. SQS is NOT FIFO. The max time a SQS message can be retained (`MessageRetentionPeriod`) is 14 days.

3. Each 64kb of SQS message is billed as a message for a max of 256kb per message total. If the message payload is larger than 256kb it is best practice to store the message in ElasticCache, DynamoDB or on S3.

4. `VisibilityTimeout` defines how long a message is INVISIBLE to other workers after being `ReceiveMessage`'d by a worker. It is invisible so the worker who retrieved the message has the opportunity to process the message and remove it from the queue. If the worker is not successfully in processing the message, the `VisibilityTimout` then expires and the message is again available to be accessed by another worker. This ensures that if part of your application fails the message is not lost. Default visibility timeout is 30 seconds and the max is 12 hours and can be changed by the `ChangeMessageVisibility` action.

![visibility timeout](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/Visibility_Timeout.png)

## SQS API Reference Points

### Messaging

| **SQS API (Messaging)**  | **Notes**  |
|:-----------------------------------------|:--------------------------------------------------------|
| [SendMessage](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html) | Delivers a message to the specified queue. |
| [ReceiveMessage](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html) | Retrieves one or more messages, with a maximum limit of 10 messages, from the specified queue. You can set `WaitTimeSeconds` to a number greater than 1 and enable long poll support per request.|
| [DeleteMessage](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_DeleteMessage.html) |Deletes the specified message from the specified queue. |
| [ChangeMessageVisibility  ](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ChangeMessageVisibility.html) | Changes the visibility timeout of a specified message in a queue to a new value. This call restarts visibility in queue but not past 12 hours. |

### Queue

| **SQS API (Queue)**  | **Notes**  |
|:-------------------------------------------|:--------------------------------------------------------|
|  [CreateQueue](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_CreateQueue.html)| Creates a new queue, or returns the URL of an existing one. |
| [DeleteQueue](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_DeleteQueue.html) | Deletes the queue specified by the queue URL, regardless of whether the queue is empty.|
| [GetQueueAttributes  ](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_GetQueueAttributes.html) | Gets attributes for the specified queue.|
|  [SetQueueAttributes  ](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html)| Sets the value of one or more queue attributes.|

## Use Cases

Message priority? Multiple queues; more resources on high priority queue; Add DelaySeconds (Delivery Delay in the Console) to the lower priority queue

Bigger than 64kb? More than one message chunk

bigger than 256kb? Message with pointer to data in DynamoDB or S3

Workflow longer than 14 days? SWF

Decrease SQS cost? long poll

Lots of Visibility timeout? up timeout

Enable long poll? `ReceiveMessageWaitTimeSeconds` > 0 

## Resources

### Reading
[SQS Developer's Guide](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/Welcome.html)

### Video
[SQS and DynamoDB](https://www.youtube.com/watch?v=n9pMxdUbBGs)

