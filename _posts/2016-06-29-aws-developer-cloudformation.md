---
layout: post
title: "AWS Developer - CloudFormation"
description: ""
category: posts 
tags: [aws, developercert, cloudformation]
---
{% include JB/setup %}

[CloudFormation](https://aws.amazon.com/cloudformation/) templates are JSON documents that build AWS infrastucture. The cool part is that these templates can be version control and run over and over again - this is infrastructure as code in a fleshy-codey sort of way! The only bummer is that you can only have 20 CloudFormation stacks in a region - and of course, you can increase this by contacting AWS. Obviously you can create as many CloudFormation templates as you want.


## CloudFormation Templates
CloudFormation Templates have 8 main sections but only the resources section is required.

* AWSTemplateFormatVersion -- this specfies the template version.. duh.

* Description -- this specifies what the heck the template does. Where can you stamp the version of the file you are creating?? A Description block requires an `AWSTemplateFormatVersion` block.

* Metadata -- as the name suggests, this sets up additional information about each of the resources

* Parameters -- imagine default values and customized template values and stuff you can pass in on the commandline OR when you run the template. Sounds a lot like commander or commander.js functionality!

* Mappings -- maps keys to values - imagine a different value for each AWS region!

* Conditions -- imagine the ability to conditionally do stuff. For instance, you can create slightly different configurations for a production or development environments 

* Resources -- create different resources like S3 buckets, EC2 Instances; this is the only required section of the template

* Outputs -- think `console.log();` you can output stuff like the URL of the website, or other variable.

## Functions
Intrinsic Functions - Some value that you need to access will not be known until runtime; think IP address or DNS name, anything that might vary each time the CloudFormation template is run. In these cases use the `Fn::GetAtt`, which retrieves the attribute from a resource, or other functions like `Fn:FindInMap` in mappings section. These functions can only be used in the  resource properties, metadata attributes, and update policy attributes.

CloudFormation Rollback - If a CloudFormation template run does not complete successfully then by default it all gets rolled back which feels like something very similiar to a transaction. 

DependsOn - The CloudFormation template engine is smart enough to figure out many dependencies but in some cases resources require a "DependsOn" attribute. A VPC-gateway, an Auto Scaling group, and IAM roles are all required to include a DependsOn block.

## CloudFormation API

There are two different CLI interfaces to CloudFormation, the older [CloudFormation CLI](https://s3.amazonaws.com/awsdocs/AWSCloudFormation/2010-05-15/cfn-ug-cli-ref-09172013.pdf) and the newer [AWS CLI](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-using-cli.html). This test covers the older version so here a little table comparing the two:

| CloudFormation CLI  | AWS CLI API  | Notes  |
|:--------------------------:|:--------------------------:|:--------------------------:|
| cfn-describe-stacks    | describe-stacks   |   only list stacks that are running |
|  cfn-list-stacks |   list-stacks  |   lists all stacks over last 90 |
| ?? | ListStackResources | list all the stack resources |

# Resources

## Qwik Labs
* [Introduction to AWS CloudFormation](https://qwiklabs.com/focuses/2931) - Complete

* [Introduction to AWS CloudFormation Designer](https://qwiklabs.com/focuses/2932) - Complete

* [Working with AWS CloudFormation](https://qwiklabs.com/focuses/2867) - Complete (no love on this one... lab was mostly broken)

## Reading
* [AWS Cloudformation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

* [Cloud Formation Template Reference](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html)