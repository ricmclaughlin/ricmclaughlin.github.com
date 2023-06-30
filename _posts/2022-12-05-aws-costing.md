---
layout: post
title: "AWS - Costing"
description: ""
category: posts
tags: [management, aws-guides, aws-solutions-arch-pro]
---
{% include JB/setup %}

The _Cost Explorer_ enables cost forecasting and cost reduction, while the _Compute Optimizer_ uses ML against CloudWatch metrics to right size resources including Lambda, EC2, AND EBS volumes saving up to 25%.

# Set up
## Cost Allocation Tags &amp; Tag Editor
Cost Allocation Tags show up as column in billing reports; can be applied after activation; takes 24 hours for tags to show up in reports and in console. Two types of tags that are activated separately:
- AWS Generated - `aws:` 
- User Tags - `user:`

 Use the [Tag editor](http://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/tag-editor.html) to tag resources in mass. Tags can then be used as *conditions* in policies using Resource Based Access Control (RBAC).

## Savings Plans
Savings Plans is a flexible pricing model that offers low prices on EC2, Lambda and Fargate usage, in exchange for a commitment to a consistent amount of usage (measured in $/hour) for a 1 or 3 year term. There are three types:
- EC2 Savings plans (up to 72%) - EC2 family and region specific; OS, tenancy, AZ flexible
- Compute savings plans (up to 66%) - flexible on compute option (EC2, Fargate, Lambda), region/AZ, OS, tenancy
- SageMaker Savings plan (up to 64%) - The plans automatically apply to eligible SageMaker ML instance usage, including SageMaker Studio notebooks, SageMaker notebook instances, SageMaker Processing, SageMaker Data Wrangler, SageMaker Training, SageMaker Real-Time Inference, and SageMaker Batch Transform regardless of instance family, size, or region. 

# Analysis 
## Compute Optimizer
AWS Compute Optimizer is a service that analyzes the configuration and utilization metrics of AWS resources using _Compute Optimizer enhanced infrastructure metrics_ which is a paid feature. It analyzes: EC2, ASG, EBS, Lambda, Fargate, but NOT ECS on EC2. The compute optimizer dashboard reports:
0. Saving opportunities - requires an opt in to Cost Explorer and activate *Receive Amazon EC2 resource recommendations* in the Cost Explorer preferences page. 
0. Performance improvement opportunities
0. Findings - grouped information

## S3
There are three tools that can assist with [S3 cost analysis](/posts/aws-s3#analysis): S3 Storage class analysis, which provides recommendations for Standard and Standard IA class objects, and S3 Storage Lens which enables storage analysis for Organizations (lots to this tool) and S3 Inventory which feels like the non-Organizations version. 

## Trusted Advisor
Cost optimization recommendations and service limits come into play here. All customers get 7 core checks and recommendations and the ability to get email notifications from the console. The full set of Trusted Advisor features are only available for Business and Enterprise support plans. In addition to more features, the ability to add CloudWatch alarms and programmatic access via the Support API are added with enterprise and business support. Trusted advisor can also be used to determine service limit issues. AWS support center OR the AWS Service Quotas service can be use to increase the limits.

## Cost Explorer
Cost Explorer is a tool that enables you to view and analyze your costs and usage. You can explore your usage and costs using the main graph, the Cost Explorer cost and usage reports, or the Cost Explorer RI reports. You can view data for up to the last 12 months, forecast how much you're likely to spend for the next 12 months, and get recommendations for what Reserved Instances to purchase. You can use Cost Explorer to identify areas that need further inquiry and see trends that you can use to understand your costs. 

## Budgets 
_Budgets_ are an AI based approach to projecting monthly costs. By default, there is no budget setup and they don't show refunds. Budgets are updated every 24 hours and include SNS/CloudWatch alarms capability. Four types of budgets: Usage, Cost, Reservation, Saving Plan. Two free budgets then 2 cents a day per budget; up to 5 SNS notifications per budget. Budget Actions can take action on Budgets as documented below.

Budgets can be centralized in the Management account, called Centralized Budget Management. Another approach is to decentralize budgets across accounts using standardized budgets applied by cFN.

# Reporting 
## Cost & Usage Report
The _Cost and Usage Reports (CUR)_ contains the most comprehensive set of cost and usage data available at the account or organization level. These results can be delivered to a bucket or create an updated report. From S3 it is possible to directly query the data with Athena or load the data directly into Redshift using an include SQL script. 

# Actions
## Service Quotas
Service Quotas enables the management of service quotas via the console, CLI, or SDK. In addition you can create CloudWatch alarms. AWS support center OR the AWS Service Quotas service can be use to increase the limits.

## Budget Actions
_Budgets Actions_ can apply an IAM policy, SCP or stop EC2/RDS service AND optionally execute these applications in a workflow. Another approach is to use SNS as a trigger a Lambda to move the account to a different OU. 

## S3
There are tons of ways to save money on S3. 
- Select just the data you need from an object? S3 Select or Glacier Select
- Data that becomes less actively accessed over time? S3 lifecycle rules
- Archived data that does not need to be readable directly? Compress it!
- Data being used by a third-party? S3 requestor pays (need to create a bucket policy so customer does not assume role in the account AND not end up getting charged.)





