---
layout: post
title: "AWS - Analytics Services"
description: ""
category: posts
tags: [aws, glue, athena, quicksight vpc, aws-dev-ops-pro]
---
{% include JB/setup %}


# Glue
[Glue](https://aws.amazon.com/glue/) is a serverless data integration service that makes it easier to discover, prepare, move, and integrate data from multiple sources for analytics, machine learning (ML), and application development. There are two parts to Glue: serverless ETL and the Glue Data Catalog which is built by the Glue Data Crawler. The Glue Data Catalog can be used by Athena, Redshift Spectrum, and EMR. 

Glue can convert data into Parquet or ORC format.

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

# QuickSight
[QuickSight](https://aws.amazon.com/quicksight/) is a cloud-scale business intelligence (BI) service that can use to deliver easy-to-understand insights - another way of saying it makes dashboards. But these dashboards are snap-shots of an analysis - the filtering, parameters, controls, and sorts are embedded in the dashboard. There's an in memory computation engine called SPICE if the data is _imported_ into QuickSight. The Enterprise edition has a security feature called Column-Level security.

QuickSight has it's own access control set up - uses IAM for admin rights; Groups and users in the enterprise edition and just users in the standard version. Dashboards, which include access to the underlying data, can be shared with groups and users.
