---
layout: post
title: "AWS - API Gateway"
description: ""
category: posts
tags: [aws-services, serverless, app-integration, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

# API Gateway
Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale. Supports versioning, auth, traffic management, OpenAPI spec, and CORS.

The key limitations of this services are the 29 second timeout and 10 MB max payload size.

API Gateway integrates with Lambda functions (in account or other account), Step Functions, HTTP endpoints (EB), EC2, non-AWS HTTP/S endpoints, or private end-points via VPC links/Direct Connect. 

WebSockets enable stateful real-time applications using the `onConnect`, `sendMessage`, and `onDisconnect`. Stateful connections are supported by the client using and the call back posting to the endpoint URL and appending the `@connections\connectionid` on the query string.

Caching uses cache keys made from headers, query strings &amp; URL. The cache can be encrypted; can set size and over-ride cached elements; when resized the cache is flushed. Default TTL is 300 Seconds (min 0, max 3600). It's possible to reset the cache using a `Cache-Control: max-age=0` call from a client with proper access.

Stages enable you to have stages like dev/test/prod and can split traffic between stages if need to implement a canary deployment.

Three types of endpoints:
- Edge-optimized - routed through CloudFront edge locations
- Regional - for clients in the same region
- Private - within a VPC using an interface VPC endpoint (ENI) using an endpoint policy.

Two types of API (the worst naming of any AWS service!):
- HTTP endpoints - caching, canary deployments, throttling, lots of control (WAF, resource policy, request validation)
- REST endpoints - Lambda/public service endpoint are targets, low latency

## Security
SSL certs and Route53 CNAMES enable encryption in flight. Cross-origin resources sharing (CORS) is supported.

## Access Control
Access control can be implemented using resource policies, IAM roles/policies, Lambda authorizers, Cognito pools, and usage plans. A Gateway resource policy can restrict which VPC, VPC Endpoints, and accounts can connect while an endpoint policy controls JUST what processes can access the endpoint.

### Usage Plans & API Keys
A usage plan specifies who can access one or more deployed API stages and methods—and optionally sets the target request rate to start throttling requests. API keys ID are used as password in conjunction with Lambda authorizers, IAM roles, and Cognito. 

## Logging, Monitoring, and Tracing
Uses CloudWatch execution logging to capture user requests and response payloads with logging level of `ERROR` amd `INFO`. Metrics also go to CloudWatch by stage. _IntegrationLatency_, _Latency_, _CacheHitCount_, _CacheMissCount_ are key metrics to watch. 

Use X-Ray on the API Gateway and Lambda to get full traceability. 

## Deployment (Publish)
Pretty easy: 
0. create a deployment and associate it with a stage which generates a URL like: `https://{restapi-id}.execute-api.{region}.amazonaws.com/{stageName}`
0. Configure the [`CanarySettings`](https://docs.aws.amazon.com/apigateway/latest/api/API_CanarySettings.html) including `percentTraffic` and `deploymentId`
0. Deploy canary
0. Test
0. If successful, update the deployment ID of the stage to the canary deployment. If unsuccessful, delete the canary

## Errors
- 400: Bad request
- 403: access denied, WAF filtered
- 429: Quota exceeded
- 502: Bad gateway
- 503: Unavailable
- 504: Endpoint failure (like a 29 second timeout issue... )


