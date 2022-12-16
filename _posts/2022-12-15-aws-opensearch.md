---
layout: post
title: "AWS - OpenSearch"
description: ""
category: posts
tags: [opensearch, aws, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

AWS [OpenSearch](https://aws.amazon.com/documentation/opensearch-service/) is for interactive log analytics, real-time application monitoring, website search. OpenSearch is an open source, distributed search and analytics suite forked from Elasticsearch. OpenSearch Dashboards is a fork of Kibana. Like the ELK stack, OpenSearch is great for log analysis, real time application monitoring, full text search, clickstream analysis, and indexing. OpenSearch is not a serverless offering. Use OpenSearch, OpenSearch Dashboards, and Logstash Agent for ELK stack use cases.

## Use Cases

Full text search DynamoDB? Use a Lambda to parse the DynamoDB Stream (or Kinesis Stream for DynamoDB) to load OpenSearch. Could also load the data via CloudWatch logs using a Subscription Filter then use a Lambda to load OpenSearch.