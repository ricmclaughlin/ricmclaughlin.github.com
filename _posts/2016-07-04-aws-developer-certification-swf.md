---
layout: post
title: "AWS Developer Certification - SWF"
description: ""
category: posts
tags: [aws, developercert, swf]
---
{% include JB/setup %}

[AWS Simple Workflow Service (SWF)](https://aws.amazon.com/swf/) is very similiar to SQS/SNS yet serves a different purpose. SWF is distributed workflow modeling system, can work with on-premises or in the cloud server processes, can include of human offline events and guarantees order of execution. SWF implements sync and async processing. Think about the entire Amazon.com order process - this is exactly what SWF does. 

## SQS vs SWF
SQS and SWF are both great for distributed services and can scale easily.  SWF can NOT have duplicates, can only be assigned once and uses a guaranteed order. SWF can last as long as one year. SQS does not guarantee message order, could have duplicates and can only last 12 hours. 

## SWF Components

* Domain - A SWF domain determines the scope of the workflow and can contain workflows. A domain is a module or an application.

* Workflows - can not interact with workflows in other domains. 

* Activity worker - Activity tasks are assigned to activity workers. A server residing outside of an AWS datacenter can perform a worker task.

* Decider - A decision task tells the decider when the state of a workflow execution has changed.

# Resources

## Reading
[SWF FAQ](https://aws.amazon.com/swf/faqs/)

## Video
[7 SWF Use Cases in 7 Minutes Each](https://www.youtube.com/watch?v=sXGlQruUrWE)