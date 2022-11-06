---
title: "S3 Object Lock"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Object Lock

- Object Lock enables a Write-Once-Read-Many (WORM) - No Delete, No Overwrite
- Object Lock can easily be enabled on new buckets, but if you want to enable it on existing buckets, you have to file for AWS support
- You cannot disable Object Lock
- Versioning is enabled on a bucket when Object Lock is enabled
	- Individual versions are locked
- Both, One, or the other, or none
- A bucket can have default Object Lock settings for all of the below information
- This is a very important S3 feature to understand for the SysOps Administrator Associate Certification Exam

- Retention Period - Prevent Deletion
	- Specify Days and Years - A Retention Period
	- Compliance Mode - Object (and Retention Period settings) cannot be adjusted, deleted or overwritten, even by the account root user
		- Strictest version of Object Lock
		- Medical or Financial Data makes sense here
	- Governance Mode - Special permissions can be granted allowing lock setting to be adjusted with the `s3:BypassGovernanceRetention` and passing the `x-amx-bypass-governance-retention:true` header. (default if done through the console UI)
- Legal Hold - Set to be ON or OFF for Object Versions, cannot be changed or deleted while ON
	- Set on an Object Version - ON or OFF
	- No Retention
	- NO Deletes or Changes until removed
	- Prevents accidental deletion of critical object versions
	- `s3:PutObjectLegalHold` is to be set when object is uploaded or when legal hold is required

#aws #s3 #aws-sysops 