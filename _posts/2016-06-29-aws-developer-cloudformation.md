---
layout: post
title: "AWS Developer - CloudFormation"
description: ""
category: posts 
tags: [aws, developercert, cloudformation, solutionsarch, devopspro]
---
{% include JB/setup %}

[CloudFormation](https://aws.amazon.com/cloudformation/) templates are JSON documents that build AWS infrastucture. The cool part is that these templates can be version control and run over and over again - this is infrastructure as code in a fleshy-codey sort of way! The only bummer is that you can only have 20 CloudFormation stacks in a region - and of course, you can increase this by contacting AWS. Obviously you can create as many CloudFormation templates as you want.

NOTE: These notes aren't just for the AWS Developer Certification... they also include information on the DevOps Pro Certification as well.

# CloudFormation Templates
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

DependsOn - The CloudFormation template engine is smart enough to figure out many dependencies but in some cases resources require a "DependsOn" attribute. A VPC-gateway, an Auto Scaling group, and IAM roles are all required to include a DependsOn block.

## Nested Templates

The AWS::CloudFormation::Stack resource can be used to call another template from within another template.

# CloudFormation Lifecycle

## Creating

The user running the template must have enough IAM rights to launch the resources in the template.

## Validating


## Rollback
CloudFormation Rollback - If a CloudFormation template run does not complete successfully then by default it all gets rolled back which feels like something very similiar to a transaction. First you might see a `CREATE_FAILED` message the likely a ROLLBACK_IN_PROGRESS message in the CF log. Rollbacks can be disabled to assist in troubleshooting.

## Updates
Think if the update will cause downtime before you do it. Is the change mutable or immutable? Generally you will see a UPDATE_IN_PROGRESS then an UPDATE_COMPLETE once the update is complete. Resource meta updates are controlled by the cfn-hub daemon, which, by default, runs every 15 minutes.

### (Stack Policies)[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html]

By default after you create a stack, anyone can update the stack and there is no Stack Policy. To restrict access to stack updates, a stack policy can be applied to the stack, which, by default, protects all the resources in the stack. You have to explictly `Allow` updates; only one stack policy per stack; many resources per (Stack Policy)[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html].

## Deleting

### (DeletionPolicy)[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-deletionpolicy.html]
Specify `Delete`, `Retain` or `Snapshot` as an attribute of the resources. Snapshot works for things that might be snapshotted... like EBS, RDS, & Redshift cluster.

## Troubleshooting
A couple of troubleshooting tips:

* You can't stop and start an instance to modify its AMI; you must replace the instance; CF deals with instance ID changes.

* S3 buckets must be empty before they can be deleted by CF

* Resource dependencies must be addressed in the reverse order of create... so delete the EC2 instance prior to deleting the VPC. 

* User permissions apply when deleting stuff.

* If a DELETE_FAILED message comes up you can use the RetainResources parameter to unstick the stack.

* Rollback fail? Nested stacks have dependencies that keep rollbacks from occuring OR the resource has been modified in a way the keeps it from being deleted.  



# CloudFormation API

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