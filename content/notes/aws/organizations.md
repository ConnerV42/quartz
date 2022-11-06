---
title: "AWS Organizations"
tags:
- aws
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Organizations**
- AWS Organizations is an account management services that lets you consolidate multiple AWS accounts into an organization that you create and centrally manage. With Organizations, you can create member accounts and invite existing accounts to join your organization. You can organize those account into groups and attach policy-based controls.
- Consolidation of payment methods
- You take a single "Standard" AWS account (one which is not within an Organization)
- You create an AWS Organization with that account, and that account now becomes the master/management account
- The management account in special for regions
	- asdf
	- asdf
- With the management account, you can invite other Standard accounts to the Organization, which makes them "member" accounts of that Organization
- The Organization Root (container within AWS Organization, that can contain AWS accounts at the top of this structure), can contain both the management account and member accounts
- The Organization Root can contain other Organizations, known as Organizational Units (OU)
- AWS Organization allows for consolidated billing, member accounts pass their billing through to the management/master account
- Certain services offer volume discounts, which are easier to achieve by using consolidated billing
- You can also create new accounts directly within an Organization, all that is required is a valid email address (AKA, no invite process)
- With Organizations, you don't need IAM users inside of every account. Instead, you can use roles across AWS accounts that are assumed when needed
- Often, large enterprises will have a separate account, distinct from the master account, that is used to hold all identities in your Organization. You can either have your own internal AWS identities using IAM, or configure AWS to allow identity federation so that on-premises identities can be used to access designated login accounts. Doing this, you can use a feature called "role switch" to assume roles in other accounts.

### Service Control Policies (SCPs)
- A feature of AWS Organizations which allows restrictions to be placed on member AWS accounts in the form of boundaries
- SCPs can be attached to AWS Organizations as a whole, one or more Organizational Units, or individual AWS accounts
- SCPs inherit down the organizational tree, so if they're attached to the Root OU, they affect everything below it.
- Even if the management account has an SCP attached, it is not affected by the SCP. As a security practice, generally, you should not use the management account for resources.
- SCPs are account permissions boundaries, they limit what the account can do (including the account root user)
	- Fine point detail: You can never restrict the account root user directly, but by applying an SCP to the account, you are indirectly restricting what the account root user can do.
- You might apply a SCP to restrict usage of any region outside of your chosen operating region
- SCPs don't grant any permissions, they only limit what permissions can be applied to an account
- SCPs have an allow list vs deny list, but they establish which permissions can be granted in an AWS account. When an SCP has an allow list, that means that, no matter what permissions identities in this account are provided with, they can only use what has specifically been allowed by the SCP that is applied to their account. Note that this is very secure, but causes much more admin overhead.
- Example: If an SCP allows S3 and EC2, then those are the only services that will ever be usable in that AWS account, no matter what permissions identities are granted.
- Deny list architectures have much lower admin overhead, but allow list architectures are much more secure.