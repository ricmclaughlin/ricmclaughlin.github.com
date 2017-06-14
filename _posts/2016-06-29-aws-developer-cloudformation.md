---
layout: post
title: "AWS Developer - CloudFormation"
description: ""
category: posts 
tags: [aws, developercert, cloudformation, solutionsarch, devopspro]
---
{% include JB/setup %}

[CloudFormation](https://aws.amazon.com/cloudformation/) templates are JSON documents that build AWS infrastucture. The cool part is that these templates can be version control and run over and over again - this is infrastructure as code in a fleshy-codey sort of way! The only bummer is that you can only have 20 CloudFormation stacks in a region - and of course, you can increase this by contacting AWS. Obviously, you can create as many CloudFormation templates as you want.

NOTE: These notes aren't just for the AWS Developer Certification... they also include information on the DevOps Pro Certification as well.

# CloudFormation Templates

CloudFormation Templates have 8 main sections but only the resources section is required.

* AWSTemplateFormatVersion -- this specfies the template version.. duh.

* Description -- this specifies what the heck the template does. Where can you stamp the version of the file you are creating?? A Description block requires an `AWSTemplateFormatVersion` block.

* Metadata -- as the name suggests, this sets up additional information about each of the resources

* [Parameters](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) -- imagine default values and customized template values and stuff you can pass in on the commandline OR when you run the template. Sounds a lot like commander or commander.js functionality!

* Mappings -- maps keys to values - imagine a different value for each AWS region!

* Conditions -- imagine the ability to conditionally do stuff. For instance, you can create slightly different configurations for a production or development environments 

* Resources -- create different resources like S3 buckets, EC2 Instances; this is the only required section of the template

* Outputs -- think `console.log();` you can output stuff like the URL of the website, or other variable.

## Nested Templates

The AWS::CloudFormation::Stack resource can be used to call another template from within another template. This is useful if you want break up templates because of size (460k on S3), the number of resources is max'd out (200), or there are more than 100 mappings, 60 parameters or 60 outputs, OR want to reuse components. Parmeters and outputs are shared between the parent and child templates.

```json 
"myStack" : {
  "Type" : "AWS::Cloudformation::Stack"
  "Properties" : {
    "TemplateURL" : "https://s3.template.json"
    "Parameters" : {
      "VPC": {
        "Name" : "Value"
      }
    }
  }
}
```

## Cloudformation Components

## Intrinsic Functions

Intrinsic Functions are functions that run inside a CF template. There are helper functions and conditional logic functions. Some value that you need to access will not be known until runtime; think IP address or DNS name, anything that might vary each time the CloudFormation template is run. These functions can only be used in the resource properties, metadata attributes, and update policy attributes. 

- `Fn::Base64` - encodes string; UserData is a common use of this function

- `Fn::GetAtt` -  which retrieves the attribute from a resource

- `Fn::FindInMap` in mappings section. 

## Condition Functions





## Resource Attributes 

CF policies aren't really separate documents thet are [resource attributes](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-product-attribute-reference.html). There are numerous types of CloudFormation Policies... when do you use what?

- CreationPolicy - used to pause resource from going to create complete before success signals or timeout; applies to instances, ASG & wait conditions only

- DeletionPolicy - used to keep important stuff from getting nuked; can be `Delete`, (which is the default); `Retain` which is useful for common infrastructure architectures, AD, databases but leaves a mess; `Snapshot` which is for volumes, DBclusters, DBInstance or Redshift

- UpdatePolicy - used to dictate how updates to an ASG are handled... AutoScalingRollingUpdate &amp; 


DependsOn - The CloudFormation template engine is smart enough to figure out many dependencies but in some cases resources require a "DependsOn" attribute. A VPC-gateway, an Auto Scaling group, and IAM roles are all required to include a DependsOn block. A `DependsOn` block can take a single value or an array. 

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

First, create a wait handler (one per wait condition)

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

## Helper Scripts

### cfn-init

Reads the instance meta-data io install pages, write files and enable/disable software. 

#### configSets

ConfigSets are used to order config tasks in the template for the cfn-init process. Something like:

```yaml
AWS::CloudFormation::Init: 
  configSets: 
    ascending: 
      - "config1"
      - "config2"
    descending: 
      - "config2"
      - "config1"
  config1: 
    commands: 
      test: 
        command: "echo \"$CFNTEST\" > test.txt"
        env: 
          CFNTEST: "I come from config1."
        cwd: "~"
  config2: 
    commands: 
      test: 
        command: "echo \"$CFNTEST\" > test.txt"
        env: 
          CFNTEST: "I come from config2"
        cwd: "~" 
```

### cfn-signal

Sends back messages to stack to signal success or failure. The cfn-signal helper script signals whether an EC2 instance has been successfully created or updated. This script is used with a CreationPolicy or an Auto Scaling group with a WaitOnResourceSignals update policy. When CloudFormation creates or updates resources with those policies, the rest of the stack pauses until the resource receives the required number of success signals, or until the timeout period is reached. The cfn-signal helper is what sends those signals back - successful or not.

### cfn-hup

Used to update in place using a deamon that detects resource metadata change and takes actions on those changes.

### cfn-get-metadata

Gets a metadata block from CloudFormation and prints it out to STDOUT.


## Rollback

CloudFormation Rollback - If a CloudFormation template run does not complete successfully then by default it all gets rolled back which feels like something very similiar to a transaction. First you might see a ```CREATE_FAILED``` message the likely a ```ROLLBACK_IN_PROGRESS``` message in the CF log. Rollbacks can be disabled to assist in troubleshooting.

## Updates

Think if the update will cause downtime before you do it. Is the change mutable or immutable? Generally you will see a ```UPDATE_IN_PROGRESS``` then an ```UPDATE_COMPLETE``` once the update is complete. Resource meta updates are controlled by the cfn-hub daemon, which, by default, runs every 15 minutes.

### Update Policies

An (Update Policy)[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-updatepolicy.html] handles updates to ```AWS::AutoScaling::AutoScalingGroup``` resources.

AutoScalingReplacingUpdate and AutoScalingRollingUpdate get applied when the AutoScaling group's launch configuration changes, there is a change to the associated subnets (VPCZoneIdentifiers) or when the instances in the ASGroup don't match the current launch configuration.

In addition, if ```WillReplace: true``` and both AutoScalingReplacingUpdate and AutoScalingRollingUpdate are specified AutoScalingReplacingUpdate takes precendents.

#### AutoScalingReplacingUpdate Policy

In the case of AutoScalingReplacingUpdate, if the ```WillReplace``` attribute is true the ASGroup AND the instances in that groups are replaced.  During the update the existing AutoScaling group is used and later destroyed AFTER the update is complete. To make this scenario work well, a ```CreationPolicy``` should also be specified so the new AutoScaling group is prepared prior to cut over. As an example:

```yaml
UpdatePolicy:
  AutoScalingReplacingUpdate:
    WillReplace: true
  CreationPolicy:
    AutoScalingCreationPolicy:
      MinSuccessfulInstancesPercent: 50
    ResourceSignal:
      Count: 
        Ref: ResourcesSignalsOnCreate
      Timeout: PT10M
```

#### AutoScalingRollingUpdate Policy

AutoScalingRollingUpdate would be appropriate in the case where the AMI and app both need updates. This is a way better way of updating a fleet instead of using ```cfn-init``` and ```cfn-hub``` because rolling updates can be inconsistent.

#### AutoScalingScheduledAction Policy

In the case where updates to stacks might occur during AutoScaling group policy scheduled actions, you can specify an AutoScalingScheduledAction to prevent changes during an update. In other words, we don't our regularly time scheduled updates to the AutoScaling group's size properties to get overwritten by an update... here is how to do that:

```yaml
UpdatePolicy:
  AutoScalingScheduledAction:
    IgnoreUnmodifiedGroupSizeProperties: true
```

### Stack Policy

By default after you create a stack, anyone can update the stack and there is no [Stack Policy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html). To restrict access to stack updates, a stack policy can be applied to the stack, which, by default, protects all the resources in the stack. You have to explictly `Allow` updates; only one stack policy per stack; many resources per [Stack Policy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html).

When a resource is updated it might be replaced or have no service interuption, suffer an interuption or be deleted. If you need to update a protected resource you can temporarily replace the policy at update time.

In addition, there is no fine grain access control in a stack policy... everyone that can run the template in update mode but you must specify a `Principal:*`. One handy feature is the `Condition` block like: 

```json
{
  "Statement": [
    {
      "Condition" : {
        "ResourceType" : [
          "AWS::EC2::*"
        ]
      } 
    },
    {
      "Effect" : "Allow",
      "Action" : "Update",
      "Principle" : "*",
      "Resource" : "*"
    }
  ]
}
```

The `Action` attribute can be set to:

- Update:Modify

- Update:Replace

- Update:Delete

- Update:*


## Deleting

### DeletionPolicy

By default when a stack is deleted all the resources are deleted. To get aroudn this in various ways you can create a [DeletionPolicy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-deletionpolicy.html) Specify ```Delete```, ```Retain``` or ```Snapshot``` as an attribute of the resources. Snapshot works for things that might be snapshotted... like EBS, RDS (instances and clusters), & Redshift cluster.

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


## Reading
* [AWS CloudFormation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

* [CloudFormation Template Reference](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html)

* [CloudFormation Troubleshooting Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html)

## Qwik Labs
* [Introduction to AWS CloudFormation](https://qwiklabs.com/focuses/2931) - Complete

* [Introduction to AWS CloudFormation Designer](https://qwiklabs.com/focuses/2932) - Complete

* [Working with AWS CloudFormation](https://qwiklabs.com/focuses/2867) - Complete (no love on this one... lab was mostly broken)