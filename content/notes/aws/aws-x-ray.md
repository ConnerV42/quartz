---
title: "AWS X-Ray"
tags:
- aws
- aws-security
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **X Ray**
- Distributed tracing application
- Tracing Header - first service generates .. it's unique (traceID), used to track a request through your distributed application
- Segments - Data blocks - host/ip, request, response work done (times), issues
- Subsegments - more granular version of the above, calls to other services as part of a segment (endpoint, etc.)
- Service Graph - JSON Document detailing services and resources which make up your application
- Service Map - Visual version of the service graph showing traces
- EC2 Usage - X-Ray Agent is required
- ECS - Agent in tasks
- Lambda - enable option
- Beanstalk - agent preinstalled
- API Gateway - per stage option
- SNS & SQS
- Requires IAM Permissions

- You can use AWS X-Ray to trace and analyze user requests as they travel through your Amazon API Gateway APIs to the underlying services. API Gateway supports AWS X-Ray tracing for all API Gateway endpoint types: regional, edge-optimized, and private. You can use AWS X-Ray with Amazon API Gateway in all regions where X-Ray is available.
- X-Ray gives you an end-to-end view of an entire request, so you can analyze latency in your APIs and their backend services. You can use an X-Ray service map to view the latency of an entire request and that of the downstream services that are integrated with X-Ray. And you can configure sampling rules to tell X-Ray which requests to record, at what sampling rates, according to criteria that you specify. If you call an API Gateway API from a service that’s already being traced, API Gateway passes the trace through, even if X-Ray tracing is not enabled on the API.
- You can enable X-Ray for an API stage by using the API Gateway management console, or by using the API Gateway API or CLI.