---
title: "S3 Object Versioning and MFA Delete"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Object Versioning
- Controlled at the bucket level
- This is disabled by default, but once it is enabled, it CANNOT be disabled, but it can be suspended (which is effectively the same thing)
	- You will continue to be billed for all Object versions, even when Object Versioning is suspended
- Without Object Versioning being enabled, each object is solely identified by it's key, which is unique inside the bucket
	- If you modify an object, the previous version is deleted
- Object Versioning allows you to store multiple versions of an object within a bucket
	- When an object with KEY = `cat.jpg` is created, it is allocated an id (ex: `11111`). When a new version is created, it will keep the same key, but the new version will be assigned a new ID (ex: `22222`). You can optionally specify an ID when requesting an S3 object. If you don't specify an ID, you'll receive the latest version.
- How does Object Versioning affect Object Deletion?
	- If we indicate to S3 that we want to delete an object, and we don't give a specific version ID, a Delete Market is created.
	- A Delete Marker is essentially just a special version of the object that will hide all previous versions of the object
	- If the Delete Marker is deleted, all previous versions are now unhidden and queryable once again
- So how do are objects actually deleted when Object Versioning is enabled?
	- When deleting an object, you must specify the version of the object that you want to delete by passing in the version ID.
- If you notice a significant increase in the number of HTTP 503-slow down responses received for Amazon S3 PUT or DELETE object requests to a bucket that has versioning enabled, you might have one or more objects in the bucket for which there are millions of versions. When you have objects with millions of versions, Amazon S3 automatically throttles requests to the bucket to protect the customer from an excessive amount of request traffic, which could potentially impede other requests made to the same bucket.
	- To determine which S3 objects have millions of versions, use [S3 Inventory](/notes/aws/s3/s3-inventory.md). The inventory tool generates a report that provides a flat file list of the objects in a bucket.

## MFA Delete
- This is enabled within the versioning configuration of an S3 bucket
- When enabled, MFA is required to change bucket versioning state
- MFA is also required to fully delete versions
- To change the bucket versioning state, or fully delete an object version, you must provide the following concatenated value to API calls: `the serial number of your MFA token + the code that MFA generates`