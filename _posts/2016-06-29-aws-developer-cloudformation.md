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

## Validating

From the command line use --template-body or --template-url to validate a template. Dependency errors, insufficient IAM permissions, invalid value/unsupported resource property, Security Group ID does not exist in VPC (you might have used a SG name instead of an ID), wait condition didn't receive the required number of signals (did the cf scripts get installed on the instance?), 

## Creating
The user running the template must have enough IAM rights to launch the resources in the template.

Bootstrapping - use cfn-* tools to get instances prep'd; there is a trade off between pre-baked images and init scripts; storing sensitive information is key as well - the "NoEcho" attribute of a parameters is an important part of the solution; rolling updates

CloudFormation with Puppet - puppet master holds instruction and definitions; clients connect and follow instructions

CloudFormation with Chef - better for longer deployments; OpsWorks for detailed stuff like configure software, deploy apps, scale on demand and monitor resources for performance, security & cost. CloudFormation supports the following resource types:

- AWS::OpsWorks::App

- AWS::OpsWorks::ElasticLoadBalancerAttachment

- AWS::OpsWorks::Instance

- AWS::OpsWorks::Layer

- AWS::OpsWorks::Stack

CloudFormation with Elastic BeanStalk - Can use CloudFormation to deploy Elastic Beanstalk; less flexible than OpsWorks; better suited to immutable deployments

### Wait Conditions
Wait conditions should be used when synchronizing resources creation; DependsOn only checks to see if the resource has been created... Should not be used for EC2 instance or autoscaling groups... use creation policies for these resources.

The lifecycle is straightforward - while waiting they are ```CREATE_IN_PROGRESS``` then they are rolled back if they get a ```CREATE_FAILED```. The wait condition has three properties:

- Count - how many signals must be received to make the 

- Handle - a ```Ref``` to the wait handle

- Timeout - the time out period in seconds

#### To setup a wait condition:

First, reate a wait handler (one per wait condition)

```yaml
myWaitHandler:
  Type: AWS::CloudFromation::WaitConditionHandle
  Properties:
```
Second, create a ```WaitCondition``` resources using the ```DependsOn``` attribute to start the timeout after that resource has been completed.

```yaml
myWaitCondition:
  Type: AWS::CloudFormation::WaitCondition
  DependsOn: Ec2Instance
  Properties:
    Handle:
      Ref: MyWaitHandler
```


### Creation Policies
When additional tasks must be completed before the resource meets the CREATE_COMPLETE state, a CreationPolicy can be used. When the resource spins up it signals the stack using helper scripts, the ```SignalResource``` API or an API call.

```yaml
CreationPolicy:
  AutoScalingCreationPolicy: #only for autoscaling groups
    MinSuccessfulInstancesPercent: 
  ResourceSignal:
    Count: Integer # default = 1
    TimeOut: String # ISO8601 format; PT1H30M10S = 1h 30m 10s

```

#### configSets



## Helper Scripts

### cfn-init

Reads the instance meta-data io install pages, write files and enable/disable software

### cfn-signal

Sends back messages to stack to signal success or failure

cfn-hup

deamon that detects resource metadata change and takes actions on those changes

## Rollback
CloudFormation Rollback - If a CloudFormation template run does not complete successfully then by default it all gets rolled back which feels like something very similiar to a transaction. First you might see a ```CREATE_FAILED``` message the likely a ```ROLLBACK_IN_PROGRESS``` message in the CF log. Rollbacks can be disabled to assist in troubleshooting.

## Updates
Think if the update will cause downtime before you do it. Is the change mutable or immutable? Generally you will see a ```UPDATE_IN_PROGRESS``` then an ```UPDATE_COMPLETE``` once the update is complete. Resource meta updates are controlled by the cfn-hub daemon, which, by default, runs every 15 minutes.

### Stack Policy

By default after you create a stack, anyone can update the stack and there is no (Stack Policies)[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html]. To restrict access to stack updates, a stack policy can be applied to the stack, which, by default, protects all the resources in the stack. You have to explictly ```Allow``` updates; only one stack policy per stack; many resources per (Stack Policy)[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html].

## Deleting

### DeletionPolicy
When creating a (DeletionPolicy)[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-deletionpolicy.html] Specify ```Delete```, ```Retain``` or ```Snapshot``` as an attribute of the resources. Snapshot works for things that might be snapshotted... like EBS, RDS, & Redshift cluster.

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