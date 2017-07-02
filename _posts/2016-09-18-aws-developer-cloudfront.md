---
layout: post
title: "AWS Developer - CloudFront"
description: ""
category: posts
tags: [aws, developercert, cloudfront, solutionsarch]
---
{% include JB/setup %}

## What 

CloudFront uses edge locations and regions as a CDN point of presence. isn't just for ALL the content on a Static website anymore and an Adobe STMP stream distributions. 

custom origins and custom origin rules determine which part of website requests go where

GET, HEAD, OPTIONS can be cached; PUT, POST, PATCH, DELETE are not cached and do not invalidate objects in the cache

Edge Location - Completely different than an availability zone - this is a location near a large population of Internet users that serves as a cache for content from the Origin which is likely some sort of AWS resource (S3 or EC2). Objects are cached for the life of the TTL; you can clear the cache but it costs.

A Distribution is the name for the CDN you create - essentially a collection of Edge Locations. Two types of distributions: RTMP (for a Adobe Flash server???) and Web Distribution.

One gotcha is that CloudFront will wait for first request to resolve before processing the second request.  Uploading via CDN is faster but does not cache. You have to disable the distribution before you can delete it!

## Distributions


### Improving Performance

Increase cache hit percentage is the #1 way to increase CloudFront performance. Increasing min and max TTL helps improve this metric.

costs money to invalidate distribution object


### Web Distributions

A 0 TTL forces CloudFront to check the `If-Modified-Since` header to see if the origin has changed

### Video Distributions

Video streaming is essentially a web cloud front distribution that hosts video oriented files - except for RTMP. There is no [streaming switch](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-hit-ratio.html#cache-hit-ratio-http-streaming) for CloudFront. Types of video oriented distribution options include:

- On-demand streaming - web cloud front distribution

- Smooth Streaming - a web distribution configured with custom origin "Enable Smooth Streaming" enabled; for Microsoft format data

- Progressive Downloads - again use a web distribution and use an HTTP-based dynamic streaming protocol such as Apple HTTP Dynamic Streaming (Apple HDS), Apple HTTP Live Streaming (Apple HLS), Microsoft Smooth Streaming, or MPEG-DASH.  Progressive downloads won't work with signed URLS.

- Live streaming - pretty much the same as progressive downloads except the origin is a WOWZA media server which probably sits on an EC2 instance.

Unlike these file based options RTMP Distributions is an actual streaming of video instead of serving files. Does not support iOS or HTML and only support Adobe Flash.

## Working with Objects

### Invalidation Techniques - 

use route53 CNAME to point new distribution

Dyanamic content ? custom origin; enable forwarding query string; TTL 0; 

TTL = 0 enables a pull through caching mechanism where CloudFront sends a GET request to the origin with an "If-Modified-Since" header.

### Proxying

### Private Content

Security files can be done using signed URLs or using signed Cookies using the Restrict Viewer Access option.

### Configuration 

CloudFront has many, many configuration options including:

ntries but not both.

* Device detection - Redirect or serve different content based on user agent

configure custom SSL Cert - work with Certification SSL?
Generic SSL Cert

#### HTTPS

 If the origin is S3 all requests are made to the distribution will stay the same... request in HTTPS then it will be forwarded via HTTPS. Custom Origins can be configured to use HTTP only or to match viewer, which will match the user agent protocol.

### Cloudfront Reports

[Cloudfront reporting](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/reports.html) uses logs data but does not require additional processing of the logs to get the good reporting sort of stuff. There are reports on caching metrics, popular objects, top referrers, and user agent information as well. 

CloudFront generates metrics to CloudWatch, CloudTrail and HTTP access logs. 

### CloudFront Security

Geographic Restrictions - by default this is disabled; you can either disable (blacklist) or enable (whitelist) but not both

As a protip, don't cache credit card information in CloudFront edge caches. For example, you can configure your origin to include a Cache-Control:no-cache="field-name" header in responses that contain credit card information, such as the last four digits of a credit card number and the card owner's contact information.

Redirect HTTP to HTTPS is a common security feature which can be enable on Cloudfront.



## When

It is going viral baby! = Whole site CDN; push to CloudFront

Control access to section of site or type of media on site = signed cookie

Control access to a URL for a period of time = signed URL



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
