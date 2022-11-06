---
title: "S3 Object Storage Class"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Object Storage Classes

### S3 Standard
- Objects are replicated across at least 3 AZs in the AWS region
- Data is available within milliseconds
- Billed GB/m fee for data stored
- Billed $ per GB for transfer OUT (IN is free) and a price per 1,000 requests
- No specific retrieval fee
- No minimum duration
- No minimum size

### S3 Standard-IA (Infrequent Access)
- Same specs as S3 Standard, but object storage costs are significantly cheaper
- New cost component, which is a retrieval fee, in the form of a per GB data retrieval fee.
- Standard-IA has a minimum duration charge of 30 days - objects can be stored for less, but the minimum billing always applies
- Standard-IA has a minimum capacity charge of 128KB per object

### S3 One Zone-IA (Infrequent Access)
- Same specs as S3 Standard-IA, but significantly cheaper
- One Zone-IA does not provide the multi-AZ resilience model of Standard or Standard-IA. Instead only one AZ is used within the region.
- Should only be used for non-critical or replaceable data

### S3 Glacier - Instant Retrieval
- Like S3 Standard-IA with regards to cheaper storage, but with more expensive retrieval and a longer minimum fee
- Minimum duration charge of 90 days
- Data is still redundantly stored across 3 AZ

### S3 Glacier - Flexible
- S3 Glacier objects cannot be made publicly available
- S3 Glacier objects are not immediately available, you have to initiate a retrieval job
- Once the retrieval job completes, you'll find your object temporarily stored in the S3 Standard-IA storage class
- Data is still redundantly stored across 3 AZ
- Minimum duration charge of 90 days
- Minimum data size of 40 KB
- Ideal for situations where archival data is stored

#### Retrieval Job Types
The faster the retrieval job, the more expensive it is.
- Expedited (1-5 minutes)
- Standard (3-5 hours)
- Bulk (5-12 hours)

### S3 Glacier Deep Archive
- 40 KB minimum size charge
- 180 day minimum duration charge

#### Retrieval Job Types
- Standard (12 hours)
- Bulk (up to 48 hours)

### S3 Intelligent Tiering
- Contains 5 tiers
	- Frequent Access - S3 Standard
	- Infrequent Access - S3 Standard-IA
	- Archive Instant Access - S3 Glacier Instant - Objects move here after 90 days
	- Archive Access - S3 Glacier Flexible (optional)
	- Deep Archive - S3 Glacier Deep Archive (optional)
- Intelligent Tiering monitors and automatically moves any objects not accessed for 30 days to a low cost infrequent access tier and eventually to archive instance access, archive access, or deep archive tiers
- A data tier management fee is applied when using S3 Intelligent Tiering.
- Only useful if retrieval patterns are changing constantly