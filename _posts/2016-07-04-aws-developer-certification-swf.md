---
layout: post
title: "AWS Developer Certification - SWF"
description: ""
category: posts
tags: [aws, developercert, swf]
---
{% include JB/setup %}

AWS Simple Workflow Service is very similiar to SQS/SNS yet serves a different purpose. SWF is distributed, can work with on-premises or in the cloud, can consist of human events and guarantees order of execution. SWF also allows for sync and async processing.

SQS and SWF are both great for distributed services and can scale easily. SQS does not guarantee message order and could have duplicates.

A SWF domain determines the scope of the workflow and can contain. Workflows can not interact with workflows in other domains.

Activity worker - Activity tasks are assigned to activity workers. A server residing outside of an AWS datacenter can perform a worker task.

Decider - A decision task tells the decider when the state of a workflow execution has changed.

