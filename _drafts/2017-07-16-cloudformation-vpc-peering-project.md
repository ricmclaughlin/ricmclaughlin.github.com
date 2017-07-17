---
layout: post
title: "CloudFormation VPC Peering Project"
description: ""
category: projects
tags: [aws, devops, cloudformation, aws-dev-ops-pro, aws-solutions-arch-pro, projects]
---
{% include JB/setup %}

The goal of this project is to create a peering connection between a _requestor_ and an _accepter_ account. To make peering work the accepter account must have a role with the `ec2:AcceptVpcPeeringConection` action allowed.

## Step 1 - Create VPC &amp; Role in Accepter account

To make the peering connection, we need a VPC and a peering role.

Name this file accepter.yaml...

```yaml

```

## Step 2 - Execute 

First, configure command line with an Access key ID and a Secret access key for the accept account using `aws configure` [documented here](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html).

Second, execute the following command substituting the strings between the &lt; and &gt; below:
`bash

`
Because we are specifying role creation in the stack, but don't care about the exact name of the role, we use `--capabilities` parameter and `CAPABILITY_IAM` as [documented here](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-template.html#capabilities)


Third, copy and save the Peer Role (and its ARN output), the   from the command.


## Step 3

Now, we need to make the peering connection on the requester side. Name this file file requester.yaml...

```yaml

```
