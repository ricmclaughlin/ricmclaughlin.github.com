---
layout: post
title: "AWS Developer - CloudFront"
description: ""
category: posts
tags: [aws, developercert, cloudfront]
---
{% include JB/setup %}

Edge Location - Completely different than an availability zone - this is a location near a large population of Internet users that serves as a cache for content from the Origin which is likely some sort of AWS resource (S3 or EC2). Objects are cached for the life of the TTL; you can clear the cache but it costs.

A Distribution is the name for the CDN you create - essentially a collection of Edge Locations. Two types of distributions: RTMP (for a Adobe Flash server???) and Web Distribution.

CloudFront has many, many configuration options including:

* Restrict Viewer Access - using signed URLs or Signed Cookies

* Geographic Restrictions - by default this is disabled; you can either disable (blacklist) or enable (whitelist) countries but not both.

It is required to disable the distribution before you can delete it!

# Resources

## Qwik Labs
[Introduction to Amazon CloudFront](https://qwiklabs.com/focuses/2362)

## Reading
[Serving Private Content through CloudFront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/PrivateContent.html)

[CloudFront Developer Guide](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

## Video
[7 SWF Use Cases in 7 Minutes Each](https://www.youtube.com/watch?v=sXGlQruUrWE)