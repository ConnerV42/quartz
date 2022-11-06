---
title: "S3 Events"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Events
- A feature that allows you to create event notification configurations on a bucket
- Supports SNS, SQS, Lambda functions, etc.
- Object Create (Put, Post, Copy, CompleteMultiPartUpload)
- Object Delete (Delete, DeleteMarkerCreated)
- Object Restore (Post (initiated), Completed)
- Replication (OperationMissedThreshold, OperationReplicatedAfterThreshold, OperationNotTracked, OperationFailedReplication)
- EventBridge is probably the better option to use, S3 Events is an older feature in AWS