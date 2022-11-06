---
title: "CloudTrail"
tags:
- aws
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **CloudTrail**
- Almost everything that can be done to an AWS account is logged by CloudTrail
- Logs API calls/account activities as a CloudTrail Event
- Actions taken by users, roles or services
- 90 days stored by default in Event History, S3 CloudTrail logs storage are not included by default
- Enabled by default - no cost for 90 day history
- To customize the service, create 1 or more Trails
- CloudTrail Logs are encrypted by default with Amazon S3 server-side encryption
- CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account. With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. By default, CloudTrail is enabled on your AWS account when you create it. When activity occurs in your AWS account, that activity is recorded in a CloudTrail event. You can easily view recent events in the CloudTrail console by going to Event history. CloudTrail provides event history of your AWS account activity, including actions taken through AWS Management console, AWS SDKs, command line tools, API calls, and other AWS services. This event history simplifies security analysis, resource change tracking, and troubleshooting.
- CloudTrail cannot be used for real-time logging, there can be up to a 15 minute delay

### Management Events
- **Management Events** provide visibility into management operations that are performed on resources in your AWS account. These are also known as control plane operations. Management events can also include non-API events that occur in your account.
- Things such as creating an EC2 instance, creating a VPC, terminating an EC2 instance
- These are logged by CloudTrail by default

### Data Events
- **Data Events**, on the other hand, provide visibility into the resource operations performed on or within a resource. These are also known as data plane operations. It allows granular control of data event logging with advanced event selectors. You can currently log data events on different resource types such as Amazon S3 object-level API activity (e.g. GetObject, DeleteObject, and PutObject API opeations), AWS Lambda function execution activity (the Invoke API), DynamoDB Item actions, and many more.
- Data Events are much higher volume than Management Events, and are not logged by default

### CloudTrail Trail
- Unit of configuration within the CloudTrail product
- This is how you provide configuration to CloudTrail on how to operate
- Logs events for the AWS region that it is created in, in the case of a One Region Trail, but an All Region Trail can be created
- Most services log regions in the region that it was created in
- Global Service Events is an option that needs to be enabled, if you want to log events from global services, such as IAM, STS and CloudFront.
- When a Trail is created, you can store logs indefinitely in S3 as compressed JSON files. These files are parsable by any tools that parse JSON.
- Logs can also be stored in CloudWatch
- You can create an Organizational Trail, which is a single management point for all accounts in that Organization. It makes managing multi-account environments much easier.