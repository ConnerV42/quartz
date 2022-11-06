---
title: "S3 PreSigned URLs"
tags:
- aws
- s3
- aws-security
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Presigned URLs
- Give another person or application access to an object inside of an S3 bucket, using your credentials in a safe and secure way
- Either you or an automated process can generated a presigned URL, which has specific access permissions encoded within it. These permissions can be for an entire bucket, or a specific object within that bucket. The URL is valid for a certain time period, after which it expires.
- The end user of the URL acts as the original person who generated the URL
- When might presigned URLs be used?
	- Imagine a S3 bucket, used as a media bucket for a web server.
	- Presigned URLs allow for the bucket to be private, and utilize an IAM user (service account) that generates presigned URLs so that the web server can utilize presigned URLs to give private videos to end users.
	- The end users in-browser client application can then use the presigned URL to access the private video
- Oddly enough, You CAN create a URL for an object you have no access to. There is no use from it, but it is possible.
- When utilizing the URL, the permissions match the identity which generated it
- Access denied could mean the generating ID never had access .. or doesn't now. You inherit the permissions of the identity RIGHT NOW.
- This means, you should NEVER generate presigned URLs with an IAM Role! IAM Role temporary credentials will generally expire long before the presigned URL does.