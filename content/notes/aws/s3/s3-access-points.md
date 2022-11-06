---
title: "S3 Access Points"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Access Points
- Simply managing access at scale for applications using shared data sets on S3 buckets. Access Points are unique hostnames that customers create to enforce distinct permissions and network controls for any request made through the access point.
- Rather than 1 bucket with 1 Bucket Policy
- Create many access points, each with different policies, and each with different network access controls
	- You can have an Access Point with a VPC origin - This requires a VPC endpoint
	- You can have an Access Point with a Internet Origin
- Each access point has its own endpoint address
- Created via Console or `aws s3control create-access-point --name secretcats --account-id 123456789012 --bucket catpics`
- Each Access Point has a unique DNS address for network address
- Access Point policies control permissions for access via the Access Point and are functionally equivalent to a bucket policy. Access Point policies can restrict identities to certain prefix(s), tags, or actions based on need.
- Any permissions defined on an Access Point need to be also defined on the Bucket Policy.
	- Normally, on the Bucket Policy, you'd grant wide open access to the Access Point
	- From there, you'd define more granular control over access to objects on the Access Point policy
