---
layout: post
title: "AWS Project - Auto Scaling"
description: ""
category: projects
tags: [aws, how-to]
---
{% include JB/setup %}

## What is Auto Scaling?



## Why Would You Implement Auto Scaling?


## How to Configure Auto Scaling for Linux
Create Launch Configuration
Create AutoScaling Group
Create Notifications
Create Scaling Policies

## Best Practices

Realize that scaling takes time
Realize the minimum unit of cost is one hour
Load Balancing spillover and queue 


## Resources
[Auto Scaling Quick Reference](http://awsdocs.s3.amazonaws.com/AutoScaling/latest/as-qrc.pdf)

[AWS Tutorial: Set Up a Scaled and Load-Balanced Application](http://docs.aws.amazon.com/autoscaling/latest/userguide/as-register-lbs-with-asg.html)


ElasticLoadBalancer, qls-93161-ElasticL-8134VG593OES
AMIId, ami-f0091d91
KeyName, qwikLABS-L251-931614
AvailabilityZone, us-west-2c
SecurityGroup, qls-931614-14d21d581ba0cff9-Ec2SecurityGroup-1ICBYH77L1692

aws autoscaling create-launch-configuration --image-id ami-f0091d91 --instance-type t2.micro --key-name qwikLABS-L251-931614 --security-groups qls-931614-14d21d581ba0cff9-Ec2SecurityGroup-1ICBYH77L1692 --user-data file:///home/ec2-user/as-bootstrap.sh --launch-configuration-name lab-lc

aws autoscaling create-auto-scaling-group --auto-scaling-group-name lab-as-group --availability-zones us-west-2c --launch-configuration-name lab-lc --load-balancer-names qls-93161-ElasticL-8134VG593OES --max-size 5 --min-size 1

arn:aws:sns:us-west-2:368816883184:lab-as-topic

aws autoscaling put-notification-configuration --auto-scaling-group-name lab-as-group --topic-arn arn:aws:sns:us-west-2:368816883184:lab-as-topic --notification-types autoscaling:EC2_INSTANCE_LAUNCH autoscaling:EC2_INSTANCE_TERMINATE


aws autoscaling put-scaling-policy --policy-name lab-scale-up-policy --auto-scaling-group-name lab-as-group --scaling-adjustment 1 --adjustment-type ChangeInCapacity --cooldown 300



