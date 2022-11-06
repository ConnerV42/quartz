---
title: "Internet Gateway"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [VPC](/notes/aws/vpc.md)
- [VPC Router](/notes/aws/vpc-router.md)

## Internet Gateway
- Region resilient gateway attached to a VPC
- You do not need a gateway per availability zone
- 1 VPC = 0 or 1 IGW, 1 IGW = 0 or 1 VPC
- Runs from within the AWS Public Zone
- Gateways traffic between the VPC and the Internet or AWS Public Zone (S3, SQS, SNS, etc.)
- Managed - AWS handles performance (it simply works)

### Creation of a Public Subnet
1. Create IGW
2. Attache IGW to VPC
3. Create custom RT
4. Associate RT
5. Default Routes => IGW
6. Subnet allocate IPv4