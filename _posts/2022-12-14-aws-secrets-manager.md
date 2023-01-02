---
layout: post
title: "AWS - Secrets Manager"
description: ""
category: posts
tags: [aws, secrets-manager, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

# What does Secrets Manager do?
AWS Secrets Manager helps you protect secrets needed to access your applications, services, and IT resources.

## Service Features
- Easily replicate secrets to multiple region
- Secure and audit secrets centrally - audit with CloudTrail
- Custom Lambda functions to extend Secrets Manager rotation to other secret types, such as API keys and OAuth tokens. 
- Generate random secrets

## Operations
Use Resource Policies to control access to secrets.
Account sharing secret
0. Created resource based policies for secret granting `secretsmanager:GetSecretValue`
0. Grant the user the ability to decrypt the secret using `kms:Decrypt` via Secrets Manager `kms:ViaService": "secretsmanager.your-region.amazonaws.com` (enable the user to have KMS decrypt the secret so it can be retrieved)

To use Secrets Manager from CloudFormation: 
0. Create Secret.
0. Reference Secret in Resource (like a RDS instance)
0. Use `SecretTargetAttachment` to tell Secrets Manager where the secret is used so it can be rotated.

# Triage
- Secret access? user ARN of secret.
- Inject secret into container? full contents, specific JSON key, specific version (all via ARN) and set to environment variable.

## When to use Secrets Manager instead of Parameter store
- Secrets rotation? can rotate Amazon RDS, Amazon Redshift, and Amazon DocumentDB keys
- Secrets rotation for other services? can fire a Lambda to rotate keys for other services
- Fine grained access control (like restricting retrieval from an IP address range)? yep.
- Cross-region capabilities? yep.
- Cross-account? nope.

## When to use Parameter Store 
- non-encrypted data
- need integration with CloudFormation
- manual control over secrets rotation (EventBridge -> Lambda -> Rotate key)



