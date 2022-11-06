---
title: "AWS Control Tower"
tags:
- aws
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## AWS Control Tower
- Allows quick and easy setup of multi-account environment
- Orchestrates other AWS services to provide this functionality
- Leverages [Organizations](/notes/aws/organizations.md), [IAM Identity Center (formerly AWS SSO)](/notes/aws/iam-identity-center.md), [CloudFormation](cloudformation.md), [AWS Config](/notes/aws/aws-config.md) and [AWS Service Catalog](/notes/aws/aws-service-catalog.md)
- Landing Zone - multi-account environment
	- SSO/ID Federation, Centralized Logging and Auditing
- Guard Rails - Detect/Mandate rules/standards across all accounts
	- Config is used under the hood to implement these guardrails
- Account Factory - Automates and Standardizes new account creation
	- Basic template is applied (CloudFormation is used under the hood for this)
- Dashboard - single page oversight of the entire environment

### Landing Zone
- Allows for implementation of a Well Architected multi-account environment
- Home Region - The region you initially deploy into
	- You can explicitly allow or deny the usage of other regions
	- Built with Organizations, Config and CloudFormation
- Security OU - Log Archive & Audit Accounts (CloudTrail & Config Logs)
- Sandbox OU - Test/less rigid security
- You can create other OU's and Accounts
- IAM Identity Center (AWS SSO) - SSO, multiple accounts, ID Federation (use existing identity stores)
- Monitoring and Notifications - CloudWatch and SNS
- End User account provisioning using Service Catalog

### Guard Rails
- Guardrails are rules - multi-account governance
- Mandatory, Strongly Recommended or Elective
- Preventative - Stop you from doing things (AWS ORG SCP)
	- enforced or not enabled
	- i.e allow or deny regions or disallow bucket policy changes
- Detective - compliance checks (AWS Config Rules)
	- Clear, in violation, or not enabled
	- Detect CloudTrail enabled or EC2 Public IPv4

### Account Factory
- Automated Account Provisioning
- Cloud Admins or end users (with appropriate permissions)
- Guardrails - automatically added
- Account admin given to a named user (IAM Identity Center)
- Account & network standard configuartion
- Accounts can be closed or repurposed
- Can be fully integrated with a business SDLC using Account Factory / Control Tower APIs

#aws #aws-sysops 