---
layout: post
title: "AWS - ID federation"
description: ""
category: posts
tags: [iam, sts, aws, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# STS vs SSO vs AWS IAM Identity Center
_Security Token Service (STS)_ forms the backbone low-level interface for federation while _AWS Single Sign-On_ enables the ability to centrally manage access to AWS accounts and business applications. _Identity Center_ replaces all of the cases - login across accounts in organization, business apps, SAML 2 apps, and even EC2 Windows instances.

## The common steps to federation at runtime are:
1. Authenticate with Identity Provider (IdP) - IdP returns a pre-signed URL

2. Obtain Temporary Security Credentials - Done by calling STS, although this could be deeper down in the service stack so you aren't calling it directly. STS, or the interface, returns session credentials, AKID, Secret Access Key, Session Token, Expiration, OR all that in a pre-signed URL.

3. Access the AWS resource by passing in the STS provided credentials - now, based the users' role, you have access to AWS Resources using the session credentials issued by STS. When this occurs 

## Common APIs to use cases
`sts:AssumeRoleWithSAML`

`sts:AssumeRoleWithWebIdentity`

`sts:GetSessionToken```



# Corporate Identity Federation

## IAM Identity Center
Successor to AWS SSO and has built in identity store or can use AD, OneLogin, Okta. Straightforward approach: Permission sets define access into account; assign permission sets to groups. There's a portal generated that enables access to all the resources.

Identity Center enables multi-account permissions, applications (basically using SAML 2.0) and Attribute-Based Access Control (ABAC) which is the use of attributes in the Identity Store for access control. 

## SAML 2.0 Federation
Use Case: Access to console, CLI, API using temp credentials AD or other SAML 2.0 IdP

Background: AD and it's hosted version [AWS Directory Service](https://aws.amazon.com/directoryservice/), LDAP and SAML can be integrated into IAM; generally you map groups in the ID provider to IAM roles

Flow: Auth with trusted IdP (could also be Active Directory Federation Service - ADFS) which returns SAML token; from here:
0. the user can sign on to the console using the token and the AWS sign-in endpoint for SAML at `https://signin.aws.amazon.com/saml` where the endpoint calls `AssumeRoleWithSAML`
0. the app can call the STS service and call `AssumeRoleWithSAML` and use the returned credentials to access the resource

Pro: No dedicated proxy on the corporate side and the proxy requires no IAM user or permissions

- API call: `AssumeRoleWithSAML`

## Custom Identity Broker
Use case: IdP isn't SAML 2.0 compatible 

Flow: The custom identity broker requests credentials, proper IAM role, from STS using `sts:GetFederationToken` then does a `sts:AssumeRole` to get credentials. From here the user can be use credentials to access API/CLI or be redirected to the console.

- API call: `sts:GetFederationToken` & `sts:AssumeRole`

# Web Identity Federation
Using the Social Identity Provider, which is an IAM object that holds configuration info about the external provider, this feature enables users to use a well-known authentication provider then use that ID to assume an IAM role. 

## Web Identity (Standard)
AWS recommends using Cognito instead. Without using Cognito, the user auths with a web IdP; using the auth'd ID then calls the `AssumeRoleWithWebIdentity`; STS validates the auth evaluates the Role/Trust policy then returns the AWS credentials. 

- API call: `AssumeRoleWithWebIdentity`

## WIF with Cognito 
Cognito pools enable multiple providers IDs to be stored and treated uniformly. Generally two roles are associated with the pool: one for authenticated users and one for unauthenticated users. Supports MFA and data sync; 

- API call:

# STS 
AWS [Security Token Service (STS)](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for federated users. Federation is creating a trust relationship between an identity store like Amazon, Facebook, Google (also called an IdP), Active Directory, IAM, any SAML 2.0 system and AWS. Users are authenticated with the Federated identity store FIRST before they hit AWS. 

# Triage
* AuthZ mobile/web using social - Cognito Identity Pools

* AuthN mobile/web using social - Cognito User Pools (mobile focused)

## Identity Triage

AuthZ mobile/web using social - Cognito Identity Pools

AuthN mobile/web using social - Cognito User Pools (mobile focused)

AuthN/Z (AWS resources using directory) - AWS SSO

AuthN/Z AWS resources from another account? - IAM role or resource policy