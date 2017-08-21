---
layout: post
title: "IoT on AWS Course Development"
description: ""
category: posts
tags: [aws-iot, curriculum-dev]
---
{% include JB/setup %}

Perhaps my favorite curriculum development project to date was "IoT on AWS" a course I developed for [Level Education](https://www.leveledu.com/) initially offered as part of their [IoT bootcamp](https://pages.northeastern.edu/IoT.html program). 

The high level educational goal of the course is to teach IoT students that are tech savvy and hardware focused enough of the AWS IoT service to get started on their own... in two 3-hours sessions. The course clearly had to include lots of the "what and why" of AWS IoT, a good definitional desciption of numerous system components and how they fit together, one or more hands-on activities that included hooking up a device to the AWS service and a good overview of what other sorts of problems the student to look to solve with AWS IoT. That's ambitious. And challenging. And would make a great course if I could pull it off. 

## Overall Development Approach

The course was also a test bed for the best practices I've learned espoused over the last couple of years: 

- A thorough walk through to the overall, problem the technology solves, why it is important to solve and alternatives on how to solve it

- Develop each course into smaller learning modules that can be recomposed into different courses and promote easy reuse

- Specific, clearly enunciated learning objectives at the beginning of the course and learning module level that are defined in the curriculum development process at the onset

- Courseware that is a "we do; you do; you extend" model, akin to a walk through but on steriods and building a checklist along the way

- Check for understanding using quizes embedded in the curriculum

- At the end of each learning module, evaluate the result and demonstrate solutions evaluation then iteratively improve the result of what we just did in the next learning module as a segue to the next learning module

- Present chances for extention and additional learning at the end of each classroom session

- Enable courseway to be easily adapted to a face-to-face, live-virtual, or self-paced e-learning environment

And this course pretty much succeeds on these standards.... after the second iteration. The big change from the first iteration was the switch from the an  to the AWS IoT button to enable easier setup and integration with AWS. 

I used the super cool [Talent LMS](https://www.talentlms.com/) for hosting the content in a self paced learner format and also use the quiz development capability of the platform for learning assessments... which are old school style quizzes. 

## Learning Objectives by Learning Module

### Course Learning Objectives

- Explain the AWS IoT service architecture including the Message Broker, Rules Engine, Thing Shadow and Thing Registry

- Demonstrate AWS Account setup in a low-cost, low-risk, & secure manner

- Configure an AWS IoT button on the AWS IoT service and make a button push send an SMS

- Implement AWS IoT service communication, rules and system interaction, debugging, and monitoring tools

- Explain advanced AWS IoT use cases and implement a mock thing and persist data

### AWS IoT Architecture Module Learning Objectives

- List and Describe the function of: Message Broker, Device Shadow, Rules Engine, Security & Identity Service, Thing Registry

- Describe the purpose and relationship between Device Certs, Verisign CA Certs, and Policies 

### AWS Account Setup Module Learning Objectives

Develop and execute a Secure Account Checklist including

- Stop using the Root Account

- Use an Admin account in an Admin Group

- Enable Multi-factor authentication

- Setup a Password Specification

- Set a billing alarm and enable admin access to billing information

Explain the different between the AWS 12 Month Free and the Always Free tiers

### AWS IoT Button Setup Module Learning Objectives

- Demonstrate Securely Connecting a Button to AWS IoT

- Configure Your IoT Button to Send an SMS using a Node.js Lambda Function

### Extending IoT on AWS Module Learning Objectives

- Demonstrate IoT Web Console Navigate

- Demonstrate IoT Thing Shadow and Cloudwatch Debugging

- Use AWS services to solve IoT requirements

1. Gather fleet metrics using Cloudwatch

2. Mock a thing using the MQTT client

3. Model and control state of thing with device shadow

4. Store and process unstructured data with S3 & SQS

5. Store structured data in DynamoDB

## Wanna take a look?

This IoT on AWS is [available online](https://iotonaws.talentlms.com) at Talent LMS - contact me at <mailto:ric@mclaughlin.today> for access.
