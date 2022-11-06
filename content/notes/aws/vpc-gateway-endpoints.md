---
title: "VPC Gateway Endpoints"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes:
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Virtual Private Cloud (VPC)](/notes/aws/vpc.md)
- [VPC Interface Endpoints](/notes/aws/vpc-interface-endpoints.md)

## VPC Endpoints (Gateway Endpoints)
- Provides private access to supported AWS services like S3 and DynamoDB (public services)
- Allows a private-only resource inside of a VPC to access S3 and DynamoDB
- Allows any resource inside a private-only VPC to access S3 and DynamoDB.
- Normally, to achieve the same functionality granted by VPC Endpoints you would need to create an [Internet Gateway](/notes/aws/internet-gateway.md) and attach it to a VPC. Then, you would have to grant the resource in that VPC a public IPv4 address or IPv6 address (always public), or implement one or more NAT Gateways which allow instance with private IP address to access these public services.
- Gateway Endpoint allows you to provide access to these services without implementing the above public infrastructure.
- Gateway Endpoints are created per service, per region and can be associated with one or more subnets in a particular VPC.
- Gateway Endpoints are #highly-available across all availability zones in a region by default. No need to worry about AZ placement.
- Gateway Endpoints do not actually go into a particular VPC subnets or availability zone, when you allocate the Gateway Endpoint to a particular subnet, a prefix list is added to the Route Tables for those subnets. The prefix list is a logical object that represents the services (DynamoDB, S3). The prefix list is the destination and the target is the Gateway Endpoint.
- When configuring a Gateway Endpoint, Endpoint Policies are used to control what it can access.
  - Allows you to, for example, only allow your Gateway Endpoint to connect to a particular subset of S3 Buckets.
- Gateway Endpoints can only be used to access services in the same region
- S3 buckets can be set to private only by allowing access ONLY from a Gateway Endpoint
- Gateway Endpoints can ONLY be accessed from within the VPC they have been created to serve.