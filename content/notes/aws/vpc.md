---
title: "AWS VPC"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Virtual Private Cloud (AWS VPC)**
- Note: /32 after an IP address denotes one IP address, while /0 refers to the entire network.
- What is the best way to block a suspicious IP address from attempting to access the resources inside of your VPC through port scans?
	- Modify the Network Access Control List (NACL) associated with all public subnets in the VPC to deny access from the IP Address block.
	- **NOTE**: This is something that you would *never* be able to do from an IAM policy, because an IAM policy does not control the inbound and outbound traffic of your VPC.
- A VPC spans all the Availability Zones in the region. After creating a VPC, you can add one or more subnets in each Availability Zone. When you create a subnet, you specify the CIDR block for the subnet, which is a subset of the VPC CIDR block. Each subnet must reside entirely within one Availability Zone and cannot span zones. Availability Zones are distinct locations that are engineered to be isolated from failures in other Availability Zones. By launching instances in separate Availability Zones, you can protect your applications from the failure of a single location.
- **Route Tables**
	- Your VPC has an implicit router and you use route tables to control where network traffic is directed. Each subnet in your VPC must be associated with a route table, which controls the routing for the subnet (subnet route table). You can explicitly associate a subnet with a particular route table. Otherwise, the subnet is implicitly associated with the main route table.
	- A subnet can only be associated with one route table at a time, but you can associate multiple subnets with the same subnet route table. You can optionally associate a route table with an internet gateway or a virtual private gateway (gateway route table). This enables you to specify routing rules for inbound traffic that enters your VPC through the gateway.
	- Be sure that the subnet route table also has a route entry to the internet gateway. If this entry doesn't exist, the instance is in a private subnet and is inaccessible from the internet.
	- In cases where your EC2 instance cannot be accessed from the Internet (or vice versa), you usually have to check 2 things:
		- Does it have an EIP or public IP address?
		- Is the route table properly configured?
- You cannot have a VPC with IPv6 CIDRs only. The default IP addressing system in VPC is IPv4. You can only change your VPC to dual-stack mode where your resources can communicate over IPv4, or IPv6, or both, but not exclusively with IPv6 only.

### VPC Flow Logs
- Capture metadata (NOT CONTENTS)
	- Source IP, Destination IP, Source Port, Destination Port, Packet Size
- Can be attached at different levels of a VPC (Flow logs capture metadata from the capture point down)
	- Attached to a VPC - All ENIs in that VPC 
	- Subnet - All ENIs in that Subnet
	- ENIs directly
- Flow Logs are NOT realtime
- Log Destinations ... S3 or CloudWatch Logs
- ... or Athena for querying...
- Flow Logs can capture ACCEPTED, REJECTED or ALL metadata

### **Gateway and Interface VPC Endpoints**
- A **VPC Endpoint** enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.
	- When you create a VPC Endpoint, you can attach an endpoint policy that controls access to the service to which you are connecting. You can modify the endpoint policy attached to your endpoint and add or remove the route tables used by the endpoint. An endpoint policy does not override or replace IAM user policies or service-specific policies (such as S3 bucket policies.) It is a separate policy for controlling access from the endpoint to the specified service. You can use a bucket policy or an endpoint policy to allow traffic to trusted S3 buckets. However, it takes a lot of time to configure a bucket policy for each S3 bucket instead of using a single endpoint policy. Therefore, endpoint policies are the best approach to control the traffic to trusted Amazon S3 buckets.
- There are 2 types of VPC endpoints: *interface endpoints* and *gateway endpoints*. You have to create the type of VPC endpoint required by the supported service.
	- An **interface endpoint** is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported service. An interface endpoint is billed for hourly usage and data processing charges.
	- A **gateway endpoint** is a gateway that is a target for a specific route in your route table, used for traffic destined to a supported AWS service. You won't get billed if you use a Gateway VPC endpoint for your S3 bucket.

### Cost - VPC is free
There are no additional charges for creating and using the VPC itself. VPC is free! Usage charges for other Amazon Web Services, including Amazon EC2, still apply at published rates for those resources, including data transfer charges.