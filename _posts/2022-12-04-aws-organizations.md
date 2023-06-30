---
layout: post
title: "AWS - Organizations"
description: ""
category: posts
tags: [management aws-solutions-arch-pro]
---
{% include JB/setup %}

# What is AWS Organizations
[AWS Organizations](https://aws.amazon.com/organizations/) is an account management service that helps grow and scale AWS resources across accounts enabling programmatic account creation, accounts grouping, simplified billing, and governance. What isn't clear at first is why this is an important service - an AWS account is really a container for resources - and large orgs have thousands of accounts which all contain separate, independent functional resources. Managing thousand, tens of thousands or even hundreds of thousands of accounts or simply having is really, really difficult without AWS Organizations, Control Tower, and AWS Config. 

Organizations has two modes: 1/ _Consolidated billing_ 2/ _All features_. All features includes consolidated billing, requires invited accounts to enable all features. Can switch from mode #1 to mode #2; can't turn off all features after it is enabled. Control and usage of RI and Savings plan are centralized in the payer account using the _Disable credit sharing_ feature. When enabled all accounts can share RI &amp; Savings plans or just individual accounts in the organization if needed. 

## Concepts
An organization has a _Management account_ (also called _Payer Account_; previously called Master Account), which is independent of other accounts and contains no functional resources, and many linked accounts which contain functional resources called _Member Accounts_. The Management account does NOT have access to the functional resources or any resources in linked accounts. Organizations makes a role called `OrganizationAccountAccessRole` to enable control over linked accounts. Accounts can be created using Organizations or they can be created separately and invited. If they are invited, the `OrganizationAccountAccessRole` must be created manually with administration access.

The organization root, also root container, contains the other OU and the ~~master~~ management account. Accounts can be grouped into _Organizational units_ (OU) (or _Container_) is analogous to a directory; Containers can contain accounts and other OU. Logical: An OU can only have one parent; an account can only be in a single OU (or Container).

Moving an account from one org to another is easy. Removed the account from org 1; invite from org 2; accept invite from org 2 in the account.

## Policies
There are four types of Policies:
- _Service Control Policies(SCP)_ - control what can and can't be done and can be assigned to accounts OR to OU. 
- _AI services opt-out policies_ keep AWS AI services from storing and using customer data to improve service performance. These services can be opted out of by enabling opt-out policies, creating an opt-out policy, and attaching the policy to the root, OU, or account.
- _Backup policies_ enables the standardization to define backup across an organization.
- _Tag policies_ standardize tags on all tagged resources

Using conditions in policies is quite useful. Tags can be used as conditions using the `aws:TagKeys` condition key. To make sure things are tagged, `aws:TagKeys` can be used in combination with `ForAllValues` to make sure the tags just exist OR `ForAnyValue` to have at least these tags present. `aws:RequestedRegion` is a handy way of restricting where resources are created. The `Null` condition can be used to require a tag is present. 

### Service Control Policies (SCP)
SCP control the maximum available permissions boundary *in the account* that can be applied at the OU or Account level. SCP applied to an OU apply to all member accounts. SCP are not enabled by default. SCP do NOT apply to the management account. SCP don't effect service-linked roles. There are two approaches to using SCP: _Deny lists_ and _Allow lists_ 

_Deny lists_ policies polices are the default; you write deny policies that restrict all the permissions allowed in the `FullAWSAccess` policy that exists by default. 

_Allow lists_, require replacing the `FullAWSAccess` policy with a policy that starts by a Deny of a set of services then an _allow list_ policies. Because an explicit Deny overrides any allow, this makes it really difficult to manage.

## Resource Access Manager
Resource Access Manager enables cross account sharing of resources inside an organization or just with other random accounts. This is a better alternative than cross-account access using resource-based policies through integration with Organizations, visibility for shared resources and easier, more transparent administration. The goal is to avoid resource duplication. 

Common use cases include VPC sharing - but security groups and the default VPC can't be shared which enables participants to manage their own resources in there. Security groups from other accounts can be referenced enabling cross account sharing easily. Can also share Transit Gateway, Route53 (Resolver rules, DNS Firewall Rule Group), License Manager Configurations, Aurora Clusters, ECS dedicated hosts and capacity reservations, and tons of other stuff. Resources shared in a VPC can be addressed using private IP addresses.

A Managed Prefix List is another great application enabling sharing of a list of IP/CIDR addresses. AWS managed prefix list is also defined. Route53 Outbound Resolver rules are very appropriate to decrease the sprawl of DNS and increase reuse of the same data.

### Resource Sharing Workflow
0. enable sharing with an AWS organizations (if applicable)
0. the owner account creates a _Resource Share_  (and retains full ownership) and names the principal to share with (account or OU) and names it.
0. If in Organization, and sharing is enabled, the acceptance of the share is automatic; if not in org or sharing is not enabled, and invite is sent and must be accepted.
0. Select the resource permissions
0. Choose principals: anyone (including accounts, roles, users) OR only with Org (entire Org or specific OU)

If the account being shared with is NOT in an Organization, an invitation is sent to the account.

### Shared Resource Usage Workflow
0. Respond to the invitation (if the resource is being shared outside of an Organization)
0. Use the resource... 
0. Notice you can't see resources from other accounts that are using the Resource Share.

## Analysis - Storage Lens
_S3 Storage Lens_ delivers *Organization*-wide, as in AWS Organizations, visibility into object storage usage, activity trends, and makes actionable recommendations to optimize costs and apply data protection best practices. 

## Control Tower
[Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html) is another abstraction on top of Organizations. It enables the set up of an environment in a few clicks, implements guardrails, detects violations, provides remediation tools, and can monitor compliance. By default, CT creates a Foundational OU (Security) with an Audit Account and a Log Archive account, a Sandbox OU, and a Custom OU that will contain provisioned accounts. The _Account Factory_ feature that uses AWS Service Catalog to create accounts &amp; Config/SCP for guardrails for the Custom OU. 

Guard rails can be preventive using SCP (which are enforced or not enabled) OR detective using Config (which are clear, in violation, or not enabled ), comes in different levels Mandatory, Strongly Recommended, Elective; use [IAM access advisor](https://aws.amazon.com/about-aws/whats-new/2019/06/now-use-iam-access-advisor-with-aws-organizations-to-set-permission-guardrails-confidently/) to set permission guardrail confidently.