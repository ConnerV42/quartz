---
title: "S3 Replication"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Replication
S3 has replication features which allow objects to be replicated between a SOURCE and DESTINATION bucket in the same or different AWS accounts.
- Cross-Region Replication
- Same-Region Replication
- Standard Replication Configuration is applied to the SOURCE bucket
- You can either replicate all objects, or a subnet of objects
- You can specify the storage class (default is to maintain the same)
- You can specify AWS account ownership (default is the source account)
- Replication Time Control - Adds a guaranteed 15 minute replication timeline to keep buckets in sync
- Replication is not retroactive & Versioning needs to be ON
- One-way replication ONLY
- Replication supports unencrypted objects, SSE-S3 and SS3-KMS
- NO system events, Glacier, or Glacier Deep Archive
- NO DELETES are replicated
- Why might you use S3 Replication?
	- SRR - Log Aggregation
	- SRR - PROD and TEST env sync
	- CRR - Global Resilience Improvements
	- CRR - Latency Reduction

#aws #s3 #aws-sysops 