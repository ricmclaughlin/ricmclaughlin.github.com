---
layout: post
title: "AWS - API Gateway"
description: ""
category: posts
tags: [api-gateway, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

# API Gateway
Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale. Supports versioning, auth, traffic management, OpenAPI spec, CORS.

The key limitations of this services are the 29 second timeout and 10 MB max payload size.

API Gateway integrates with Lambda functions (in account or other account), Step Functions, HTTP endpoints (EB), EC2, non-AWS HTTP/S endpoints, or private end-points via VPC links/Direct Connect. 

WebSockets enable stateful real-time applications using the `onConnect`, `sendMessage`, and `onDisconnect`. Stateful connections are supported by the client using the API Gateway and the call back posting to the endpoint URL and appending the `@connections\connectionid` on the query string.

Caching using cache keys made from headers, query strings & URL. The cache can be encrypted. can set size and over-ride cached elements; when resized the cache is flushed. Default TTL is 300 Seconds (min 0, max 3600). It's possible to reset the cache using a `Cache-Control: max-age=0` call from a client with proper access.

Stages enable you to have stages like, dev/test/prod and can split traffic between stages if need to implement a canary deployment.

Three types of endpoints:
- Edge-optimized - routed through CloudFront edge locations
- Regional - for clients in the same region
- Private - within a VPC using an interface VPC endpoint (ENI) using an endpoint policy.

## Security
SSL certs and Route53 CNAMES enable encryption in flight.

Access control can be implemented using resource policies, IAM roles/policies, Lambda authorizers, Cognito pools, and usage plans. A Gateway resource policy can restrict which VPC, VPC Endpoints, and accounts can connect while an endpoint policy controls JUST what processes can access the endpoint.

Cross-origin resources sharing (CORS) is supported.

### Usage Plans & API Keys
Usage plans define access control, and amount of usage while API keys ID clients and are used to meter access and set throttling and quota limits.

## Logging, Monitoring, and Tracing

Uses CloudWatch execution logging to capture user requests and response payloads with logging level of `ERROR` amd `INFO`.

Metrics also go to CloudWatch by stage. _IntegrationLatency_, _Latency_, _CacheHitCount_, _CacheMissCount_ are key metrics to watch. 

Use X-Ray on the API Gateway and Lambda to get full tracability. 

## Errors
- 400: Bad request
- 403: access denied, WAF filtered
- 429: Quota exceeded
- 502: Bad gateway
- 503: Unavailable
- 504: Endpoint failure (like a 29 second timeout issue. )


