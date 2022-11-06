---
title: "Egress-Only Internet Gateway"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [NAT Gateway](/notes/aws/nat-gateway.md)

### Egress-Only Internet Gateway
- Egress-Only Internet Gateway is an **outbound-only** Internet Gateway that allows private IPv6 resources in your VPC's private subnets to initiate a connection to the public internet and public AWS services _without_ allowing the Internet to initiate a connection (egress) to your resources.
- Why is this necessary? A [NAT Gateway](/notes/aws/nat-gateway.md) already allows resources with a private IPv4 address to access public networks or public AWS services, without allowing externally initiated connections. BUT, if you use a NAT Gateway with an IPv6 address, all IPs are allowed IN and OUT.
- This is due to all IPv6 addresses being public, whereas IPv4 addresses can be public or private.
- Architecture is exactly the same as a normal Internet Gateway
- Egress-Only Internet Gateway's are High Availability by default across all AZs in the region - scales as required.