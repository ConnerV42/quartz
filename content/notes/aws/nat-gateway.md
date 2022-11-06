---
title: "NAT Gateway"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes:
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Egress-Only Internet Gateway](/notes/aws/egress-only-internet-gateway.md)
- [VPC Gateway Endpoints](/notes/aws/vpc-gateway-endpoints.md)
- [VPC Interface Endpoints](/notes/aws/vpc-interface-endpoints.md)

### Helpful Links
- [VPC Pricing](https://aws.amazon.com/vpc/pricing/)
 > If you choose to create a NAT gateway in your VPC, you are charged for each “NAT Gateway-hour" that your gateway is provisioned and available. Data processing charges apply for each gigabyte processed through the NAT gateway regardless of the traffic’s source or destination. Each partial NAT Gateway-hour consumed is billed as a full hour. You also incur standard AWS data transfer charges for all data transferred via the NAT gateway. If you no longer wish to be charged for a NAT gateway, simply delete your NAT gateway using the AWS Management Console, command line interface, or API.
>> Price per NAT gateway ($/hour) is $0.045
>>> Price per GB data processed is $0.045

## **NAT Gateway**
- A NAT Gateway allows for private IPv4 addresses (of say, an EC2 instance or ECS instance) in a VPC's private subnet to connect to the public internet, and/or public AWS services while also _preventing the Internet from initiating a connection with those private IPv4 addresses.
- A NAT Gateway is a #highly-available, managed Network Address Translation service. NAT gateway is created in a specific AZ and implemented with redundancy in that zone. You must create a NAT gateway on a public subnet to enable instances in a private subnet to connect to the Internet or other AWS services, but prevent the Internet from initiating a connection with those instances.
- If you have resources in multiple AZs and they share one NAT gateway, and if the NAT gateway's AZ is down, resources in other AZs lose Internet access. To create an AZ-independent architecture, create a NAT gateway in each AZ and configure your routing to ensure that resources use the NAT gateway in the same AZ.
- **NOTE**: You would never need to configure more than a single NAT gateway in a given AZ. This is because NAT Gateway is already redundant in nature, meaning, AWS already handles any failures that occur in your NAT Gateway in an availability zone.
- NAT Gateway works with IPv6, with the extremely important caveat that absent any other filtering, NAT Gateway (IPv6) will allow all IPs bi-directionally. If you need to prevent egress traffic, use an [[egress-only-internet-gateway]] instead.