---
title: "Simple Queue Service"
tags:
- aws
- serverless
- sqs
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Simple Queue Service** (Amazon SQS) 
- Amazon SQS is a message queue service used by distributed applications to exchange messages through a polling model. It can be used to decouple sending and receiving components without requiring each component to be concurrently available.
- SQS offers reliable, highly-scalable hosted queues for storing messages while they travel between applications or micro-services. SQS lets you move data between distributed applications components and helps you decouple these components. Amazon SWF is a web service that makes it easy to coordinate work across distributed application components.
- Standard queues provide at-least-once delivery, which means that each message is delivered at least once. Standard queues do not, however, preserve the order of messages. This feature only applies to FIFO queues.
- SQS uses short polling by default, querying only a subset of of the servers (based on a weighted random distribution) to determine whether any messages are available for inclusion in the response. Short polling works for scenarios that require higher throughput. However, you can also configure the queue to use long polling, to reduce cost.
- Take note that you cannot assign a priority to individual items in an SQL queue. If you want higher priority requests, you'll have to implement 2 separate SQS queues, side by side. One for premium members, that is polled first, and one for free members, which is polled if the premium queue is empty.
- Pairs well with Lambda in serverless stacks. Lambda supports the synchronous and asynchronous invocation of a Lambda function. You can control the invocation type only when you invoke a Lambda function. When you use an AWS service as a trigger, the invocation type is predetermined for each service.
- You can use an AWS Lambda function to process messages in an Amazon Simple Queue Service (Amazon SQS) queue. Lambda event source mappings support standard queues and first-in, first-out (FIFO) queues. With Amazon SQS, you can offload tasks from one component of your application by sending them to a queue and processing them asynchronously.
- Amazon SQS automatically deletes messages that have been in a queue for more than the maximum message retention period. The default message retention period is 4 days.
### **Consuming messages using short polling**:
- When you consume messages from a queue using short polling, Amazon SQS samples a subset of its servers (based on a weighted random distribution) and returns messages from only those servers. Thus, a particular ReceiveMessage request might not return all of your messages. However, if you have fewer than 1,000 messages in your queue, a subsequent request will return your messages. If you keep consuming from your queues, Amazon SQS samples all of its servers, and you receive all of your messages.
- The following diagram shows the short-polling behavior of messages returned from a standard queue after one of your system components makes a receive request. Amazon SQS samples several of its servers (in gray) and returns messages A, C, D, and B from these servers. Message E isn't returned for this request, but is returned for a subsequent request.
- Short polling occurs when the WaitTimeSeconds parameter of a ReceiveMessage request is set to 0 in one of two ways:
	- The `ReceiveMessage` call sets `WaitTimeSeconds` to 0.
	- The `ReceiveMessage` call doesn't set `WaitTimeSeconds`, but the queue attribute `ReceiveMessageWaitTimeSeconds` is set to 0.
### **Consuming messages using long polling**:
- When the wait time for the ReceiveMessage API action is greater than 0, long polling is in effect. The maximum long polling wait time is 20 seconds. Long polling helps reduce the cost of using Amazon SQS by eliminating the number of empty responses (when there are no messages available for a ReceiveMessage request) and false empty responses (when messages are available but aren't included in a response). For information about enabling long polling for a new or existing queue using the Amazon SQS console, see the Configuring queue parameters (console). For best practices, see Setting up long polling.
- Long polling offers the following benefits:
	- Long polling helps reduce your cost of using Amazon SQS by reducing the number of empty responses when there are no messages available in the queue before sending a response. Unless the connection times out, the response to the `ReceiveMessage` request contains at least one of the available messages, up to the maximum number of messages specified in the `ReceiveMessage` action.
	- Reduce empty responses by allowing Amazon SQS to wait until a message is available in a queue before sending a response. Unless the connection times out, the response to the ReceiveMessage request contains at least one of the available messages, up to the maximum number of messages specified in the ReceiveMessage action. In rare cases, you might receive empty responses even when a queue still contains messages, especially if you specify a low value for the ReceiveMessageWaitTimeSeconds parameter.
	- Reduce false empty responses by querying all—rather than a subset of—Amazon SQS servers.
	- Return messages as soon as they become available.
### ApproximateNumberOfMessages Metric
- The `ApproximateNumberOfMessages` metric in Amazon CloudWatch can scale out EC2 instances in your decoupled application components.
- The number of messages in your Amazon SQS queue does not solely define the number of instances needed. In fact, the number of instances in the fleet can be driven by multiple factors, including how long it takes to process a message and the acceptable amount of latency (queue delay).
- The solution is to use a *backlog per instance* metric with the target value being the *acceptable backlog per instance* to maintain. You can calculate these numbers as follows:
	- **Backlog per instance**: To determine your backlog per instance, start with the SQS metric `ApproximateNumberOfMessages` to determine the length of the SQS queue (number of messages available for retrieval from the queue). Divide that number by the fleet's running capacity, which for an Auto Scaling group is the number of instances in the `InService` state, to get the backlog per instance.
	- **Acceptable backlog per instance**: To determine your target value, first calculate what your application can accept in terms of latency. Then, take the acceptable latency value and divide it by the average time that an EC2 instance takes to process a message.
- Amazon SQS automatically deletes messages that have been in a queue for more than the maximum message retention period. The default message retention period is 4 days

### **ApproximateAgeOfOldestMessage** Metric
- The `ApproximateAgeOfOldestMessage` metric is useful when applications have time-sensitive messages and you need to ensure that messages are processed within a specific time period. You can use this metric to set Amazon CloudWatch alarms that issue alerts when messages remain in the queue for extended periods of time. You can also use alerts to take action, such as increasing the number of consumers to process messages more quickly. With a target tracking scaling policy, you can scale (increase or decrease capacity) a resource based on a target value for a specific CloudWatch metric. To create a custom metric for this policy, you need to use AWS CLI or AWS SDKs. Take note that you need to create an AMI from the instance first before you can create an Auto Scaling group to scale the instances based on the `ApproximateAgeOfOldestMessage` metric.