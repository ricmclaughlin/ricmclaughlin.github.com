---
layout: post
title: "AWS Developer Certification - IAM"
description: ""
category: posts
tags: [aws, developercert, iam]
---
{% include JB/setup %}

[AWS Indentity and Access Management](https://aws.amazon.com/iam/) (IAM) enables you to securely control access to AWS services. As such there is no cost involved with it, yet IAM is one of the services that every single AWS solution uses and does NOT apply to a single region.

## Identity Management
Root Account Credentials - the user account that created the AWS account is called the *root account*. The root account has complete access to ALL the resources on the account - makes sense. You also can't restrict access control to the root account - makes sense because otherwise a different user **could** lock the root account out of the account they created!

IAM users - Authentication is the name of the game here. 

Federated users - Authentication can occur via some other mechanism and be added granted access to AWS resources via IAM. This is done using the Security Token Service or if you are hip you would use [Cognito Identity](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html).

## Roles vs Access Keys
A role is set of permissions that can be attached to groups, users or services. Roles are not access key credentials - they don't give a user the ability to login to AWS - they control what the user can do or not do on AWS.

For example, IAM roles are used for an EC2 instance that needs access to S3 instead of creating a credential. IAM Roles can grant acces to users from one AWS account to another or enable a mobile app to access AWS resources.

When an IAM role is assigned to a resource, the resource takes on that role. As an application of this concept, the code that runs on an EC2 instance takes on the role assigned to that instance.

An access key is used to sign requests from outside AWS. AWS SDKs and CLI use access keys. Of course they can be disabled. Of course they can be deleted. They can not be retrieved. Access keys do have a temporary nature to them which can be optionally assigned.

## IAM &amp; STS
The AWS [Security Token Service API]({{ BASE_PATH }}/posts/aws-developer-certification-security-token-service) for you to request temporary security credentials and this is very closely related to IAM.

Federation - creating a trust relationship between an identity store like Amazon, Facebook, Google (also called an Identity Broker), Active Directory, any SAML 2.0 system and AWS. Users are authenticated with the Federated indentity store FIRST before they hit AWS. Make sense... how could it work the other way around?? The steps are:

1. Authenticate with Identity Provider 
2. Obtain Temporary Security Credentials - Done by calling STS using a call to `AssumeRoleWithWebIdentity` or `AssumeRoleWithSAML`
3. Access the AWS resource - now, based the users' permission, you have access to AWS Resources.

# Resources
## Qwik Labs
[qwiklabs - Introduction to AWS Identity and Access Management (IAM)](https://qwiklabs.com/focuses/2885) - This is a low cost (1 credit) lab that simply walks through the stuff of IAM - policies, users, groups and touches on roles.

## Reading
[IAM FAQ](https://aws.amazon.com/iam/faqs/)

## Videos
[IAM Policy Ninja](https://www.youtube.com/watch?v=Du478i9O_mc)
[IAM Best Practices to Live By](https://www.youtube.com/watch?v=_wiGpBQGCjU)