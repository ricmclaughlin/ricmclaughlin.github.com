---
layout: post
title: "AWS - CloudFront"
description: ""
category: posts
tags: [cloudfront, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

[CloudFront](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) uses edge locations and regions as a CDN point of presence. Also protects against networking and application layer attack and integrates with Shield, WAF, &amp; Route53 while using HTTPS on requests and pull forwards. Support websockets.

# Origins
There are numerous content origins:

- _S3_ - S3 Website hosted must be enabled; use Origin Access Control (OAC) to restrict access to the bucket to CloudFront; can also be used as an ingress point for files into S3; 

- _MediaStore Container &amp; MediaPackage Endpoint_ to deliver Video on Demand or live streaming video using AWS Media Services

- _Custom Origin (HTTP)_ including ALB/CLB, EC2 (with public IP), API Gateway, or any other HTTP endpoint. Three ways to deny direct origin access by clients: use SG to deny other access to these resources to any other actor than CloudFront., configure CloudFront &amp; origin to use a _Custom HTTP Header_ including a secret, OR restrict access to CloudFront public IP using a NACL/Firewall. 

## Origin Groups
To increase HA and do failover, create a primary and secondary origin in an _Origin Group_. Origin groups can span regions enabling cross-region HA.

# CloudFront Security

Geo Restrictions - by default this is disabled; you can either allow or block by country; restricted viewers get a `403` and yes, you can create custom error pages

As a protip, don't cache credit card information in CloudFront edge caches. For example, you can configure your origin to include a Cache-Control:no-cache="field-name" header in responses that contain credit card information, such as the last four digits of a credit card number and the card owner's contact information.

Redirect HTTP to HTTPS is a common security feature which can be enable on Cloudfront.

## Access Control Content

Access control can be done using signed URLs or using signed Cookies using the Restrict Viewer Access option. This is done using a policy...

Use signed URLs for RTMP. Progressive Download can use a signed URL, user could use it to download the entire file which is sort of bad for some use cases. Use for marketing emails too.

Use signed cookies for progressive downloads, smooth streaming, or scenarios where you DON'T want the URL to change. Use for whole site authentication too.

### CloudFront Signed URL vs S3 Pre-SIgned URL
CloudFront Signed URL give access to a path (no matter the origin), uses an account wide key-pair, can filter by IP, path, date, expiration, and use caching features. 

Use presigned S3 URLs when you aren't using CloudFront, uses the IAM principal of the signer, and has a limited lifetime.

# Pricing
To reduce cost, the number of edge location can be reduced using _Price Classes_. There are three price classes:
0. all regions - best performance
0. Class 200 - most regions, but excludes the expensive ones
0. Class 100 - only the least expensive ones

# Customization at the Edge
CloudFront has two different ways to change requests/responses at the edge: _CloudFront functions_ and _Lambda@Edge_. This customization enables request filtering, AuthZ/N, HTTP responses, Bot mitigation, and A/B testing. Can't use both Functions and Lambda@Edge on the viewer req/res at the same time.

## CloudFront Functions
Native feature of CloudFront deployed at the edge point of presenses; modifies/transforms the viewer req/res; written in JavaScript; can scale to millions of TPS with less than 1ms access time; 1/6 the cost of Lambda@Edge; no access to the file system network. Use CloudFront Functions for:
- Cache Key normalization
- Header manipulation
- URL rewrites/redirects
- AuthZ/N

## Lambda@Edge
deployed at regional edge cache; written in Python or Node.js in a VM with network and file system access; scales to 1000s of TPS; used to modify/transform the viewer req/res and origin req/res; author functions in us-east-1 then replicated to regional edge cache locations. Up to 5 seconds execution time on the view side and up to 30 seconds on the origin side. Use Lambda@Edge for:
- longer execution time
- more cpu
- access to AWS services
- access to request body (perhaps to load the right sized image based on the browser `User-Agent`)
- route request to load content to be cached from the closest origin (for example, load content from the closest bucket to the user into the closest edge location)

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

## Configuration 

CloudFront has many, many configuration options including:

* Device detection - Redirect or serve different content based on user agent

* CNAMEs - supports wildcard CNAMES and up to 100 CNAMES total

* Origin KeepAlive Timeout - keep connection from edge location to the CloudFront origin open

* Allowed HTTP Methods - can be just GET and HEAD

* Origin Protocol Policy - HTTP, HTTPS or Match Viewer; S3 only support HTTP

* Custom Error pages - when the origin returns a 4XX or 5XX status code; has a min Error Caching TTL

### HTTPS

If the origin is S3 all requests are made to the distribution will stay the same protocol... so a request in HTTPS then it will be forwarded via HTTPS. Custom Origins can be configured to use HTTP only or to match viewer, which will match the user agent protocol.

There are three different ways to configure SSL (in order of goodness):

- Generic SSL Cert - using *.cloudfront.net SSL certificate

- SNI Custom SNL - included with CloudFront; multiple domains to serve SSL over same IP address; good support overall browsers; 

- Dedicated IP Custom SSL - expensive; limited support from ancient browsers

## Cloudfront Reports

[Cloudfront reporting](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/reports.html) uses logs data but does not require additional processing of the logs to get the good reporting sort of stuff. There are reports on caching metrics, popular objects, top referrers, and user agent information as well. 

CloudFront generates metrics to CloudWatch, CloudTrail and HTTP access logs. 



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

- Increase HA? multiple distributions behind Route53

- Custom Error Message? from S3 with low TTL

- Use Route53 Alias records which are free instead of CNAME; Alias your zone apex

- Uploading via CDN is faster but does not cache. 

- Restrict access to S3 content? origin access identity, which is a special CloudFront user with a bucket policy

