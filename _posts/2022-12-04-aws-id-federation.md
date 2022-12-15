---
layout: post
title: "AWS - ID federation"
description: ""
category: posts
tags: [iam, sts, aws, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# STS vs SSO vs AWS IAM Identity Center
Security Token Service (STS) forms the backbone low-level interface for federation while AWS Single Sign-On enables the ability to centrally manage access to AWS accounts and business applications. Identity Center replaces all of the cases - login across accounts in organization, business apps, SAML 2 apps, and even EC2 Windows instances.

## IAM Identity Center
Successor to AWS SSO and has built in identity store or can use AD, OneLogin, Okta. Straightfoward approach: Permission sets define access into account; assign permission sets to groups. There's a portal generated that enables access to all the resources.

ID Center enables multi-account permissions, applications (basically using SAML 2.0) and Attribute-Based Access Control (ABAC) which is the use of attributes in the Identity Store for access control. 

## STS 
AWS [Security Token Service (STS)](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for federated users. Federation is creating a trust relationship between an identity store like Amazon, Facebook, Google (also called an IdP), Active Directory, IAM, any SAML 2.0 system and AWS. Users are authenticated with the Federated indentity store FIRST before they hit AWS. 

STS roles can require an External ID (basically a password) which requires the use of CLI, API or user of IAM:AssumeRole calls and also require MFA which forces the use of the console. Session tags can be used in combination with the ```aws:PrincipalTag``` in policies to restrict access of roles.

## The common steps to federation at runtime are:

1. Authenticate with Identity Provider 

2. Obtain Temporary Security Credentials - Done by calling STS, although this could be deeper down in the service stack so you aren't calling it directly. STS, or the interface, returns session credentials, AKID, Secret Access Key, Session Token, Expiration, OR all that in a pre-signed URL.

3. Access the AWS resource by passinstg in the STS provided credentials - now, based the users' role, you have access to AWS Resources using the session credentials issued by STS. When this occurs 

There are several use cases for STS including Corporate ID federation, Web Identity federation, and Cross account federation. The other sort of odd, less commonly thought of, use case for STS is in roles assigned to EC2 instance or other AWS services on your behalf.

## Common APIs to use cases
```sts:AssumeRole```

```sts:AssumeRoleWithSAML```

```sts:AssumeRoleWithWebIdentity ```

```sts:GetSessionToken```

```sts:GetFederationToken```

# Corporate Identity Federation

Overall, the approach is to map user groups from the directory service to IAM roles, then have the user assume the role AFTER authentication with the IdP... typically LDAP or AD.

The Console Access with Broker and the API Access with Broker scenarios need an identity proxy that must communicate with the IdP and STS and hold an IAM user credentials and more. Storing these credentials on a server that can be compromised is not ideal.

The `AssumeRole` with SAML implementation method does not require a dedicated proxy and generally a better fit with more corporate scenarios. 

## Console Access with Broker

Use Case: SSO to IAM console access using local directory service; no SAML 

Flow: User Requests browses to proxy; auth against directory which returns group membership; proxy gets list of roles from groups via STS; user selects role; proxy calls `STS:AssumeRole` then returns auth package; proxy generates console redirect.

Con: IAM user required for proxy server 

## API Access with Broker

Use Case: an app needs access to AWS resources via API; no SAML

Flow: App requests session from proxy; auth against directory which returns entitlements; proxy requests session from STS using `STS:GetFederationToken`; STS returns session; app calls AWS API using session

Cons: The IAM user associated to the proxy must have `GetFederationToken` policy and all the premissions for all of the users; and `GetFederationToken` does not support MFA 

## AssumeRoleWithSAML

Use Case: SSO to IAM console without proxy server against AD or other SAML IdP

Background: AD and it's hosted version [AWS Directory Service](https://aws.amazon.com/directoryservice/), LDAP and SAML can be integrated into IAM; generally you map groups in the ID provider to IAM roles

Flow: Auth with IdP which returns SAML token; with token in-hand the user is redirected to AWS sign-in endpoint for SAML at `https://signin.aws.amazon.com/saml`; the endpoint calls `AssumeRoleWithSAML` then creates and passes the user a console URL redirect; Roles must be configured to include the "saml:group": "groupname"

Pro: No dedicated proxy on the corporate side and the proxy requires no IAM user or permissions

## Web Identity Federation

Using the Social Identity Provider, which is an IAM object that holds configuration info about the external provider, this feature enables users to use a well-known authentication provider then use that ID to assume an IAM role. 

After the first call from the app to STS, it returns a temporary security credential including an access key/secret key pair (just like AWS does) and a session token. The AWS STS API call `AssumeRole` requests a temporary security credential - you will want to save these credentials for the length of the session and request new ones before they expire.

### Web Identity (Standard)

Without using Cognito, the user auths with a web IdP; using the auth'd ID then calls the `AssumeRoleWithWebIdentity`; STS validates the auth evaluates the Role/Trust policy then returns the AWS credentials. AWS recommends using Cognito instead.

### WIF with Cognito 

Cognito pools enable multiple providers IDs to be stored and treated uniformally. Generally two roles are associated with the pool: one for authenticated users and one for unauthenticated users. If an unauth'd user authenticates, or reauths with a different ID the IDs can be merged. User pools store AuthN; identify pools store AuthZ.

- Cognito Unauthenticated - using this flow the user creates a new unauthenticated user in the Cognito pool then requests an OpenID token; client calls ```STS:AssumeRole``` to swap the OpenID token out for a role; STS verifies the request and issues credentials. 

- Cognito Authenticated Classic (Simple) - this flow is exactly the same as the Web Identity (Standard) flow except it handles a single pool of users, unauth'd to auth'd flow and abstracts away some of the complexity from handling more than one IdP. 

- Cognito Authenticated Enhanced (Simplfied) - this flow removes several steps in the auth process. Client auths with IdP; client does a get or create ID with Cognito which validates the auth; client requests credentials for ID from Cognito; Cognito returns the goods

## Triage

* AuthZ mobile/web using social - Cognito Identity Pools

* AuthN mobile/web using social - Cognito User Pools (mobile focused)

## Identity Triage

AuthZ mobile/web using social - Cognito Identity Pools

AuthN mobile/web using social - Cognito User Pools (mobile focused)

AuthN/Z (AWS resources using directory) - AWS SSO

AuthN/Z AWS resources from another account? - IAM role or resource policy