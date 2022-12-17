---
layout: post
title: "AWS - Athena"
description: ""
category: posts
tags: [aws, athena, aws-services, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Athena
[Athena](https://aws.amazon.com/athena/) is a serverless, interactive analytics service built on open-source frameworks, supporting open-table and file formats to query data in S3. Athena provides a simplified, flexible way to analyze petabytes of data where it lives. Great for log analysis... because that data lives in S3.

Amazon Athena uses Presto, an open source, distributed SQL query engine optimized for low latency, interactive data analysis of CSV, JSON, ORC, Avro, or Parquet formatted data. 

Pricing: $5 per TB of data scanned

Performance improvement:
- Use columnar data format (to scan less data) like Parquet or ORC
- Use compressed data
- Use S3 keys to partition data for easier querying (like year=1999/month=01/day=04)
- User large files >128mg to minimize overhead

Athena also does Federated Query which uses Lambda as the _Data Source Connector_ to run the query against relational, non-relational, and custom data sources - even on-prem. 

