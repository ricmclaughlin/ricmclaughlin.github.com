---
layout: post
title: "AWS SysOps - Security"
description: ""
category: posts
tags: [aws, sysops, security, solutionsarch]
---
{% include JB/setup %}

## AWS Shared Responsbility Model
The customer is responsible for the stuff "in" the cloud. Some thing are related to AWS like user access management, security groups, ACL and VPC configuration. Others are things like application level security, OS security and patches as well as OS security configuration.

AWS is responsible for stuff "of" the cloud things like the network ACL, API Endpoints, DDOS protection, EC2 spoof data surpression, and hypervisor instance isolation. Physical access to facilities for anyone is no OK.

Port scanning and penetration testing is against AWS protocol but can be done with AWS permission.

AWS does provide protection from DDoS, Man in the middle (MITM), IP Spoofing and port scanning. Other tenants can not packet sniff.

## AWS and IT Audits
For IT audits, AWS provides information to auditors from the host OS to the physical security of the building but will not allow physical access to the facilities - the security "of" the cloud. This leaves AWS customers responsible for everything else like data, apps and infrastructure setup - the security "in" the cloud.

## Multi-Factor Authentication (MFA)
Console Access? Turn on MFA app, FOB or other hardware MFA!

### MFA Integration
Some services like S3, SQS and SNS can force users of an API to use MFA. This requires an integration with STS. For instance, you can require MFA to access S3 buckets via the API or the bucket owner can create a policy that disables object deletion.

## STS
AWS [Security Token Service (STS)](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users that you authenticate (federated users). 

After the first call from the app to STS, it returns a temporary security credential including an access key/secret key pair (just like AWS does) and a session token. The AWS STS API call `AssumeRole` requests a temporary security credential - you will want to save these credentials for the length of the session and request new ones before they expire.

When using an API there are two options to use the temporary security credentials: add the session token to the HTTP header or adding it to the query string parameter `X-AMZ-Security-Token`.

Overall, this service seems to be a backend to [AWS Cognito](https://aws.amazon.com/cognito/) and somewhat obsolete.

## Underlying OS Access
There are a number of AWS services that enable us to get low and down to the OS level - EMR, EC2, ECS, Elastic BeanStalk and Opswork. Other platforms like RDS, DynamoDB do NOT allow access to OS level details.

## Bastion Hosts
Create the instance in your public subnet and assign it a public IP address. Then, use ssh-agent forwarding or OpenSSH ProxyCommand to connect to your private instances.

## Network Access Control Lists vs Security Groups

* Security Groups - firewall port filtering at the instance level renamed; allows filtering inbound and outbound; return traffic is allowed so it is state-ful; supports allow rules only

* Network Access Control List - firewall port filtering at the subnet level renamed; allows filtering inbound and outbound; return traffic is not specifically allowed so it is stateless; Each ACL rule has a number and rules are evaluated ascending; Best practice is to create rule numbers by 5 so other rules can be added later; Once rules are added all other traffic is denied by default. A network ACL overrides security groups. Only 1 ACL per subnet and each subnet MUST have an ACL. Unlike IAM or Security groups in a Network ACL an explicit Deny CAN override an explicit Allow if allow is a higher rule number.

## Security Credentials

1. Password - duh. These get setup in the IAM. 

1. MFA - these get setup in the IAM too.

1. Access Keys - these include an access key and secret access key

1. Key Pairs - SSH login, CloudFront Signed URLs

1. x.509 - used to sign SOAP requests for S3

## Trusted Advisor
Not a generic security tool - a security tool for IAM but does not tell you to 

1. turn on user activity logging

2. Use a MFA for the root account

## Resources
[Security WhitePaper](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf)
