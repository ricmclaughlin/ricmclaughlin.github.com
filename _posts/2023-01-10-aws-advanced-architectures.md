---
layout: post
title: "AWS - Advanced Architectures"
description: ""
category: posts 
tags: [aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Advanced Architectures
## License tied to MAC/IP address
0. Request ENI
0. Connect License to ENI
0. Store license in bucket
0. Create Lambda (or script) to update IP address in Parameter store/DNS
0. Create bootstrap script to attach the ENI &amp; get script

## UDP traffic
0. Use NLB for load balancing
0. Set up DNS to Elastic IP of NLB
0. Set up NACL to deny non-UDP traffic to NLB

## CloudFront -> ELB, Bucket
0. Enable CF distribution with ELB &amp; Bucket as origin
0. Create origin access identity (OAI) in CF
0. Create bucket policy that requires OAI
0. Create CF behavior that route requests based on path

## Full text search DynamoDB
For real-time-ish:
0. Configure DynamoDB Stream (or Kinesis Stream for DynamoDB) 
0. Use a Lambda to parse the stream to load OpenSearch. 
OR batched-later-ish
0. Load the data via CloudWatch logs using a Subscription Filter 
0. Use a Lambda to load OpenSearch.