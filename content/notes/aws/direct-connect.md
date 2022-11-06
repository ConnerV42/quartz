---
title: "Direct Connect (DX)"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Site-to-Site VPN](/notes/aws/site-to-site-vpn.md)
- [Transit Gateway](/notes/aws/transit-gateway.md)

## Direct Connect
Direct Connect is AWS's physical private link connecting your business premises to its public and private services
- A physical connection (1, 10, or 100 Gbps options) to an AWS Region
- Business Premises => DX Location => AWS Region
	- AWS Regions have multiple DX Locations. These are normally major metro data centers.
- Physical Port Allocation at a DX Location and authorization to connect to that port
- Port Hourly Cost & Outbound Data transfer
- Provisioning time ... physical cables & no resilience
- Low & consistent latency + High Speeds
- Can be used to access AWS Private Services (VPCs) and AWS Public Services - NO INTERNET
- Many pros and cons compared to a Site-to-Site VPN

### Resilience
- DX Locations are connected to the AWS regions via redundant high speed connections. You can assume these are highly available.
- Direct Connect is not resilient by default, but it can be if customized.
- Improvements
	- Use 2 AWS DX Routers at the DX Location, each mapping to multiple Customer DX Routers, which run back to 2 Customer Premises Routers at the on-premises location. You are now safe from a failure from a router from either of the two paths. This isn't completely safe though. If either the DX Location or the Customer Premises location fails, you have an outage.
	- To improve upon this further, use two different DX locations and two different Customer Premises.