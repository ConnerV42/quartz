---
title: "VPC Router"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [VPC](/notes/aws/vpc.md)
- [Internet Gateway](/notes/aws/internet-gateway.md)

## VPC Router
- Every VPC has a VPC Router - Highly available
- In every subnet .. 'network+1' address
- Routes traffic between subnets
- Controlled by 'Route Tables', each subnet has one
- A VPC has a Main route table - subnet default

### Route Tables
- A Route Table controls what happens to data as it leaves the subnet or subnets that that Route Table is associated with.
- Route Tables are a list of routes, attached to 0 or more subnets.
- A subnet has to have a route table, which is either the main route table of the VPC, or a custom one that you've created.
- Local routes are always there, uneditable and match the VPC IPv4 or IPv6 CIDR Range. For anything else, higher prefix values that are more specific, take priority.
- Each entry on a route table has a Destination and Target
- If the target is local, that means that the route is to the VPC itself
- All route tables have at least one route, the local route. The local route matches the VPC CIDR range.