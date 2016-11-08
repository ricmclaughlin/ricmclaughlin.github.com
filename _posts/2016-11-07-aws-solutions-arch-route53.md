---
layout: post
title: "AWS Solutions Arch - Route53"
description: ""
category: posts
tags: [aws, solutionsarch]
---
{% include JB/setup %}

[Route 53](https://aws.amazon.com/route53/) earns its name because DNS runs on port 53 and it is the DNS service for AWS.

Route53 Supports:

- MX records - Mail

- CNAME - cannonical names - CNAME can't be used for a naked domain (zone apex)

- Alias records - just like a CNAME record; An alias record is the elastic IP of the DNS world; no cost

- A record - points to an IP address

## Routing Policies
Simple - Default; only one resource; no intelligences

Weighted - percentages between 2 or targets

Latency - which region will offer THEM the fastest response time

Failover - active/passive setup; when a site goes does it redirects to the passive

Geolocation - pick which state/country routes to a particular location

### Use Cases
Minimize costs? CNAME = $; Alias = free

naked Domain? = A record or Alias record

Zone Apex? = A record or Alias record

A or B site testing? = Weighted Routing

## Resources

[Route 53](https://aws.amazon.com/route53/)