---
title: "ECS"
tags:
- aws
- aws-compute
- containers
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Fargate](/notes/aws/fargate.md)

## #ecs- Elastic Container Service

### Avoid ECS Downtime
- [StackOverflow: Restart ECS service with no downtime](https://stackoverflow.com/questions/42735328/aws-ecs-restart-service-with-the-same-task-definition-and-image-with-no-downtime)
To avoid downtime, you should manipulate 2 parameters: _minimum healthy percent_ and _maximum percent_:
> For example, if your service has a desired number of four tasks and a maximum percent value of 200%, the scheduler may start four new tasks before stopping the four older tasks (provided that the cluster resources required to do this are available). The default value for maximum percent is 200%.

Regardless of whether your task definition changed and to what extent, there can be an "overlap" between the old and the new tasks, and this is the way to achieve resilience and reliability.
```
aws ecs update-service --force-new-deployment --service my-service --cluster cluster-name
```

---

### Secret Injection
- Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in either AWS [[secrets-manager]] or AWS [[systems-manager]]. Parameter Store parameters and then referencing them in your container definition. This feature is supported by tasks using both the EC2 and Fargate launch types.

---

### FireLens
- [FireLens](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_firelens.html) - Custom Log Routing
- You can use FireLens for Amazon ECS to use task definition parameters to route logs to an AWS service or AWS Partner Network (APN) destination for log storage and analytics. FireLens works with Fluentd and Fluent Bit. We provide the AWS for Fluent Bit image or you can use your own Fluentd or Fluent Bit image.

Creating Amazon ECS task definitions with a FireLens configuration is supported using the AWS SDKs, AWS CLI, and AWS Management Console.

---

### ECS Exec
- [ESC Exec](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-exec.html) to run commands in or get a shell to a container running on an Amazon EC2 instance or on AWS Fargate. This makes it easier to collect diagnostic information and quickly troubleshoot errors. For example, in a development context, you can use ECS Exec to easily interact with various process in your containers and troubleshoot your applications. And, in production scenarios, you can use it to gain break-glass access to your containers to debug issues.

### Misc.
- [ECS Troubleshooting Page](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/troubleshooting.html)
- [ecs vs fargate](https://cloudonaut.io/ecs-vs-fargate-whats-the-difference/)