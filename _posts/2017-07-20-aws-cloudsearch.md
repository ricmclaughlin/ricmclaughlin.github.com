---
layout: post
title: "AWS - CloudSearch"
description: ""
category: posts
tags: [aws, aws-dev-ops-pro, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

AWS [CloudSearch](https://aws.amazon.com/documentation/cloudsearch/) is the hosted, as-a-service, version of [Apache SOLR](http://lucene.apache.org/solr/) which is built on Apache Lucene.

## Overall 

CloudSearch offers many types of search including full text, boolean, prefix and range. In addition, you can use 
term boosting, faceting, highlighting, and enable autocomplete as well. Normal types of files (html, pdf, MS document) can be searched as can DynamoDB tables.

What is a bit different from other search offering is the data load process. Instead of the system indexing data in a series of paths, data is uploaded to a search domain location defined by CloudSearch then indexed.

Integration with IAM - You control access to the Amazon CloudSearch configuration service APIs and the domain services, which control the use of the domain, APIs independently.

Scaling is automatic based on data and search traffic but can manually scale out as well. Multi-AZ is available as well.

## Setup

1. Create domain

2. Enable access using access policy (private by default)

3. Set Instance type, desired replica count (for stuff bigger than `search.m3.2xlarge`) and partition count

4. Upload content in batches of less than 5 mb

### Setup Indexing

Use the `aws cloudsearch define-index-field` to manually setup index field or `cs-configure-from-batches` to automatically setup index fields.

### Content Update

The model is to upload data into CloudSearch, so if data changes it must be resubmitted to CloudSearch. A document batch is a collection of add and delete operations that represent the documents you want to add, update, or delete from your domain. Batches can be described in either JSON or XML... Maximize batches to get update performance and the max size for a batch is 5 mb... 

## Use

Amazon CloudSearch configuration service APIs and the domain services APIs are independently packaged. 

As normal, when accessing via the CLI or an SDK requests are signed and this saves back and forth authentication traffic.

Filtering is efficient and does not contribute to ranking. 

There is no way to easily delete all the documents in a domain and must be re-indexed to scale down. 

### Scaling

Clusters start up on `search.m3.small` instances and scale up for increased load, speed requests, increased size and improving fault tolerance. Multi-AZ increases fault tolerance but doubles the cost.

Use manual scaling for data load and query spikes and realize that setting that `update-scaling-parameters` sets the baseline.

## ElasticSearch vs CloudSearch

| Feature | ElasticSearch | CloudSearch  |
|---------------|---------------|-------|
| Underlying | Lucene/Elastic Search | SOLR |
| Interface | HTTP endpoint | SDK/CLI |
| HA | Single AZ | Multi AZ |
| Update | incremental-pull | batch-push |

## Trouble Shooting

- `504` or `507` errors will occur if the batch size is at a rate too high or too large; Use the CLI for batches bigger than 5 mb.

- `507` errors can also be a general service overload condition; scale out manually

- `409` errors are generally service resource limits; contact AWS

- Reduce hit size by querying after 2 characters in the UI, use stopwords list
