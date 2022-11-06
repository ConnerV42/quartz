---
title: "AWS Step Functions"
tags:
- aws
- serverless
- lambda
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Lambda](/notes/aws/lambda.md)
- [AWS SWF](/notes/aws/aws-swf.md)

### Helpful Links
- [AWS Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/)

## **Step Functions**
- AWS Step Functions provides serverless orchestration for modern applications. Orchestration centrally manages a workflow by breaking it into multiple steps, adding flow logic, and tracking the inputs and outputs between the steps. As your applications execute, Step Functions maintains application state, tracking exactly which workflow step your application is in, and stores an event log of data that is passed between application components. That means that if networks fail or components hang, your application can pick up right where it left off.
- Application development is faster and more intuitive with Step Functions, because you can define and manage the workflow of your application independently from its business logic. Making changes to one does not affect the other. You can easily update and modify workflows in one place, without having to struggle with managing, monitoring and maintaining multiple point-to-point integrations. Step Functions frees your functions and containers from excess code, so your applications are faster to write, more resilient, and easier to maintain.

### What Limitations of Lambda does Step Functions address?
- Lambda is FaaS (Functions as a Service)
- Lambda has a 15 minute max execution time
	- Although Lambdas can be chained, this gets messy at scale
- Lambda Runtime Environments are stateless

### State Machines - Coordinates the Work Occurring
- A State Machine is a fancy term for a serverless workflows
- You have a START -> multiple STATES in between -> and an END
- States are the THINGS inside these workflows
- Maximum Duration for a state machine is 1 year (Standard Workflow) or 5 minutes (Express Workflow)
- Standard Workflow (default) and Express Workflow (High volume, IoT, streaming data)
- Can be started via API Gateway, IOT Rules, EventBridge, Lambda, or Manually
- You can create, and export state machines to your liking by using a templating language called Amazon States Language (ASL) - JSON Template
- IAM Roles are used for permissions

### State Types
- SUCCEED
- FAIL
- WAIT - Waits a certain period of time, or until a specific date and time
- CHOICE - Allows the State Machine to take a different path, depending on an input. A fork in the road.
- PARALLEL - Allows for parallel branches within the State Machine
- MAP - Accepts a list of things, like orders, for example. Performs a set of actions for each order in the list.
- TASK - A single unit of work performed by a state machine. This can be integrated with Lambda, Batch, DynamoDB, ECS, SNS, SQS, Glue, SageMaker, EMR, Step Functions