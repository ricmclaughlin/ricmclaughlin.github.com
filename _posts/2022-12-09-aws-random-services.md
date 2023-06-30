---
layout: post
title: "AWS - Other Services"
description: ""
category: posts
tags: [other, aws-solutions-arch-pro]
---
{% include JB/setup %}

# IoT Core
Enables Internet of Things (IoT) devices to connect to AWS; serverless. MQTT communication occurs over _IoT Topics_, while _IoT rules_ monitor steam flow on the topic and fire IoT _Rules Actions_. It's common to pipe data from Topics into Kinesis Data Firehose.

# Simple Email Service (SES)
Managed email service that handles inbound/outbound emails and provides insights, anti-spam on a reputation dashboard. Supports DomainKeys Identified Mail (DKIM) and Sender Policy Framework (SPF) and shared, dedicated, and customer owned IP. Email can be sent via SMTP, Console, or APIs.

Use _Configuration Sets_ to customize and analyze email send events by sending them to SNS (for immediate feedback) or to Kinesis Data Firehose AND IP pool management to send different emails from different IPs.

# Device Farm
Application testing service across browsers and real mobile devices. Generates videos and logs to document found issues and devs can then remotely log-in to devices for debugging.

# WaveLength
AWS Wavelength embeds AWS compute and storage services within 5G networks, providing mobile edge computing infrastructure for developing, deploying, and scaling ultra-low-latency applications. This enables some services (EC2, EBS, VPC) to be present at the edge so traffic does not leave the communication service providers network; in cases there are cloud resources required for the solution there is a high-bandwidth connection to the parent AWS region. There are no additional charges or service agreements for this service.

# Local Zones
AWS Local Zones places compute, storage, database, and other select AWS resources close to large population and industry centers. Local zones enable you to create a VPC and place services there.

# Outposts
Hybrid cloud using what are basically Snowball edge that brings low-latency, data residency, and easier migration from on-prem to the cloud. Uses S3 Outposts storage class with a SSE-S3 as the default encryption method; use S3 Access points to access the data from the cloud.

# AppSync
Enables a GraphQL interface into one or more backend data sources via Websockets or MQTT (on Websockets). GraphQL Schema resolves interpret the requests, marshall and consolidate the data then transmits it back to the client. Cognito authorization is well integrated using groups in the schema.

# Fault Injection Simulator (FIS)
Chaos engineering service that injects faults into a running system to test self healing. In FIS you generate an experiment template, run the experiment, then monitor the results using EventBridge and CloudWatch.

# X-Ray
AWS X-Ray is a service that collects request data that an application serves, and provides tools that can view, filter, and gain insights into that data to identify issues and opportunities for optimization. For any traced request to your application, detailed information not only about the request and response, but also about calls that the application makes to downstream AWS resources, microservices, databases, and web APIs. EC2, ECS require an agent; Lambda/API Gateway works by default; Beanstalk automatically installs the agent if X-Ray is enabled during set up.

# Personal Health Dashboard
Dashboard to show how AWS outages &amp; maintenance event impact a specific account's resources and enable remediation. This service does NOT return public events from the Service Health Dashboard. This data is also available via the AWS Health API. Integrates with EventBridge.

## Amazon MQ
Amazon MQ is a managed service broker for RabbitMQ and ActiveMQ. Amazon MQ runs on servers, so you have to set up multi-AZ with failover and is not as performant as SQS. Migration from IBM MQ, TIBCO EMS, RabbitMQ, and ApacheMQ to Amazon MQ is totally possible.

## Simple Workflow Service (SWF)
SWF is the OG workflow service on AWS; it's largely been replaced by [Step Functions](/posts/aws-step-functions) &amp; Amazon Managed Workflows for Apache Airflow (MWAA). SWF enables workflows up to a year in length, integrates with on-prem, and allows off line, human tasks.