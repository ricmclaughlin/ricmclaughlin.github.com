---
layout: post
title: "AWS - Cross Account Role Switch Walkthrough"
description: ""
category: projects
tags: [iam, aws, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

One of the most common situations in real life with AWS is cross account access to the AWS console. This post is a very simple scenario to demonstrate how that can be implemented.

Scenario: There are a group of developers in a group called "developers-admin-groupin an account called "development" that need to place source code into a bucket in the "production" account so it can be deployed to servers.

- ID the production and development account numbers

- Create the bucket in the production account.. let's call it "prod-source-dropoff"

- In the production account, create "developers-access-production-policy" policy

```json
{ 
  "Version": "2012-10-17", 
  "Statement": [ 
    { 
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets", 
      "Resource": "arn:aws:s3:::" 
    }, 
    { 
      "Effect": "Allow", 
      "Action": [ 
        "s3:ListBucket", 
        "s3:GetBucketLocation" 
      ], 
      "Resource": "arn:aws:s3:::prod-source-dropoff" 
    }, 
    { 
      "Effect": "Allow", 
      "Action": [ 
        "s3:GetObject", 
        "s3:PutObject", 
        "s3:DeleteObject" 
      ], 
      "Resource": "arn:aws:s3:::prod-source-dropoff/" 
    }
  ]
}
```

- In the production, account create "developer-access-production-role" role and assign 'developers-access-production-policy'

- Login to developer account

- Create a role called "sts-developer-production-bucket-access-role" with an inline policy of:

```json
{ 
  "Version": "2012-10-17", 
  "Statement": {
    "Effect": "Allow", 
    "Action": "sts:AssumeRole", 
    "Resource": "arn:aws:iam::PRODUCTION-ACCOUNT-ID:role/developer-access-production-role" 
    } }
```

- Assign the role to the "developers" group

- Login into the developer account as a member of the developers group and do a "switch role" and input the account number and the role "developers-access-production-policy" and click "switch role"

- You should have access to the "prod-source-dropoff" bucket in the production account



