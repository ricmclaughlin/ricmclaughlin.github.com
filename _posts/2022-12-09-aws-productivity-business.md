---
layout: post
title: "AWS - Productivity and Business Apps"
description: ""
category: posts
tags: [other, aws-solutions-arch-pro]
---
{% include JB/setup %}

# WorkSpaces
Workspaces is a secure cloud desktop service using Virtual Desktop Infrastructure (VDI) that integrates with Active Directory. Support Windows and Linux. Uses WorkSpaces Application Manager (WAM) to deploy and provision applications. All WorkSpace have Windows update turned on but you have full control over frequency. Updates are installed during the default maintenance window (Sunday 0-4 AM) or during a defined window; the AutoStop feature automatically restarts the WorkSpace to install updates.

To set up a cross region configuration, set up an AD connector in the failover region (because you can't multi-region Managed Microsoft AD) and use a Route53 Failover type TXT record to alias to the failover region. Using WorkDocs for storage enables a completely failover solution.

Workspaces also support IP Access Control groups which are similar to Security Groups.

# AppStream
AppStream enables applications to be streamed to any device without acquiring or provision infrastructure via a web browser.
