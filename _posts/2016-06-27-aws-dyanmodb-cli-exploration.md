---
layout: post
title: "AWS - DyanmoDB CLI Exploration"
description: ""
category: projects
tags: [aws, dynamodb, aws-dev-ops-pro, aws-solutions-arch-pro, cli-exploration]
---
{% include JB/setup %}

This is a very simple exploration of [AWS DynamoDB](https://aws.amazon.com/dynamodb/developer-resources/) from the [AWS CLI](https://aws.amazon.com/cli/). To get started with the CLI first [install it](http://docs.aws.amazon.com/cli/latest/userguide/installing.html), then use `aws configure` to [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for your account and region. 

## DynamoDB API Reference points

### Table

| **DynamoDB API (Table)**  | **Notes**  |
|:-----------------------------------------|:--------------------------------------------------------|
| CreateTable | Creates a new table. Optionally, you can create one or more secondary indexes, and enable DynamoDB Streams for the table. |
| DescribeTable | Returns information about a table, such as its primary key schema, throughput settings, index information, and so on. |
| ListTables | Returns the names of all of your tables in a list. |
| UpdateTable | Modifies the settings of a table or its indexes, creates or remove new indexes on a table, or modifies DynamoDB Streams settings for a table.(used to change the required provisioned throughput capacity.) |
| DeleteTable | Removes a table and all of its dependent objects from DynamoDB. |
 
## Item

| **DynamoDB API (CRUD)**  | **Notes**  |
|:-----------------------------------------|:--------------------------------------------------------|
| PutItem | Writes a single item to a table. You must specify the primary key attributes, but you don't have to specify other attributes. |
| BatchWriteItem | Can write to one table or delete from one or more tables  |
| GetItem | You must specify the primary key for the item that you want. You can retrieve the entire item, or just a subset of its attributes - charged for the entire record. |
| BatchGetItem | Retrieves up to 100 items from one or more tables. This is more efficient than calling GetItem multiple times because your application only needs a single network round trip to read the items. |
| UpdateItem | You can also perform conditional updates, so that the update is only successful when a user-defined condition is met. Optionally, you can implement an atomic counter, which increments or decrements a numeric attribute without interfering with other write requests.|