---
title: "IAM Identity Center"
tags:
- aws
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [IAM](/notes/aws/iam.md)

## IAM Identity Center (Formerly AWS SSO)
- Recommended solution for any workforce style identity federation requirements
- Manage SSO Access - AWS Accounts and External Applications
- Flexible Identity Source - SSO Integrates with a range of workplace Identity Sources ranging from built-in SSO identities through to self-managed on-premises Active Directory
- Preferred by AWS vs traditional workforce identity federation (SAML2.0 is mostly only around to support legacy clients that haven't migrated)
- SSO extends past AWS, and also delivers SSO solutions for Slack, Dropbox, Office 365, and Salesforce
- This means that is handles SSO & permissions for both AWS accounts and external applications for a business
- This product is completely free
- Note that this applies to enterprise or workplace identities, not customer identities (web applications using Twitter, Google, Facebook, or any other web identity). For customer identities, a tool like AWS Cognito is the best fit.

### Supported Identity Stores
- AWS SSO itself - Built-in identity store
- AWS Managed Microsoft AD
- On-premises Microsoft AD (Two way trust or AD Connector)
- External Identity Provider - SAML2.0