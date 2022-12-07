---
layout: post
title: "AWS - Costing"
description: ""
category: posts
tags: [aws, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Costing
Cost explorer enables cost forecasting and cost reduction, while Compute Optimizer users ML against CloudWatch metrics to right size resources including Lambda, EC2, AND EBS volumes saving up to 25%.

## Cost allocation Tags &amp; Tag Editor
Cost Allocation Tags show up as column in billing reports; can be applied after activation; takes 24 hours for tags to show up in reports and in console. Two types of tags that are activated separately:
 
 - AWS Generated - ```aws:``` 

 - User Tags - ```user:```

 Use the [Tag editor](http://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/tag-editor.html) to restriction actions to resources across tons of resources. These tags can then be used as *conditions* in policies.

## Trusted Advisor
Cost optimization recommendations and service limits come into play here. All customers get 7 core checks and recommendations and the ability to get email notifications from the console. The full set of Trusted Advisor features are only available for business and enterprise support plans. In addition to more features, the ability to add CloudWatch alarms and programmatic access via the Support API are added with enterprise and business support. Trusted advisor can also be used to determine service limit issues. AWS support center OR the AWS Service Quotas service can be use to increase the limits.

## Service Quotas

Service Quotas enables the management of service quotas via the console, CLI, or SDK. In addition you can create CloudWatch alarms. AWS support center OR the AWS Service Quotas service can be use to increase the limits.

## Budgets &amp; Cost Explorer

Budgets are an AI based approach to projecting monthly costs. By default, there is no budget setup and they don't show refunds. Budgets are updated every 24 hours and include SNS/CloudWatch alarms capability. Four types of budgets: Usage, Cost, Reservation, Saving Plan. Two free budgets then 2 cents a day per budget; up to 5 SNS notifications per budget.

Budgets Actions can apply an IAM policy, SCP or stop EC2/RDS service AND optionally execute these applications in a workflow. Another approach is to use SNS as a trigger a Lambda to move the account to a different OU. 

Budgets can be centralized in the Management account, called Centralized Budget Management. Another approach is to decentralize budgets across accounts using standardized budgets applied by cFN.

## Savings Plans
Savings Plans is a flexible pricing model that offers low prices on EC2, Lambda and Fargate usage, in exchange for a commitment to a consistent amount of usage (measured in $/hour) for a 1 or 3 year term. There are three types:
- Compute savings plans (up to 66%) - flexible on compute option (EC2, Fargate, Lambda), region/AZ, OS, tenancy

- EC2 Savings plans (up to 72%) - EC2 family and region specific; OS, tenancy, AZ flexible

- SageMaker Savings plan (up to 64%) - The plans automatically apply to eligible SageMaker ML instance usage, including SageMaker Studio notebooks, SageMaker notebook instances, SageMaker Processing, SageMaker Data Wrangler, SageMaker Training, SageMaker Real-Time Inference, and SageMaker Batch Transform regardless of instance family, size, or region. 

## S3

There are tons of ways to save money on S3. 

- Select just the data you need from an object? S3 Select or Glacier Select

- Data that becomes less actively accessed over time? S3 lifecycle rules

- Archived data that does not need to be readable directly? Compress it!

- Data being used by a third-party? S3 requestor pays (need to create a bucket policy so customer does not assume role in the account AND not end up getting charged.)






