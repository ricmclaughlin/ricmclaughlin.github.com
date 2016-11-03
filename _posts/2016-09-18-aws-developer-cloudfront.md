---
layout: post
title: "AWS Developer - CloudFront"
description: ""
category: posts
tags: [aws, developercert, cloudfront]
---
{% include JB/setup %}

## What 
CloudFront isn't just for ALL the content on a Static website anymore and an Adobe STMP stream distributions. 

configure custom SSL Cert - work with Certification SSL?

custom origins and custom origin rules determine which part of website requests go where

GET, HEAD, OPTIONS can be cached
Edge Location - Completely different than an availability zone - this is a location near a large population of Internet users that serves as a cache for content from the Origin which is likely some sort of AWS resource (S3 or EC2). Objects are cached for the life of the TTL; you can clear the cache but it costs.

A Distribution is the name for the CDN you create - essentially a collection of Edge Locations. Two types of distributions: RTMP (for a Adobe Flash server???) and Web Distribution.

CloudFront has many, many configuration options including:

* Restrict Viewer Access - using signed URLs or Signed Cookies

* Geographic Restrictions - by default this is disabled; you can either disable (blacklist) or enable (whitelist) countries but not both.

* Device detection - Redirect or serve different content based on user agent



Generic SSL Cert

use route53 CNAME to point new distribution

Dyanamic content ? custom origin; enable forwarding query string; TTL 0; 

TTL = 0 enables a pull through caching mechanism where CloudFront sends a GET request to the origin with an "If-Modified-Since" header. 

### Video Streaming

on demand streaming - web cloud front distribution

Smooth - a web distribution with custom origin "Enable Smooth Streaming" enabled

Progressive Downloads - again use a web distribution and use an HTTP-based dynamic streaming protocol such as Apple HTTP Dynamic Streaming (Apple HDS), Apple HTTP Live Streaming (Apple HLS), Microsoft Smooth Streaming, or MPEG-DASH. Same with a WOWZA media server.

There is no [streaming switch](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-hit-ratio.html#cache-hit-ratio-http-streaming) for CloudFront.

## Operations

CloudFront generates metrics to CloudWatch, CloudTrail and HTTP access logs. 

Security
  signed URLs, signed Cookies
  don't cache credit card information in CloudFront edge caches. For example, you can configure your origin to include a Cache-Control:no-cache="field-name" header in responses that contain credit card information, such as the last four digits of a credit card number and the card owner's contact information.

will wait for first request to resolve before processing the second request

It is required to disable the distribution before you can delete it!

costs money to invalidate distribution object

## When

It is going viral baby! = Whole site CDN

Control access to section of site or type of media on site = signed cookie

Control access to a URL for a period of time = signed URL

Redirect HTTP to HTTPS



Increase performance of site?
  push content to cloudfront
  Increase TTL

increase streaming performance

# Resources

## Qwik Labs
[Introduction to Amazon CloudFront](https://qwiklabs.com/focuses/2362)

## Reading
[Serving Private Content through CloudFront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/PrivateContent.html)

[CloudFront Developer Guide](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

[Increase cache hits](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-hit-ratio.html)
