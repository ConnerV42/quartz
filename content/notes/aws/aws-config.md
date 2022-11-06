---
title: "AWS Config"
tags:
- aws
- aws-security
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## AWS Config
- Primary function is to record configuration changes over time (configuration items) on resources, and grouping this information into configuration histories
- Auditing of changes, compliance with standards
- Does not prevent changes from happening, no protection
- Regional service, supports cross-region and account aggregation (but not by default)
- Changes can generate SNS notification and near-realtime events via EventBridge and Lambda

- Config Rules can be created, which your resources are evaluated against. You can either use AWS Managed or custom (using Lambda).

- **AWS Config** is a service that enables you to assess, audit and evaluate the configurations of your AWS resources. Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations. With AWS Config, you can review changes in configurations and relationships between AWS resources, dive into detailed resource configuration histories, and determine your overall compliance against the configurations specified in your internal guidelines. This enables you to simplify compliance auditing, security analysis, change management, and operational troubleshooting.
- Primary function is to record changes over time on resources within an AWS account (great for auditing and compliance)
- It does *not* prevent changes from happening, but can be used for automatic remediation
- It's a regional service that does support cross-region and account aggregation
- Changes can generate SNS notifications and near real-time events via EventBridge and Lambda
- When enabled, AWS Config will record changes in a S3 bucket called a Config Bucket, but you can do a lot more with the product.
- Instead of the default setup outlined above, you can set up config rules which use AWS Lambda to evaluate whether or not a change is compliant or non-compliant.
- AWS Config can also be integrated with EventBridge, which can invoke Lambda function for automatic remediation.
- AWS Config can integrate with Systems Manager as well

## #sysops scenarios
**Question**: A company has a tagging strategy for controlling access to Amazon EC2 across their AWS Organization units. The system administrator noticed that some tags do not follow the company's naming convention which causes permission issues.

Which solution can help the administrator identify the affected resources with non-compliant tags?

**Answer**: Set up the `require-tags` managed rule in AWS Config. You can assign metadata to your AWS resources in the form of tags. Each tag is a label consisting of a user-defined key and value. Tags can help you manage, identify, organize, search for, and filter resources. You can create tags to categorize resources by purpose, owner, environment, or other criteria.

You can use tags to control access by restricting IAM permissions based on specific tags or tag values. For example, IAM user or role permissions can include conditions to limit EC2 API calls to specific environments (such as development, test, or production) based on their tags.