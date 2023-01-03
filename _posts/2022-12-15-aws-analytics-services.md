---
layout: post
title: "AWS - Analytics Services"
description: ""
category: posts
tags: [aws, analytics, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Glue
[Glue](https://aws.amazon.com/glue/) is a serverless data integration service that makes it easier to discover, prepare, move, and integrate data from multiple sources for analytics, machine learning (ML), and application development. There are three main parts to Glue: a serverless ETL function, the Glue Data Catalog, and Glue Data Crawler which builds the Data Catalog. The Glue Data Catalog can be used by Athena, Redshift Spectrum, and EMR. 

Glue can convert data into Parquet or ORC format.

# QuickSight
[QuickSight](https://aws.amazon.com/quicksight/) is a cloud-scale business intelligence (BI) service that can use to deliver easy-to-understand insights - another way of saying it makes dashboards. But these dashboards are snap-shots of an analysis - the filtering, parameters, controls, and sorts are embedded in the dashboard. There's an in memory computation engine called SPICE if the data is _imported_ into QuickSight. The Enterprise edition has a security feature called Column-Level security.

QuickSight has it's own access control set up - uses IAM for admin rights; Groups and users in the enterprise edition and just users in the standard version. Dashboards, which include access to the underlying data, can be shared with groups and users.
