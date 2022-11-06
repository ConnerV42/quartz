---
title: "AWS Cost Explorer"
tags:
- aws
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## AWS Cost Explorer
- Capable of analyzing usage within the account, and giving recommendations about reserved instance purchases (if you have enough usage on the account)
- Cost Anomaly Detection: Help reduces cost with machine learning by tracking costs and usage
- Rightsizing Recommendations: Reviews historical EC2 usage to identify opportunities for greater cost and usage efficiency

### Cost Allocation Tags
- Cost allocation tags - have to be enabled individually
- (per account for standard accounts or ORG Master for ORGS)
- AWS Generated - e.g. `aws:createdBy` (details which identity created a resource, if already enabled) or `aws:cloudformation:stack-name`
- Added to resources AFTER they're enabled by AWS, not retroactive
- User-defined tags can also be enabled `user:something`
- Both are visible in cost reports and can be used as a filter
- Can take up to 24 hours to be visible and active