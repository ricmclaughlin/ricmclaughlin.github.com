---
layout: post
title: "AWS - CloudFormation"
description: ""
category: posts 
tags: [aws, devops, cloudformation, aws-dev-ops-pro, aws-solutions-arch-pro]
---
{% include JB/setup %}

[CloudFormation](https://aws.amazon.com/cloudformation/) templates are JSON documents that build AWS infrastucture. The cool part is that these templates can be version control and run over and over again - this is infrastructure as code in a fleshy-codey sort of way! The only bummer is that you can only have 20 CloudFormation stacks in a region - and of course, you can increase this by contacting AWS. Obviously, you can create as many CloudFormation templates as you want.

# CloudFormation Templates

CloudFormation Templates have 8 main sections but only the resources section is required. All sections are independent of each other. 

* AWSTemplateFormatVersion - this specfies the template version.. duh.

* Description - this specifies what the heck the template does. Where can you stamp the version of the file you are creating?? A Description block requires an `AWSTemplateFormatVersion` block.

* [Parameters](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) - imagine default values and customized template values and stuff you can pass in on the command line OR when you run the template. 

* Mappings - maps keys to values - imagine a different value for each AWS region!

* Resources - create different resources like S3 buckets, EC2 Instances; this is the only required section of the template

* Outputs - think `console.log();` you can output stuff like the URL of the website, or other variable.

* Metadata - as the name suggests, this sets up additional information about each of the resources

* Transform - used for the [Serverless](https://github.com/awslabs/serverless-application-model) definition

* Conditions - imagine the ability to conditionally do stuff. For instance, you can create slightly different configurations for a production or development environments 

## Nested Templates

The AWS::CloudFormation::Stack resource can be used to call another template from within another template. This is useful if you want break up templates because of size (460k on S3), the number of resources is max'd out (200), or there are more than 100 mappings, 60 parameters or 60 outputs, OR want to reuse components. Parameters and outputs are shared between the parent and child templates.

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

### Resource Types

Sets the [type of resource](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) you want to create... for example, 

```yaml
  Type: "AWS::EC2::Instance"
  Properties: 
```

### Resource Properties Types

Within resources, there are numerous different data structures that must be built to set options - essentially, you are creating an embedded object in a resource. And these embedded objects have types which can be set using the `Type` property sets the possible properties available as documented the [resource specification](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-resource-specification.html). 

### Pseudo Parameters

Pseudo parameter are predefined template parameters for values like `AWS::AccountId`, `AWS::NotificationARNs`, `AWS::Region`, `AWS::StackId` and `AWS::StackName`.

### Intrinsic Functions

Intrinsic Functions are functions that run inside a CF template. There are helper functions and conditional logic functions. Some value that you need to access will not be known until runtime; think IP address or DNS name, anything that might vary each time the CloudFormation template is run. These functions can only be used in the resource properties, metadata attributes, and update policy attributes. 

- `Fn::Base64` - encodes string; UserData is a common use of this function

- `Fn::GetAtt` -  which retrieves the attribute from a resource

- `Fn::FindInMap` - finds something in the mappings section 

- `Fn::Sub` - variable subsitution using a litteral block like ${varName}

- `Ref` - reference the ID of a resources; can be written as `!Ref` or `Ref:`

- `Fn::ImportValue` - imports a value from another stack exported using the `Export` attribute in the `Outputs` section of a template

- `Fn::Join` - join strings together

- `Fn::Select` - select an item from an array

#### Condition Function

Normal programming conditional logic can be addressed using `Fn::And`, `Fn::Equals`, `Fn::If`, `Fn::Not` and `Fn::Or`.

### Resource Attributes 

CF policies aren't really separate documents that are [resource attributes](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-product-attribute-reference.html). There are numerous types of CloudFormation Policies... when do you use what?

- `CreationPolicy` - used to pause resource from going to create complete before success signals or timeout; applies to instances, ASG & wait conditions only

- `DeletionPolicy` - used to keep important stuff from getting nuked; can be `Delete`, (which is the default); `Retain` which is useful for common infrastructure architectures, AD, databases but leaves a mess; `Snapshot` which is for volumes, DBclusters, DBInstance or Redshift

- `UpdatePolicy` - used to dictate how updates to an ASG are handled... 

### Custom Resources

[Custom Resources](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) enable you to support AWS Products that lack CF support, operate on non-AWS resources and perform advanced logic is limited. A custom resource directly runs a Lambda function or creates then sends SNS message when the resource is created, updated, or deleted. This can be specfied like `Custom::MyCustomThingy` or `AWS::CloudFormation::CustomResource`.

Here is a quick summary of how to make a [Custom Resources](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html):

0. Define the service token, which is a URL that must be in the same region, in a template.

1. The template is used to create, update or delete a custom resource and the engine calls the service token.

2. The custom resource provider processes the cloudformation request and returns a response of `SUCCESS` or `FAILURE`

# CloudFormation Lifecycle

## Validating

From the command line use `--template-body` or `--template-url` to validate a template. Dependency errors, insufficient IAM permissions, invalid value/unsupported resource property, Security Group ID does not exist in VPC (you might have used a SG name instead of an ID), wait condition didn't receive the required number of signals (did the cf scripts get installed on the instance?), 

## Creating

The user running the template must have enough IAM rights to launch the resources in the template.

Bootstrapping - use cfn-* tools to get instances prep'd; there is a trade off between pre-baked images and init scripts; storing sensitive information is key as well - the "NoEcho" attribute of a parameters is an important part of the solution; rolling updates

CloudFormation with Puppet - puppet master holds instruction and definitions; clients connect and follow instructions

CloudFormation with Chef - better for longer deployments; OpsWorks for detailed stuff like configure software, deploy apps, scale on demand and monitor resources for performance, security & cost. CloudFormation supports the following resource types: App, ElasticLoadBalancerAttachment, Instance, Layer & Stack.

CloudFormation with Elastic BeanStalk - Can use CloudFormation to deploy Elastic Beanstalk; less flexible than OpsWorks; better suited to immutable deployments

### Wait Conditions

Wait conditions should be used when synchronizing resources creation and there several different forms:

`DependsOn` - for the simple cases of ordering

`WaitConditions` - resources must signal the start and end wait period

`CreationPolicy` - simplified `WaitConditions` for EC2 instances and ASG

#### `DependsOn`

DependsOn is the CloudFormation template engine is smart enough to figure out many dependencies but in some cases resources require a `DependsOn` attribute. A VPC-gateway, an Auto Scaling group, and IAM roles are all required to include a DependsOn block. A `DependsOn` block can take a single value or an array.  The problem is that `DependsOn` only checks to see if the resource has been created NOT if it is operationally complete... One nifty features of wait conditions is the ability to pass data to and from the created resource.

The lifecycle is straightforward - while waiting they are `CREATE_IN_PROGRESS` then they are rolled back if they get a `CREATE_FAILED`. The wait condition has three properties:

- Count - how many signals must be received to make the 

- Handle - a `Ref` to the wait handle

- Timeout - the time out period in seconds

Should not be used for EC2 instance or autoscaling groups... use creation policies or wait conditions for these resources and use cases.

### Creation Policies

When waiting for an EC2 instance or ASG to setup, a `CreationPolicy` should be used. Creation policies are essentially simplified wait conditions. When the resource spins up it signals the stack using helper scripts, the `SignalResource` API or an API call.

```yaml
CreationPolicy:
  AutoScalingCreationPolicy: #only for autoscaling groups or EC2 instances
    MinSuccessfulInstancesPercent: 22
  ResourceSignal:
    Count: Integer # default = 1
    TimeOut: String # ISO8601 format; PT1H30M10S = 1h 30m 10s
```

#### Wait Condition Setup

For resources other than EC2 instances OR Auto Scaling groups, a wait condition is what you want. The resource must signal the start and end of the wait time out period. 

First, create a wait handler (one per wait condition)

```yaml
myWaitHandler:
  Type: AWS::CloudFromation::WaitConditionHandle
  Properties:
```
Second, create a `WaitCondition` resources using the `DependsOn` attribute to start the timeout after that resource has been completed.

```yaml
myWaitCondition:
  Type: AWS::CloudFormation::WaitCondition
  DependsOn: myEc2Instance
  Properties:
    Handle:
      Ref: MyWaitHandler
        Timeout: 4500
        Count: 2
```
Third, signal the start of the wait condition.

```yaml
myEc2Instance:
  Properties:
    UserData:
      fn::Base64
      # more stuff...
```

Fourth, signal the end of the wait condition or failure from the EC2.

## Rollback

If a CloudFormation template run does not complete successfully then by default it all gets rolled back, which feels like something very similiar to a transaction. In a nested stacks scenario, a parent stack roll-back will triggger a roll-back child stacks as well. 

When a roll back occurs first you might see a `CREATE_FAILED` message the likely a `ROLLBACK_IN_PROGRESS` message in the CF log. Rollbacks can be disabled to assist in troubleshooting.

### Rollback Failure

When a dependent resource cannot be returned to its original state the rollback will fail and report `UPDATE_ROLLBACK_FAILED`. This sort of thing can be causes by resource signals failing, changes to resources created by the stack, insufficient premissions, system limits (like max EC2 instances) or security problem (insufficient permissions or security token).

To fix this failed rollback, either fix and continue rolling back using the `continue-update-rollback` as documented [here](http://docs.aws.amazon.com/cli/latest/reference/cloudformation/continue-update-rollback.html) or [skip the resources](http://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_ContinueUpdateRollback.html) using the `ResourcesToSkip` parameter of `continue-update-rollback`

## Updates

Think if the update will cause downtime before you do it. Is the change mutable or immutable? Generally you will see a `UPDATE_IN_PROGRESS` then an `UPDATE_COMPLETE` once the update is complete. Resource meta updates are controlled by the cfn-hub daemon, which runs every 15 minutes by default.

Change sets are incremental changes to an existing stack. This allows you to preview the changes to the infrastructure from a template change.

### Stack Policy

After you create a stack, by default anyone can update the stack. A [Stack Policy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html) can be used restrict access to stack updates, a stack policy can be applied, which, by default, protects all the resources in the stack. You have to explictly `Allow` updates; only one stack policy per stack; many resources per [Stack Policy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html).

Stack policices can be applied at stack creation or updated to contain the policy. When a resource is updated it might be replaced or have no service interuption, suffer an interuption or be deleted. If you need to update a protected resource, you can temporarily replace the policy at update time.

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

The `Action` attribute can be set to `Update:Modify`, `Update:Replace`, `Update:Delete` or `Update:*` and the resource can use the `NotResource` for inverted logic.

## Deleting

By default when a stack is deleted all the resources are deleted. To get around this you can create a [DeletionPolicy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-deletionpolicy.html). See above.delet 

## EC2 Instances

Some aspects of EC2 instances are mutable... which is basically the instance type and other aspects of instance that allow it to continue on the same host. Change the AMI or the virtualization type and we are in mutable land and the instance must be fully replaced. 

To deploy and update applications and other required system components on EC2 instances easily there are a number of help scripts that can be used.

### cfn-init

cfn-init get run in the user data section of an instance, where it reads the template metadata from the `AWS::CloudFormation::Init` key, which is a declarative configuration block, then installs packages, writes files, enables/disable and start/stop services, creates users/groups and sources (for app code from git or s3). Files are backed up prior to overwrite. 

#### configSets

Inside the instance meta-data section use `AWS::CloudFormation::Init` to set config that the cfn-init process will use to configure packages, groups, users, sources, commands and enable/disable service. To add additional flexibility, use `ConfigSets` to order config tasks in the template for the cfn-init process. Something like:

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

Sends back messages to stack to signal success or failure. The cfn-signal helper script signals whether an EC2 instance has been successfully created or updated. This script is used with a `CreationPolicy` or an Auto Scaling group with a `WaitOnResourceSignals` update policy. When CloudFormation creates or updates resources with those policies, the rest of the stack pauses until the resource receives the required number of success signals, or until the timeout period is reached. The cfn-signal helper is what sends those signals back - successful or not.

### cfn-hup

Used to update in place using a deamon that detects resource metadata change and takes actions on those changes. A common use case has cfn-hup Not useful in updating autoscaling groups because there is no synchronization between the instances update timing leaving the ASG fleet in an odd configuration.

### cfn-get-metadata

Gets a metadata block from CloudFormation and prints it out to STDOUT.

## Auto Scaling Groups

To create a wait condition for an ASG, you can use a [`CreationPolicy`](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-creationpolicy.html). On the instance side, you have to signal the start and end of the wait time.

### Update Policies

An [Update Policy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-updatepolicy.html) handles updates to `AWS::AutoScaling::AutoScalingGroup` resources.

AutoScalingReplacingUpdate and AutoScalingRollingUpdate get applied when the AutoScaling group's launch configuration changes, there is a change to the associated subnets (VPCZoneIdentifiers) or when the instances in the ASGroup don't match the current launch configuration.

In addition, if `WillReplace: true` and both AutoScalingReplacingUpdate and AutoScalingRollingUpdate are specified AutoScalingReplacingUpdate takes precendents.

#### AutoScalingReplacingUpdate Policy

In the case of AutoScalingReplacingUpdate, if the `WillReplace` attribute is true the ASGroup AND the instances in that groups are replaced.  During the update the existing AutoScaling group is used and later destroyed AFTER the update is complete. To make this scenario work well, a `CreationPolicy` should also be specified so the new AutoScaling group is prepared prior to cut over. As an example:

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

AutoScalingRollingUpdate would be appropriate in the case where the AMI and app both need updates. This is a way better way of updating a fleet instead of using `cfn-init` and `cfn-hub` because rolling updates can be inconsistent.

#### AutoScalingScheduledAction Policy

In the case where updates to stacks might occur during AutoScaling group policy scheduled actions, you can specify an AutoScalingScheduledAction to prevent changes during an update. In other words, we don't our regularly time scheduled updates to the AutoScaling group's size properties to get overwritten by an update... here is how to do that:

```yaml
UpdatePolicy:
  AutoScalingScheduledAction:
    IgnoreUnmodifiedGroupSizeProperties: true
```

## Triage

### Best Practices

* Don't hardcode names, passwords or usernames in template as this causes portability issues; use parameters and `!Ref`  

* When using the `Fn::GetAZs` use `0` or `1` or `3` to maximize portability

### Troubleshooting

* `DELETE_FAILED` - lack permissions, S3 buckets must be empty before they can be deleted by CF, can use the RetainResources parameter to unstick the stack.

* Dependency error - can happen in both create and delete scenarios - and generally can be resolved using the `DependsOn` attribute... so delete the EC2 instance prior to deleting the VPC and add the IGW before adding the IEP; CF will error if there is a recursive or cross reference

* Insufficient IAM permissions - need permissions to run CF and the resources it creates, modifies or deletes.

* Rollback fail? Nested stacks have dependencies that keep rollbacks from occuring OR might not be getting signals on resource creation OR the resource has been modified in a way the keeps it from being deleted.  

* You can't stop and start an instance to modify its AMI; you must replace the instance; CF deals with instance ID changes.

* Invalid Value or Unsupported Resource Property - use parameter types to eliminate this problem with parameters

### Wait Conditions


| Wait for  | Use  | Notes  |
|:--------------------------:|:--------------------------:|:--------------------------:|
| ASG Completion    |  CreationPolicy  | requires use of `cfn-signal` or 'SignalResource'  |
| EC2 (standalone)    |  CreationPolicy  | requires use of `cfn-signal`   |
| VPC Gateway Attachment      |  `DependsOn`  | anything with a public IP requires attachment  |
| ECS Service      |  `DependsOn`  | autoscaling group or container instances required first |
| Ordering |  `DependsOn`  | but not for ASG or EC2; requires use of `cfn-signal`   |
| More than one resource type | `WaitCondition` | Anything not an AST or EC2 instance |


## Reading

* [AWS CloudFormation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

* [CloudFormation Template Reference](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html)

* [CloudFormation Troubleshooting Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html)