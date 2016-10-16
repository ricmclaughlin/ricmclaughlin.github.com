---
layout: post
title: "AWS Developer - STS"
description: ""
category: posts
tags: [aws, developercert, sts]
---
{% include JB/setup %}

AWS [Security Token Service (STS)](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users that you authenticate (federated users). 

After the first call from the app to STS, it returns a temporary security credential including an access key/secret key pair (just like AWS does) and a session token. The AWS STS API call `AssumeRole` requests a temporary security credential - you will want to save these credentials for the length of the session and request new ones before they expire.

When using an API there are two options to use the temporary security credentials: add the session token to the HTTP header or adding it to the query string parameter `X-AMZ-Security-Token`.

Overall, this service seems to be a backend to [AWS Cognito](https://aws.amazon.com/cognito/) and somewhat obsolete. 

To make this work, you first need to develop an identity broker that communicates with LDAP and STS and in all scenarios the the broker authenticates with LDAP first then with STS

Past that, your app gets temp access after receiving a token

OR 

Your app can assume an IAM role to interact with an AWS resource.