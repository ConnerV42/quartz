---
title: "S3 Security"
tags:
- aws
- s3
- aws-security
disableToc: false
---

### Related Notes
- [s3](/notes/aws/s3/s3.md)

### Useful Links
- [Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html)

## S3 Security
- S3 is private by default. Everything that is done to control S3 permissions is based upon this starting point
- The only identity that has any access to an initial S3 bucket, is the account root user of the account which created that bucket. Other permissions must be explicitly granted.

```json
{
  "Version": "2012-10-17"
	"Statement": [
		"Sid": "PublicRead",
		"Effect": "Allow",
		"Principal": "*", // The principal part of a policy defines who that statement applies to. (which identities, principal)
		"Action": ["s3:GetObject"],
		"Resource": ["arn:aws:s3:::secretcatproject/*]
	]
}
```
- A wildcard means that any principal can perform the action on this secret cat project bucket. This means that this applies to all AWS accounts, and even anonymous principals.

### When To Use Which Policy
- Identity Policies: Controlling different resources
- Identity Policies: You have a preference for IAM
- Identity Policies: Same account
- Bucket Policies: Just controlling S3
- Bucket Policies: Anonymous or Cross-Account
- ACLs: NEVER - unless you must

### S3 Permissions

#### S3 Bucket Policy (Resource Policy)
-  A form of resource policy
- Like identity policies, but attached to a bucket
- Permissions from a resource perspective
- Identity policies have a significant limitation. They can only be attached to identities in your own account. identity policies can only control security in your own account. You have no way of giving an identity in another account access to your S3 bucket.
- Resource policies, however, allow you to reference any other identity, whether they're in the same account or a separate account.
	- This is a major benefit of resource polices
- Resource policies can also allow or deny anonymous principals.
	- This is in contrast to identity policies, which by design, have to be attached to valid identity in AWS.
	- Resource policies can be used to open a bucket to the world, by referencing all principals, even those not authenticated by AWS, Anonymous Principals.
	
#### Block Public Access Settings
- This was added because so many companies don't understand the S3 permission model
- If enabled, this will override Resource Policies that grant public access

#### Access Control Lists (ACLs) - Legacy
- ACLs on objects and bucket
- A subresource of the object or bucket
- AWS doesn't recommend their usage and recommends identity policies or bucket policies
	- They've been replaced because they are inflexible and only allow very simple permissions
		- For example, they don't support conditions
- There are just 5 permissions, that applied to either the entire bucket, or a specific bucket
	- READ
	- WRITE
	- READ_ACP
	- WRITE_ACP
	- FULL_CONTROL
- ACLs cannot be used on a group of objects