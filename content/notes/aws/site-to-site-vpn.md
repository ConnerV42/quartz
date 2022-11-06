---
title: "AWS Site-to-Site VPN"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Direct Connect](/notes/aws/direct-connect.md)

## AWS Site-to-Site VPN
- Offers the quickest way to create a network link between an AWS environment and an on-premises environment, or another cloud.
- A logical connection between a VPC and on-premises network encrypted using IPSec, running over the **public internet**.
- Highly available, if designed and implemented correctly
- Quick to provision, in less than an hour (in constrast to the long provisioning times of Direct Connect)
- Speed Limitations: 1.25 Gbps
- Latency Considerations: inconsistent, because it travels over the public internet (Look into Direct Connect, if this is an issue)
- Cost - AWS hourly cost, GB out cost, data cap (on-premises)
- Speed of setup - hours .. all software configuration
- Can be used as a backup for Direct Connect

### Virtual Private Gateway (VGW)
- Logical gateway object within AWS, which can be the target on one or more route tables
- You create and associate it with a single VPC
- Actually has physical endpoints with 2 public IPv4 addresses, each in different availability zones (Highly Available, by design)
	
### Customer Gateway (CGW)
- This can refer to two different things
	- The logical configuration entity in AWS
	- The physical device that this logical configuration represents
	
### Static VPN Connection (between the VGW and CGW)
- When a VPN is created in the AWS Public Zone, you need to link it to a VGW. This process links the VPN to both physical endpoints of the VGW.
- The VPN also needs to be linked to the CGW. This allows 2 VPN tunnels to be created, which flow from the physical endpoints of the VGW to the singular CGW. (At this state, the VPN design is only partially highly available. If the availability zone housing one of the physical endpoints in AWS goes down, traffic will still flow through the other VPN tunnel. But if the CGW goes down, traffic will be halted).
- A VPN tunnel is an encrypted channel through which data can flow through the VPC to the on-premises network, or vice-versa.
- To resolve single point of failure, we just need to add another on-premises customer route, using a separate internet connection.
	- If this is done, it requires creating a second VPN connection on the AWS side, which will create an additional 2 physical endpoints on the same VGW, bringing the total up to 4 physical endpoints altogether, managed by the VGW.
	- This achieves high availability

### Dynamic VPN Connection
- Uses the Border Gateway Protocol
- The main difference is how routes are communicated
- A static VPN uses static network configuration. Static routes are added to the route tables, and networks for remote side statically configured on the VPN connection. But, there's no load balancing or multi-connection failover.
- With Dynamic VPNs, BGP is configured on both the customer and AWS side using ASN. Networks are exchanged via BGP. Multiple VPN connections provide HA and traffic distribution. With Dynamic VPNs, you can still add static routes, or you can enable route propagation, which allows routes to be dynamically learned by the route table.