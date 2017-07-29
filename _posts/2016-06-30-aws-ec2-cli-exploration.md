---
layout: post
title: "AWS - EC2 CLI Exploration"
description: ""
category: projects
tags: [aws, ec2, aws-dev-ops-pro, aws-solutions-arch-pro, cli-exploration]
---
{% include JB/setup %}

This is a very simple exploration of [AWS EC2 Instances](https://aws.amazon.com/kinesis/streams/) from the [AWS CLI](https://aws.amazon.com/cli/). To get started with the CLI first [install it](http://docs.aws.amazon.com/cli/latest/userguide/installing.html), then use `aws configure` to [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for your account and region. 

Create volume, create an instance with user data, attach the volume to an instance, manipulate the instance, take a snapshot, copy the snapshot, create an AMI, register the AMI, copy the AMI.

## Create volume

### List Volumes

```bash
aws ec2 describe-volumes --region us-west-2
```

### Delete Volumes 

```bash
aws ec2 delete-volume --region us-west-1 --volume-id vol-0a0deda1b7aa09c35
```

#### Create Volume

Encrypt the volume using `–encrypted true`; optionally use `-–kms-key-id <arn>` to use KMS master key

```bash
aws ec2 create-volume --region us-west-2 --availability-zone us-west-2a --size 80 --volume-type gp2 --tag-specifications 'ResourceType=volume,Tags=[{Key=purpose,Value=production},{Key=cost-center,Value=mystuff}]'
```

#### Filtering the Volume List

```bash
aws ec2 describe-volumes --region us-west-2 --filters Name=tag-key,Values="purpose" Name=tag-value,Values="production"
```

## Create Instance

```bash
aws ec2 run-instances --image-id ami-6df1e514 --count 1 --instance-type t2.micro --key-name account-2 --security-groups the-new-SG --tag-specifications 'ResourceType=instance,Tags=[{Key=purpose,Value=production},{Key=cost-center,Value=mystuff}]'
```

### Describe Instances

```bash 
aws ec2 describe-instances
```

```bash 
aws ec2 describe-instances  --filters "Name=tag:Purpose,Values=production"
```

### Start/Stop/Terminate Instances

```bash 
aws ec2 stop-instances --instance-id i-07423f36098de7b0c
```

```bash 
aws ec2 stop-instances --instance-id i-07423f36098de7b0c
```

```bash 
aws ec2 terminate-instances --instance-id i-07423f36098de7b0c
```
EBS volumes attached at creation time will have the DeleteOnTerminate flag set to true, and any new ones will have it set to false

## Create AMI

If this is an EBS backed instance there is no need to register it...

```bash
aws ec2 create-image \
--instance-id i-07423f36098de7b0c  \
--name mygoodness-v1
```

--block-device-mappings can be specified for the image as well...

--no-reboot | --reboot (to insure file system integrity)

### Copy AMI to Another Region

```bash
aws ec2 copy-image --source-image-id ami-e0b3ab99 --source-region us-west-2 --region us-east-1 --name "mygoodness-v2"
```

Use --kms-key-id <key arn> to encrypt...

## Snapshot volumes

Encrypted volumes -> encrypted snapshots

```bash
aws ec2 create-snapshot --volume-id vol-08bf38a3b26ac47a4 
```


encrypted snapshots -> encrypted snapshot
unencrypted snapshot + `--encrypted` -> encrypted snapshot

```bash
aws ec2 copy-snapshot --source-region us-west-2 --source-snapshot-id snap-066877671789bd71b --description "This is my copied snapshot." --region us-east-1 
```

```bash
  delete-snapshot
```

## Handy stuff

### Find tagged resources of a resource type

```bash
aws ec2 describe-tags --region us-west-2 --filters Name=resource-type,Values="volume" Name=tag-key,Values="purpose" Name=tag-value,Values="production"
```

### Find tagged resources of any type

```bash
aws ec2 describe-tags --region us-west-2 --filters  Name=tag-key,Values="purpose" Name=tag-value,Values="production"
```

### Instance Meta-Data

There is a ton of meta-data available about each EC2 instance available via this [handy URL: curl http://169.254.169.254/latest/meta-data/](http://169.254.169.254/latest/meta-data/) from the instance itself.
