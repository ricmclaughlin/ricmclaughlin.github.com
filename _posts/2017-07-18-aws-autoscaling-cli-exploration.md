---
layout: post
title: "aws autoscaling cli exploration"
description: ""
category: projects
tags: []
---
{% include JB/setup %}


To full get a better understanding of the a

| API      |  Action     |  Purpose|
|----------|---------------|---------------|
| `enter-standby` | puts instance in standby | Pause the instance for maintenance  |
| `exit-standby` | takes instance out of standby |  Unpause the instance for maintenance      |
| `create-launch-configuration` | create one | like UI make this first   |
| `delete-launch-configuration` | create one  | delete (there is no modify) |
| `update-auto-scaling-group` | update an existing ASG  | Update ASG |
| `update-auto-scaling-group` | update an existing ASG  | Update ASG |
| `put-lifecycle-hook` | add hook to pending or termination | event hook |
| `put-scaling-policy` | adds an autoscaling policy | create scaling policy |