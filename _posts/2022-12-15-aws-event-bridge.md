---
layout: post
title: "AWS - EventBridge"
description: ""
category: posts
tags: [eventbridge, aws, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

## EventBridge (CloudWatch Events)

EventBridge is a serverless service that uses events to connect application components together, making it easier to build scalable event-driven applications. EventBridge provides a simple and consistent way to ingest, filter, transform, and deliver events so you can build new applications quickly. 

Events - Events take the form of small JSON documents generated when a resource changes state, an API call is delivered via CloudTrail to CloudWatch, OR when your app generates an event. Use Events instead of CloudTrail when you need real-time monitoring not batched monitoring. Events (Scheduled scripts) can be a scheduled as well. Events can be archived indefinitely or a set period; these archived events can be replayed.

Rules - code that matches and routes incoming events; all rules that match an event get fired; can process the JSON or pass the entire message

Targets - Where events are processed including Lambda, Kinesis, SNS or built-in targets

There are three types of event buses: Default, Partner, custom. 

The schema registry is built through analysis of events coming down the bus. The registry enables you to know what the data will look like for an event so code can easily be written. Schemas can also be versioned.

Access control is done through resource based policies; common to enable accounts to send events to a single account for aggregation.
