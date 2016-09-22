---
layout: post
title: "AWS Developer Certification - SNS"
description: ""
category: posts
tags: [aws, developercert, sns]
---
{% include JB/setup %}

AWS Simple Notfication Services is a pub-sub service - no polling required. SNS is a natural architectural match with SQS. SQS is used by distributed applications and can be used to decouple sending and receiving components without requiring each application component to be concurrently available. The basis of the system is a Topic.

1. Many services can publish to a SNS topic. Messages on a topic can be customized based on the subscribing service.

1. Many services can subscribe to an SNS topic - processing can be handled realtime by subscribing a Lambda function or webservice to the topic or processing can be handled in a loosely coupled, as available process, by subscribing a SQS queue to the topic. Protocols include HTTP/HTTPS, email, Email-JSON, SMS, SQS, AWS Lambda and application.

1. Subscriptions must be confirmed and will expires in 3 days if unconfirmed.

1. The SNS document format is very close to an HTTP request format - which makes a lot of sense seeing that you can send an SNS message to an HTTP/HTTPS endpoint. Here is the format of an SNS message:

```JSON
POST / HTTP/1.1
  x-amz-sns-message-type: Notification
  x-amz-sns-message-id: 22b80b92-fdea-4c2c-8f9d-bdfb0c7bf324
  x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
  x-amz-sns-subscription-arn: arn:aws:sns:us-west-2:123456789012:MyTopic:c9135db0-26c4-47ec-8998-413945fb5a96
  Content-Length: 773
  Content-Type: text/plain; charset=UTF-8
  Host: myhost.example.com
  Connection: Keep-Alive
  User-Agent: Amazon Simple Notification Service Agent
  {
    "Type" : "Notification",
    "MessageId" : "22b80b92-fdea-4c2c-8f9d-bdfb0c7bf324",
    "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
    "Subject" : "My First Message",
    "Message" : "Hello world!",
    "Timestamp" : "2012-05-02T00:54:06.655Z",
    "SignatureVersion" : "1",
    "Signature" : "EXAMPLEw6JRNwm1LFQL4ICB0bnXrdB8ClRMTQFGBqwLpGbM78tJ4etTwC5zU7O3tS6tGpey3ejedNdOJ+1fkIp9F2/LmNVKb5aFlYq+9rk9ZiPph5YlLmWsDcyC5T+Sy9/umic5S0UQc2PEtgdpVBahwNOdMW4JPwk0kAJJztnc=",
    "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem",
    "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-2:123456789012:MyTopic:c9135db0-26c4-47ec-8998-413945fb5a96"
  }
```
## Mobile Push
Mobile Push is a primary use case for SNS. Setup is required for each mobile platform.

1. Request credentials from mobile platform - get a login

2. Get request token from mobile platform - get a thingy that represents your app

3. Create a Platform Application Object - register it with SNS and get an object you can use

4. Create a Platform End Point - create an event handler that recieves the SNS message and routes it to the mobile platform


## Security Rules
By default, ALL requests to use a resource are denied - *default deny*.
All *allows* override any default denies.
All *explicit denies* override allows.

# Resources

## Reading
[SNS Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/welcome.html)

