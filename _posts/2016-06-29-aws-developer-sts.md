---
layout: post
title: "AWS Developer - Delegation and Federation"
description: ""
category: posts
tags: [aws, developercert, sts]
---
{% include JB/setup %}

7:49 - 

Although basic it is note-worthy that Delegation is when you authorize access to others or other things to resources in your account while Federation when you allow other parties to authorize other on your behalf.

A role is required to do authentication and authorization  

## Corporate and ID Federation

AD and it's hosted version [AWS Directory Service](https://aws.amazon.com/directoryservice/), LDAP and SAML can be integrated into IAM; generally you map groups in the ID provider to IAM roles

login give you session credentials, AKID, Secret Key, Session Token and Expiration

sts:AssumeRole, 15, 1 hour, 1 hour
GetFederationToken, 15 min, 36 hours, 12 hours

### AssumeRole vs GetFederationToken

Both return permissions

## Web Identity Federation

Use Social Identity Provider, which is an IAM object that holds configuration info about the external provider; 



## Role Switching &amp; Cross Account Roles

https://aws.amazon.com/blogs/security/enable-a-new-feature-in-the-aws-management-console-cross-account-access/



AWS [Security Token Service (STS)](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users that you authenticate (federated users). 

After the first call from the app to STS, it returns a temporary security credential including an access key/secret key pair (just like AWS does) and a session token. The AWS STS API call `AssumeRole` requests a temporary security credential - you will want to save these credentials for the length of the session and request new ones before they expire.

When using an API there are two options to use the temporary security credentials: add the session token to the HTTP header or adding it to the query string parameter `X-AMZ-Security-Token`.

To make this work, you first need to develop an identity broker that communicates with LDAP and STS and in all scenarios the the broker authenticates with LDAP first then with STS

Past that, your app gets temp access after receiving a token

OR 

Your app can assume an IAM role to interact with an AWS resource.