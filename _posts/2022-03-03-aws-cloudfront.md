---
layout: post
title: "AWS - CloudFront"
description: ""
category: posts
tags: [cloudfront, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

[CloudFront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) uses edge locations and regions as a CDN point of presence. 

One gotcha is that in cases of duplicate request from different clients CloudFront will wait for first request to resolve before processing the second request. 

Per Distribution service limits include:

- 40 Gbps data transfer rate

- 100,000 requests per second

- 25 Origins

- 100 CNAMES + wildcard CNAME

Per Account limits include:

- 200 web distributions; 100 RTMP

## Distributions

A Distribution is the name for the CDN you create - essentially a collection of Edge Locations. Two types of distributions: RTMP, for Adobe Flash based content, and a Web Distribution which can use S3 or a custom origin.

GET, HEAD, OPTIONS can be cached; PUT, POST, PATCH, DELETE are not cached and do not invalidate objects in the cache

Custom origins and custom origin rules determine which part of website requests go where.

You have to disable the distribution before you can delete it!

### RTMP Distributions

Unlike these file based options RTMP Distributions is an actual streaming of video instead of serving files and the origin must be a S3 bucket. Does not support iOS or HTML and only support Adobe Flash.

### Web Distributions

#### Static Content

Generally a longer TTL setting for a piece of content improves the performance. A 0 TTL forces CloudFront to check the `If-Modified-Since` header to see if the origin has changed. 

#### Dynamic Content

If it is dyanamic content use a custom origin; enable forwarding query string; and set the TTL to 0. TTL = 0 enables a pull through caching mechanism where CloudFront sends a GET request to the origin with an "If-Modified-Since" header.

Types of dynamic content includes:

- Unique content per request

- Not unique but has a short TTL - stale content will be served if origin is unavailable

- End user input changes user request

#### Video Distributions

Video streaming is essentially a web cloud front distribution that hosts video oriented files - except for RTMP. There is no [streaming switch](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-hit-ratio.html#cache-hit-ratio-http-streaming) for CloudFront. Types of video oriented distribution options include:

- On-demand streaming - web cloud front distribution

- Smooth Streaming - a web distribution configured with custom origin "Enable Smooth Streaming" enabled; for Microsoft format data

- Progressive Downloads - again use a web distribution and use an HTTP-based dynamic streaming protocol such as Apple HTTP Dynamic Streaming (Apple HDS), Apple HTTP Live Streaming (Apple HLS), Microsoft Smooth Streaming, or MPEG-DASH. Progressive downloads won't work with signed URLS.

- Live streaming - pretty much the same as progressive downloads except the origin is a WOWZA media server which probably sits on an EC2 instance. CloudFront is super effective with a WOWZA server...

Optimize distribution by not forwarding headers, cookies or query strings in these scenarios and use signed cookies for access control.

### Access Control Content

Access control can be done using signed URLs or using signed Cookies using the Restrict Viewer Access option. This is done using a policy...

Use signed URLs for RTMP. Progressive Download can use a signed URL, user could use it to download the entire file which is sort of bad for some use cases. Use for marketing emails too.

Use signed cookies for progressive downloads or smooth streaming, because there are tons of small files and signed URLs won't work. Use for whole site authentication too.

Resticting users from going directly to S3 use Origin Access Identity (OAI) which is a CloudFront user you can give read access to the origin bucket.

## Configuration 

CloudFront has many, many configuration options including:

* Device detection - Redirect or serve different content based on user agent

* CNAMEs - supports wildcard CNAMES and up to 100 CNAMES total

* Origin KeepAlive Timeout - keep connection from edge location to the CloudFront origin open

* Allowed HTTP Methods - can be just GET and HEAD

* Origin Protocol Policy - HTTP, HTTPS or Match Viewer; S3 only support HTTP

### HTTPS

If the origin is S3 all requests are made to the distribution will stay the same protocol... so a request in HTTPS then it will be forwarded via HTTPS. Custom Origins can be configured to use HTTP only or to match viewer, which will match the user agent protocol.

There are three different ways to configure SSL (in order of goodness):

- Generic SSL Cert - using *.cloudfront.net SSL certificate

- SNI Custom SNL - included with CloudFront; multiple domains to serve SSL over same IP address; good support overall browsers; 

- Dedicated IP Custom SSL - expensive; limited support from ancient browsers

## Cloudfront Reports

[Cloudfront reporting](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/reports.html) uses logs data but does not require additional processing of the logs to get the good reporting sort of stuff. There are reports on caching metrics, popular objects, top referrers, and user agent information as well. 

CloudFront generates metrics to CloudWatch, CloudTrail and HTTP access logs. 

## CloudFront Security

Geo Restrictions - by default this is disabled; you can either disable (blacklist) or enable (whitelist); restricted viewers get a `403` and yes, you can create custom error pages

As a protip, don't cache credit card information in CloudFront edge caches. For example, you can configure your origin to include a Cache-Control:no-cache="field-name" header in responses that contain credit card information, such as the last four digits of a credit card number and the card owner's contact information.

Redirect HTTP to HTTPS is a common security feature which can be enable on Cloudfront.

### Invalidation Techniques

Objects are cached for the life of the TTL

- Slow, cheap, sucky invalidation: Delete origin file; wait for TTL to expire

- Fast, Expensive invalidation: Use Invalidation API to remove object from edge location

- Blue/green invalidation: Use route53 CNAME to point new distribution as an alternative

- Build Process invalidation: Rename files

### Best practices 

- Support zone apex? - use Route53 to alias to CloudFront distribution; no charge for alias record lookup (Queries to Alias records that are mapped to Elastic Load Balancers, Amazon CloudFront distributions, AWS Elastic Beanstalk environments, and Amazon S3 website buckets are free.)

- Redirect HTTP to HTTPS is a common security feature which can be enable on Cloudfront.

- Need multiple Geo Restrictions? create multiple distributions

- If your web server is not connected to a load balancer, we recommend that you use web server variables instead of the X-Forwarded-For header to avoid IP address spoofing.

- Configure PUT to edge location and forward to S3

- Increase performance of your website? Increase cache hit percentage is the #1 way to increase CloudFront performance. Increasing min and max TTL helps improve this metric.

- Realtime metrics about website? monitor distribution with CloudWatch

- Increase HA? multiple Distributions behind Route53

- Custom Error Message? from S3 with low TTL

- Use Route53 Alias records which are free instead of CNAME; Alias your zone apex

- Uploading via CDN is faster but does not cache. 

- [Serving Private Content through CloudFront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/PrivateContent.html)

- [Increase cache hits](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-hit-ratio.html)

- Restrict access to S3 content? origin access identity, which is a special CloudFront user with a bucket policy

