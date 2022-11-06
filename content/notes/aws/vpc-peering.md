---
title: "VPC Peering"
tags:
- aws
- aws-security
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [VPC](/notes/aws/vpc.md)

## VPC Peering
- A service that lets you create a direct, private, encrypted network link between 2 VPCs. No more than 2.
- Works same/cross-region and same/cross-account
-  Public Hostnames resolve to private IPs (optional)
- Same region Security Group's can reference peer Security Groups (only works in the same region) using Security Group IDs
- VPC Peering does NOT support transitive peering (Connecting VPC A to VPC C by going through an intermediary VPC A -> B -> C would not work, you would need to setup VPC Peering between A -> C)
- VPC Peering connections cannot be created where there is overlap in the VPC CIDRs - ideally NEVER use the same address ranges in multiple VPCs

---

- A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them privately. Instances in either VPC can communicate with each other as if they are within the same network. You can create a VPC peering connection between your own VPCs, with a VPC in other AWS account, or with a VPC in a different AWS Region. AWS uses the existing infrastructure of a VPC to create a VPC peering connection; it is neither a gateway nor a VPN connection and does not rely on a separate piece of physical hardware. There is no single point of failure for communication or a bandwidth bottleneck. This could allow you to do something like, push minor code releases from a DEV VPC to a PROD VPC to speed up time to market.

- A VPC peering connection does not support edge to edge routing. This means that if either VPC in a peering relationship has one of the following connections, you cannot extend the peering relationship to that connection:
	- A VPN connection or an AWS Direct Connect connection.
	- An Internet connection through an Internet gateway.
	- An Internet connection in a private subnet through a NAT device.
	- A gateway VPC endpoint to an AWS service; for example, an endpoint to Amazon S3.
	- (IPv6) A ClassicLink connection. You can enable IPv4 communication between a linked EC2-Classic instance and instances in a VPC on the other side of a VPC peering connection. However, IPv6 is not supported in EC2-Classic, so you cannot extend this connection for IPv6 communication.
	
## #sysops Scenarios
- *QUESTION*: A media company has two VPCs: VPC-1 and VPC-2 with peering connection between each other. VPC-1 only contains private subnets while VPC-2 only contains public subnets. The company uses a single AWS Direct Connect connection and a virtual interface to connect their on-premises network with VPC-1. How could you increase the fault tolerance of the connection to VPC-1?
- *ANSWER*: Establish a hardware VPN over the Internet between VPC-1 and the on-premises network.
- *ANSWER*: Establish another AWS Direct Connect connection and private virtual interface in the same AWS region as VPC-1.