---
layout: post
title: "AWS Developer Certification - CloudFormation"
description: ""
category: posts 
tags: [aws, developercert, cloudformation]
---
{% include JB/setup %}

[CloudFormation](https://aws.amazon.com/cloudformation/) templates are JSON documents that build AWS infrastucture. The cool part is that these templates can be version control and run over and over again - this is infrastructure as code in a fleshy codey sort of way!

CloudFormation Templates have 8 main sections but only the resources section is required.

* AWSTemplateFormatVersion -- this specfies the template version.. duh.

* Description -- this specifies what the heck the template does. Where can you stamp the version of the file you are creating??

* Metadata -- as the name suggests, this sets up additional information about each of the resources

* Parameters -- imagine default values and customized template values and stuff you can pass in on the commandline. Sounds a lot like commander or commander.js functionality!

* Mappings -- maps keys to values - imagine a different value for each AWS region!

* Conditions -- imagine the ability to conditionally do stuff. For instance, you can create slightly different configurations for a production or development environments 

* Resources - create different resources like S3 buckets, EC2 Instances; this is the only required section of the template

* Outputs - think `console.log();`

Intrinsic Functions - Some value that you need to access will not be known until runtime; think IP address or DNS name, anything that might vary each time the CloudFormation template is run. In these cases use the `Fn::GetAtt` or other functions like `Fn:FindInMap` in template.

CloudFormation Rollback - If a CloudFormation template run does not complete successfully then by default it all gets rolled back which feels like something very similiar to a transaction. 
