---
layout: post
title: "AWS - Organizations"
description: ""
category: posts
tags: [aws, organizations, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# What is AWS Organizations
[AWS Organizations](https://aws.amazon.com/organizations/) is an account management service that helps grow and scale AWS resources accross accounts enabling programmatic account creation, accounts grouping, simplified billing, and governance. What isn't clear at first is why this is an important service - an AWS account is really a container for resources - and large orgs have thousands of accounts which all contain seperate, independent functional resources. Managing thousand, tens of thousands or even hundreds of thousands of accounts or simply having is really, really difficult without AWS Organiztions, Control Tower, and AWS Config.

## Set up
Generally, an organization has a paying account, which is independent of other accounts and contains no functional resources, and many linked accounts which contain functional resources. The paying account does NOT have access to the functional resources or any resources in linked accounts.

Accounts can be grouped into organizational units (OU). Service control policies (SCP) and tag policies help control what can and can't be done and can be assigned to accounts OR to OU.

## Resource Access Manager

Resource Access Manager enables cross account sharing of resources inside an organization or just with other random accounts. This is a better alternative than cross-account access using resource-based policies through integration with Organizations, visibility for shared resources and easier, more transparent adminstration.

### Resource Sharing Workflow

0. Create a shared resource - this is more _exposing_ the resource to the Organizations OU or other specific listed accounts in the region

If the account being shared with is NOT in an Organization, an invitation is sent to the account.

### Shared Resource Usage Workflow

0. Respond to the invitation (if the resource is being shared outside of an Organization)

0. Use the resource... 

## Details

- An OU can only have one parent

- An account can only be in a single OU

- SCP get applied to all accounts in the OU

- SCP can allow and deny actions