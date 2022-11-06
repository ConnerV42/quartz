---
title: "S3 Lifecycle Configuration"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)
- [S3 Object Storage Class](/notes/aws/s3/s3-object-storage-class.md)

## S3 Lifecycle Configuration
- A set of rules, consisting of actions
- Rules can apply to an entire bucket, or groups of objects
- Transition Actions move objects to a different Object Storage Class
	- Transitions can only happen in a downward direction
	- Be aware of the 30 day S3 waiting period before you can transition an object
- Expiration Actions can delete Objects or Object Versions after a certain time period