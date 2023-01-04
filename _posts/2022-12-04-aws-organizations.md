---
layout: post
title: "AWS - Organizations"
description: ""
category: posts
tags: [aws, organizations, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

# What is AWS Organizations
[AWS Organizations](https://aws.amazon.com/organizations/) is an account management service that helps grow and scale AWS resources across accounts enabling programmatic account creation, accounts grouping, simplified billing, and governance. What isn't clear at first is why this is an important service - an AWS account is really a container for resources - and large orgs have thousands of accounts which all contain separate, independent functional resources. Managing thousand, tens of thousands or even hundreds of thousands of accounts or simply having is really, really difficult without AWS Organizations, Control Tower, and AWS Config. 

Organizations has two modes: 1/ _Consolidated billing_ 2/ _All features_. All features includes consolidated billing, requires invited accounts to enable all features. Can switch from mode #1 to mode #2; can't turn off all features after it is enabled. Control and usage of RI and Savings plan are centralized in the payer account using the _Disable credit sharing_ feature. When enabled all accounts can share RI &amp; Savings plans or just individual accounts in the organization if needed. 

## Set up
Generally, an organization has a paying account, which is independent of other accounts and contains no functional resources, and many linked accounts which contain functional resources. The paying account does NOT have access to the functional resources or any resources in linked accounts. Organizations makes a role called `OrganizationAccountAccessRole` to enable control over linked accounts. Accounts can be created using Organizations or they can be created separately and invited. If they are invited, the `OrganizationAccountAccessRole` must be created manually.

The organization root is associated with the ~~master~~ management account. 

Accounts can be grouped into _Organizational units_ (OU). _Service control policies_ (SCP) and tag policies help control what can and can't be done and can be assigned to accounts OR to OU. Logical: An OU can only have one parent; an account can only be in a single OU.

Moving an account from one org to another is easy. Removed the account from org 1; invite from org 2; accept invite from org 2 in the account.

## Service Control Policies (SCP)
SCP control the maximum available permissions for ALL users that can be applied at the OU or Account level but does NOT apply to the management account. SCP applied to an OU apply to all member accounts. SCP don't effect service-linked roles. SCP must have an explicit Allow - they don't allow anything by default. An explicit Deny disables the use of the service in the account. There are three types of SCP:

- _Multi-Account Permission policies_ enable access across accounts - and creates role in accounts being shared.

- _AI services opt-out policies_ keep AWS AI services from storing and using customer data to improve service performance. These services can be opted out of by enabling opt-out policies, creating an opt-out policy, and attaching the policy to the root, OU, or account.

- _Backup policies_ enables the standardization to define backup across an organization.

Using conditions in policies is quite useful. Tags can be used as conditions using the `aws:TagKeys` condition key. To make sure things are tagged, `aws:TagKeys` can be used in combination with `ForAllValues` to make sure the tags just exist OR `ForAnyValue` to have at least these tags present. `aws:RequestedRegion` is a handy way of restricting where resources are created. The `Null` condition can be used to require a tag is present. 

## Control Tower
[Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html) is another abstraction on top of Organizations. It enables the set up of an environment in a few clicks, implements guardrails, detects violations, provides remediation tools, and can monitor compliance. Includes an account factory that uses AWS Service Catalog to share resources. 

Guard rails - preventive, detective (using Config), comes in different levels mandatory, Strongly Recommended, elective; use [IAM access advisor](https://aws.amazon.com/about-aws/whats-new/2019/06/now-use-iam-access-advisor-with-aws-organizations-to-set-permission-guardrails-confidently/) to set permission guardrail confidently.

## Resource Access Manager
Resource Access Manager enables cross account sharing of resources inside an organization or just with other random accounts. This is a better alternative than cross-account access using resource-based policies through integration with Organizations, visibility for shared resources and easier, more transparent administration. The goal is to avoid resource duplication. 

Common use cases include VPC sharing - but security groups and the default VPC can't be shared which enables participants to manage their own resources in there. Security groups from other accounts can be referenced enabling cross account sharing easily. Can also share Transit Gateway, Route53 (Resolver rules, DNS Firewall Rule Group), License Manager Configurations, Aurora Clusters, ECS dedicated hosts and capacity reservations, and tons of other stuff. 

A Managed Prefix List is another great application enabling sharing of a list of IP/CIDR addresses. AWS managed prefix list is also defined. Route53 Outbound Resolver rules are very appropriate to decrease the sprawl of DNS and increase reuse of the same data.

### Resource Sharing Workflow
0. Create a shared resource - this is more _exposing_ the resource to the Organizations OU or other specific listed accounts in the region

If the account being shared with is NOT in an Organization, an invitation is sent to the account.

### Shared Resource Usage Workflow
0. Respond to the invitation (if the resource is being shared outside of an Organization)

0. Use the resource... 

## Analysis - Storage Lens
_S3 Storage Lens_ delivers *Organization*-wide, as in AWS Organizations, visibility into object storage usage, activity trends, and makes actionable recommendations to optimize costs and apply data protection best practices. 

