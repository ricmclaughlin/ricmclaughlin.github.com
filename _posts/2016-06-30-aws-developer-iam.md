---
layout: post
title: "AWS Developer - IAM"
description: ""
category: posts
tags: [aws, developercert, iam]
---
{% include JB/setup %}

[AWS Indentity and Access Management](https://aws.amazon.com/iam/) (IAM) enables you to securely control access to AWS services. As such there is no cost involved with it, yet IAM is one of the services that every single AWS solution uses and does NOT apply to a single region.

## Identity Management

Root Account Credentials - the user account that created the AWS account is called the *root account*. The root account has complete access to ALL the resources on the account - makes sense. You also can't restrict access control to the root account - makes sense because otherwise a different user **could** lock the root account out of the account they created!

IAM users - Authentication is the name of the game here. 

Federated users - Authentication can occur via some other mechanism and be added granted access to AWS resources via IAM. This is done using the Security Token Service or if you are hip you and using some sort of web/app authentication you would use [Cognito Identity](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html).

## Roles vs Access Keys

A role is set of permissions that can be attached to groups, users or services. Roles are not access key credentials - they don't give a user the ability to login to AWS - they control what the user can do or not do on AWS.

For example, IAM roles are used for an EC2 instance that needs access to S3 instead of creating a credential. IAM Roles can grant access to users from one AWS account to another or enable a mobile app to access AWS resources.

When an IAM role is assigned to a resource, the resource takes on that role. As an application of this concept, the code that runs on an EC2 instance takes on the role assigned to that instance.

An access key is used to sign requests from outside AWS. AWS SDKs and CLI use access keys. Of course, they can be disabled. Of course, they can be deleted. They can not be retrieved. Access keys do have a temporary nature to them which can be optionally assigned.

## Federation Overall

The AWS [Security Token Service API]({{ BASE_PATH }}/posts/aws-developer-certification-security-token-service) for you to request temporary security credentials and this is very closely related to IAM. 

Federation - creating a trust relationship between an identity store like Amazon, Facebook, Google (also called an Identity Broker), Active Directory, any SAML 2.0 system and AWS.  Users are authenticated with the Federated indentity store FIRST before they hit AWS. The common steps are:

1. Authenticate with Identity Provider 

2. Obtain Temporary Security Credentials - Done by calling STS. In a single sign-on scenarios you would call ```AssumeRoleWithSAML``` or ```AssumeRole``` or in the case of a Web Indentity Federation by calling ```AssumeRoleWithWebIdentity```.

3. Access the AWS resource - now, based the users' role, you have access to AWS Resources.

STS returns session credentials, AKID, Secret Access Key, Session Token, &amp; Expiration.

## Corporate Identity Federation

Overall, the approach is to map user groups from the directory service to IAM roles. 

### Custom Identity Proxy

There are two use cases here and both need an IAM user and this user has to be secured on the proxy server. 

#### Console Access Scenario

Use Case: SSO Console access using local directory service

Flow: User Requests browses to proxy; auth against directory which returns group membership; proxy gets list of roles from groups via STS; user selects role; proxy calls ```STS:AssumeRole``` then returns auth package; proxy generates console redirect.

Con: IAM user required for proxy server

#### API Access Scenario

Use Case: an app needs access to AWS resources via API

Flow: App requests session from proxy; auth against directory which returns entitlements; proxy requests session from sts using ```STS:GetFederationToken```; sts returns session; app calls AWS API using session

Cons: The IAM user associated to the proxy must have ```GetFederationToken``` policy and all the premissions for all of the users; and ```GetFederationToken``` does not support MFA 

### AssumeRole with SAML

Using SAML (Security Assertion Markup Language 2.0) you can give your federated users single sign-on (SSO) access to the AWS Management Console without using a corporate proxy. The AWS sign-in endpoint for SAML is ```https://signin.aws.amazon.com/saml```

Flow: Auth with Identity provider which returns SAML token; user signs on to AWS sign-in endpoint for SAML; the endpoint calls AssumeRoleWithSAML then creates and passes the user a console URL redirect

Pro: No dedicated proxy on the corporate side and the proxy requires no IAM user or permissions

### Web Identity Federation

#### Web Identity (Standard)

Without using Cognito the user auths with a web IdP; using the auth'd ID then calls the ```AssumeRoleWithWebIdentity```; STS validates the auth evaluates the Role/Trust policy then returns the AWS credentials. 

#### Cognito WIF

Cognito pools enable multiple providers IDs to be stored and treated uniformally. Generally two roles are associated with the pool: one for authenticated users and one for unauthenticated users. If the an unauth'd user authenticates, or reauths with a different ID the IDs can be merged.

- Cognito Unauthenticated - using this flow the user creates a new unauthenticated user in the Cognito pool then requests an OpenID token; client calls ```STS:AssumeRole``` to swap the OpenID token out for a role; STS verifies the request and issues credentials. 

- Cognito Authenticated Classic (Simple) - this flow is exactly the same as the Web Identity (Standard) flow except it handles a single pool of users, unauth'd to auth'd flow and abstracts away some of the complexity from handling more than one IdP. 

- Cognito Authenticated Enhanced (Simplfied) - this flow removes several steps in the auth process. Client auths with IdP; client does a get or create ID with Cognito which validates the auth; client requests credentials for ID from Cognito; Cognito returns the goods



## Security Rules

By default, ALL requests to use a resource are denied - *default deny*.

All *allows* override any default denies.

All *explicit denies* override allows.

## Rotating Credentials

Rotating keys is a big part of security... 

Roles use STS to automatically get access to AWS resources and rotate credentials several times a day. From an EC2 instance you can retrieve the information from the instance meta-data.

## Instance Profiles

Instance profiles are wrappers around an IAM role that will be passed to an EC2 instance. When an EC2 instance is created from the console the instance profile is created automatically; from the CLI you would need to create this and use it automatically

# Resources

## Use Cases

Limit action to owner (ID and email that created account)? user owner concept

## Qwik Labs

[qwiklabs - Introduction to AWS Identity and Access Management (IAM)](https://qwiklabs.com/focuses/2885) - This is a low cost (1 credit) lab that simply walks through the stuff of IAM - policies, users, groups and touches on roles.

## Reading

[IAM FAQ](https://aws.amazon.com/iam/faqs/)

## Videos

* [IAM Policy Ninja](https://www.youtube.com/watch?v=Du478i9O_mc)

* [IAM Best Practices to Live By](https://www.youtube.com/watch?v=_wiGpBQGCjU)