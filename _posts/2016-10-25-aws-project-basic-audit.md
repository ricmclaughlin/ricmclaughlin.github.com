---
layout: post
title: "AWS Project - Basic Account Audit"
description: ""
category: projects
tags: [aws, how-to]
---
{% include JB/setup %}

Say you wanted to do a basic audit of a small AWS account - make sure there are not any *glaring* holes in the security, make sure you aren't spending $10k a month without knowing it, make sure your services aren't out of limits. What sort of things would you assess? Here is a light-weight version of what I look at. Not exhaustive. Yet, in a 5 minute, 30 minute and 2 hour format. Plus a bit of monitoring thrown in to make sure it all stays sane. 

And the fact that this audit does not require any special tools: just trusted advisor. 

# 5 Minute Review

###Security:

1. Check IAM - are all the security checks done? If not, fix that now.

2. Password Spec - Lots of different ways to specify the password spec for your account and I won't judge... just make sure there is a password spec that makes sense.

3. Has the IAM login URL been customized? This is sort of goofy but if the account has been renamed it is more difficult for random hacker to FIND your console login. Takes about 4.65 seconds to fix.

### Cost:

1. Can IAM users see the [bill](https://console.aws.amazon.com/billing/home?#/account)? If not enable it.

2. Billing and Budgets enabled? If not, enable. Set a $1000 budget and track if you spend 1% if this is a free account.

Ok, that isn't a lot of stuff yet useful and do-able in 5 minutes.

# 30 Minute Review

1. For the first 5 minutes of this review, do the 5 minute review.

1. Fire up [Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/)

### Security:

Service Limits:

Checks for usage that is more than 80% of the service limit. Values are based on a snapshot, so your current usage might differ. Limit and usage data can take up to 24 hours to reflect any changes. In some cases, your usage might be greater than the indicated limit for a period of time.


# 2 Hour Review
IAM In-Depth
[Credential Report](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)
Policy simulator as Audit Evidence

Cloud Watch 

## On-going Monitoring

Cost Monitoring

AWS Config - AWS Config is a super effective tool for monitoring the changes in the  configuration of your environment - it examines what you have running and configured and dumps it to a json file. Over time, you can understand what you have, what has changed and all that good sort of stuff. 

Cost Explorer

## Principles

1. Don't assign policies directly to users. This has never been a good idea and remains and bad idea. Hard to track and manage.

2. Encrypt data at rest - first, off, why not? It is so easy to do... just click the box. Integrates so nicely with AWS Key Management System

3. 




## Resources

[Trusted Advisor Best Practices](https://aws.amazon.com/premiumsupport/trustedadvisor/best-practices/) - You don't get all bells and whistles in the "free" version of the app but a quick glance will show you what you want to look for in an indepth audit.

