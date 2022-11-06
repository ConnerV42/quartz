---
title: "Trusted Advisor"
tags:
- aws
- aws-best-practices
- aws-security
- aws-cost-management
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Trusted Advisor**
- Account Level product (The service isn't free, but includes a free version with 7 core checks for basic and developer support)
- AWS Trusted Advisor is an online tool that provides you real-time guidance to help you provision your resources following AWS best practices. It inspects your AWS environment and makes recommendations for saving money, improving system reliance and reliability, or closing security gaps.
- Trusted Advisor includes an ever-expanding list of checks in the following five categories:
	- **Cost Optimization** – recommendations that can potentially save you money by highlighting unused resources and opportunities to reduce your bill.
	- **Security** – identification of security settings that could make your AWS solution less secure.
	- **Fault Tolerance** – recommendations that help increase the resiliency of your AWS solution by highlighting redundancy shortfalls, current service limits, and over-utilized resources.
	- **Performance** – recommendations that can help to improve the speed and responsiveness of your applications.
	- **Service Limits** – recommendations that will tell you when service usage is more than 80% of the service limit.

Free Version Core Checks
- S3 Bucket Permissions
- Security Groups - Specific Ports Unrestricted
- IAM Use
- MFA on Root Account
- EBS Public Snapshots
- RDS Public Snapshots
- 50 service limit checks

Business & Enterprises support plans contains 115 further checks
- 14 cost
- 17 security
- 24 fault tolerant
- 10 performance
- 50 sevice limit
- Access via the AWS Support API to initiate checks and programmatically request support whenever required
	
