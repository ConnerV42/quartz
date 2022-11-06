---
title: "AWS Lambda"
tags:
- aws
- aws-compute
- serverless
- lambda
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Lambda**
- AWS Lambda lets you run code without provisioning or managing servers. You pay only for the compute time you consume. You can run code for virtually any type of application or backend service - all with zero administration. Just upload your code, and Lambda takes care of everything required to run and scale your code with high availability. You can setup your code to automatically trigger from other AWS services or call it directly from any web or mobile app.
	- A nice feature about Lambda, it handle multiple requests. All of the docs specifically mention loading static resources/db connections/etc outside the response handler so they'll be reused between requests.
- The first time you invoke your function, AWS Lambda creates an instance of the function and runs its handler method to process the event. When the function returns a response, it stays active and waits to process additional events. If you invoke the function again while the first event is being processed, Lambda initializes another instance, and the function processes the two events concurrently. As more events come in, Lambda routes them to available instances and creates new instances as needed. When the number of requests decreases, Lambda stops unused instances to free up the scaling capacity for other functions.
- Your functions' concurrency is the number of instances that serve requests at a given time. For an initial burst of traffic, your functions' cumulative concurrency in a Region can reach an initial level of between 500 and 3000, which varies per Region.
- Lambda can scale faster than the regular Auto Scaling feature of EC2, Elastic Beanstalk, or ECS. This is because Lambda is more lightweight than other computing services. Under the hood, Lambda can run your code to the thousands of available AWS-managed EC2 instances (that could already be running) within seconds to accommodate traffic. This is faster than the Auto Scaling process of launching new EC2 instances that could take a few minutes or so. An alternative is to overprovision your compute capacity but that will incur significant costs.