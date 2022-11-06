---
title: "Simple Notification Service"
tags:
- aws
- sns
- serverless
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Simple Notification Service (SNS)**
- SNS is a fully managed pub/sub messaging service. With Amazon SNS, you can use topics to simultaneously distribute messages to multiple subscribing endpoints such as Amazon SQS queues, AWS Lambda functions, HTTP endpoints, email addresses, and mobile devices (SMS, Push)
- Messages in an SQS queue will continue to exist even after the EC2 instance has processed it, until you delete that message. You have to ensure that you delete the message after processing to prevent the message from being received and processed again once the visibility timeout expires.
- A fanout scenario occurs when a message published to an SNS topic is replicated and pushed to multiple endpoints, such as Amazon SQS queues, HTTP(S) endpoints, and Lambda functions. This allows for parallel asynchronous processing. For example, you can develop an application that publishes a message to an SNS topic whenever an order is placed for a product. Then, 2 or more SQS queues that are subscribed to the SNS topic receive identical notifications for the new order An EC2 server instance attached to one of the SQS queues can handle the processing or fulfillment of the order. And you can attach another EC2 server instance to a data warehouse for analysis of all orders received. By default, an SNS topic receives every message published to the topic. You can use SNS message filtering to assign a filter policy to the topic subscription, and the subscriber will only receive a message that they are interested in. Using SNS and SQS together, messages can be delivered to applications that require immediate notification of an event. This method is known as fanout to Amazon SQS queues.
- If you are after parallel asynchronous processing, you'll need to fan-out messages to multiple SQS queues, using a filter policy in SNS subscriptions to allow parallel asynchronous processing. If you don't set a filter policy in SNS, the subscribers would receives all the messages published to the SNS topic.
- A "fanout" pattern is when an Amazon SNS message is sent to a topic and then replicated and pushed to multiple Amazon SQS queues, HTTP endpoints, or email addresses. This allows for parallel asynchronous processing. For example, you could develop an application that sends an Amazon SNS message to a topic whenever an order is placed for a product. Then, the Amazon SQS queues that are subscribed to that topic would receive identical notifications for the new order. The Amazon EC2 server instance attached to one of the queues could handle the processing or fulfillment of the order, while the other server instance could be attached to a data warehouse for analysis of all orders received.
	- When a consumer receives and processes a message from a queue, the message remains in the queue. Amazon SQS doesn't automatically delete the message. Because Amazon SQS is a distributed system, there's no guarantee that the consumer actually receives the message (for example, due to a connectivity issue, or due to an issue in the consumer application.) Thus, the consumer must delete the message from the queue after receiving it and successfully processing it.
	- Immediately after the message is received, it remains in the queue. To prevent other consumers from processing the message again, Amazon SQS sets a _visibility timeout_, a period of time during which Amazon SQS prevents other consumers from receiving and processing the message. The default visibility timeout for a message is 30 seconds. The maximum is 12 hours.