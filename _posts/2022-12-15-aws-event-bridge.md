---
layout: post
title: "AWS - EventBridge"
description: ""
category: posts
tags: [app-integration, serverless, aws, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

# EventBridge (CloudWatch Events)
EventBridge is a serverless service that uses events to connect application components together, making it easier to build scalable event-driven applications. EventBridge provides a simple and consistent way to ingest, filter, transform, and deliver events so you can build new applications quickly. Access control is done through resource-based or identity-based policies; common to enable accounts to send events to a single account for aggregation.

## Components
- Events - Events take the form of small JSON documents generated when a resource changes state, an API call is delivered via CloudTrail to CloudWatch, OR when your app generates an event. Events (Scheduled scripts) can be used as well. Events can be archived indefinitely or a set period; these archived events can be replayed.

- Rules - code that matches and routes incoming events; all rules that match an event get fired; can process the JSON or pass the entire message

- Targets - Where events are processed including Lambda, Kinesis, and SNS, built-in targets, OR API Destinations.

- Event buses - Default, Partner, custom. 

- Schema registry - built through analysis of events coming down the bus. The registry enables you to know what the data will look like for an event so code can easily be written. Schemas can also be versioned.


## Triage
real-time monitoring not batched monitoring? Use EventBridge instead of CloudTrail
