---
layout: post
title: "AWS Project - Auto Scaling"
description: ""
category: projects
tags: [aws, how-to]
---
{% include JB/setup %}

## What is Auto Scaling?
Autoscaling is magical stuff... gone are the days of fixed number of servers sized to meet the maximum demand.

## How to Configure Auto Scaling for Linux
There are four basic steps to creating an auto-scaling group:

1. Create Launch Configuration

2. Create AutoScaling Group

3. Create Notifications

4. Create Scaling Policies

## Best Practices

- Realize the minimum unit of cost is one hour - once a new instance it should stay on the entire hour...

- Load Balancing spillover and queue - most times instance CPU utilization are good but not perfect metrics for autoscaling; ELB load balancing queue and spill over are better metrics

- Set an autoscaling cool down period - Realize that scaling up and putting systems in service takes time; by setting an autoscaling cool down period giant teardowns right after build ups make sense.

## Resources
[Auto Scaling Quick Reference](http://awsdocs.s3.amazonaws.com/AutoScaling/latest/as-qrc.pdf)

[AWS Tutorial: Set Up a Scaled and Load-Balanced Application](http://docs.aws.amazon.com/autoscaling/latest/userguide/as-register-lbs-with-asg.html)
