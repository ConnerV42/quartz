---
title: "VPC Interface Endpoints"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [VPC Gateway Endpoints](/notes/aws/vpc-gateway-endpoints)

## VPC Endpoints (Interface Endpoints)

- Similar to [VPC Gateway Endpoints](/notes/aws/vpc-gateway-endpoints), Interface Endpoints allow private access to AWS Public Services.
- Supports all services, excluding DynamoDB.
- Interface endpoints are NOT highly available, they are added to a specific subnets within a VPC.
	- 1 subnet == 1 Availability Zone
- Network access is controlled via Security Groups.
- Supports Endpoint Policies to restrict what can be done with the endpoint.
- Currently ONLY supports TCP and IPv4.
- Uses [[privatelink]] to inject 3rd party applications or services into private VPC subnets.
- Uses DNS and a private IP
- Interface Endpoints provide a NEW service endpoint DNS
	- e.g. vpce-123-xyz.sns.us-east-1.vpce.amazonaws.com
		- Resolves to the private IP address of the Interface Endpoint and allows access to SNS without public IP addressing
- PrivateDNS overrides the default DNS for services, so that even for applications that have not been re-configured to use the Interface Endpoint-specific DNS can still access the desired AWS service without an issue.