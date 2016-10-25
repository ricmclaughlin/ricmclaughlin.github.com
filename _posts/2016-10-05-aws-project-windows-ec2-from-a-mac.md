---
layout: post
title: "AWS Project - Windows EC2 from a Mac"
description: ""
category: projects
tags: [aws, ec2, how-to]
---
{% include JB/setup %}

First, I gotta say is that windows server.. I'm rusty at it. The idea that I would paste my public key into a dialog box to get my admin password is completely foriegn to me and that feels really great. After spending most of my professional career working on Windows, I can finally say that the Koolaid is out of my system! 

But that isn't what this lab write up is about. Creating EC2 instances with Windows AMIs is the topic. 

Creating an EC2 instance with a Windows AMI is just like creating an EC2 instance with a Linux AMI... access the AWS console, click `Launch Instance` then select the "Windows 2012 r2" AMI and use the trust t2.micro instance type and you are up and running quickly. Make sure you enable port 3389 in the security group so you can access your instance once it is up and running.

One bit of minutiae is that Windows also does not use bash - or not natively at this point - so post launch scrips need to be in powershell. I wrote a tiny, little one like this to install IIS.

`<powershell>Install-WindowsFeature Web-Server -IncludeManagementTools -IncludeAllSubFeature</powershell>`

Attaching to your new instance is a bit harder - obviously you can't `ssh` into it! You need an RDP client - which is easy. Our friends at Microsoft [make one](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12). 

Getting the administrator password for your instance requires you paste the contents of your .pem file into connection dialog.

From here you are up and running! 

## Learnings
1. Don't run Windows on AWS.

2. I like Windows less than I did when I was a MCSD and MCSE years ago!

<img width="250" src="{{ BASE_PATH }}/assets/themes/ricify/images/mcp.jpg" alt="Ric's MCP card">



