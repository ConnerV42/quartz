---
title: "NACLs and Security Groups"
tags:
- aws
- aws-networking
- aws-security
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [VPC](/notes/aws/vpc.md)

## Network Access Control List (NACL)
- NACLs are associated with subnets
- Connections between resources within a subnet are NOT affected by NACLs
- Each NACL contains 2 sets of rules
	- Inbound Rules - Affect data entering the subnet
	- Outbound Rules - Affect data leaving the subnet
	- These rule sets are only focused on the direction of the traffic, not necessarily whether it's a request or a response. A request can be either inbound or outbound.
- NACLs are #stateless, so they don't know whether traffic is a request or response.
- NACLs offer both explicit allows and explicit deny's
- Only impacts data crossing the subnet boundary, communication between instances in the same subnet is not affected.
- Allows you to block specific IP's and specific IP ranges
- Not aware of any logical resources, they only allow you to use IP's, CIDR ranges, ports and protocols.
- NACLs cannot be assigned to logical resources, they can only be assigned to subnets
- NACLs are often used together with Security Groups to add explicit DENY (Bad IPs/Nets)
- Each subnet can have one NACL (Default or Custom)
- A NACL can be associated with MANY Subnets

## Security Groups
- Security Groups have #state, so they are aware of inbound/outbound requests
- Security Groups automatically know whether something is a request or a response
- Security Groups have no explicit deny capability
	- They can only ALLOW or implicitly DENY (By not explicitly allowing traffic)
	- Because of this, Security Groups cannot be used to block specific bad actors or traffic
- Supports IP/CIDR based rules
- Supports references to other Security Groups and itself
- Security Groups are not attached to instances, they are attached to ENI's (Elastic Network Interfaces) (even if the UI shows it this way)
- Security Groups apply to all traffic that enters or leaves the Elastic Network Interface.
- Security Groups are capable of using logical references, meaning that, if you had an backend API that receives inbound traffic from another application that has a security group, the backend API security group can reference the logical resource (the other Security Group) as it's source of traffic for it's inbound rule.
	- This has the added benefit of scaling very well (meaning that multiple instances that have the Security Group associated with them, now have access to this backend API)
- Security Groups also allow self referential rules. Meaning, that it can reference itself as the source and allow all traffic. This allows instances that share the same Security Group to freely communicate with each other. IP changes are handled automatically, which makes it easy to use for services that use autoscaling.


### Security Group VS. Network Access Control List (NACL):
- Acts as a firewall for associated Amazon EC2 instances | Acts as a firewall for associated subnets
- Controls both inbound and outbound traffic at the instance level | Controls both inbound and outbound traffic at the subnet level
- You can secure your VPC instances using only security groups | NACLs are an additional layer of defense
- Supports allow rules only | Supports allow rules and deny rules
- Stateful (Return traffic is automatically allowed, regardless of any rules) | Stateless (Return traffic must be explicitly allowed by rules)
- Evaluates all rules before deciding whether to allow traffic | Evaluates rules in number order when deciding whether to allow traffic, starting with the lowest numbered rule
- Applies only to the instance that is associated to it | Applies to all instances in the subnet it is associated with
- Has separate rules for inbound and outbound traffic | Has separate rules for inbound and outbound traffic
- A newly created security group denies all inbound traffic by default | A newly create NACL denied all inbound traffic by default
- A newly created security group has an outbound rule that allows all outbound traffic by default | A newly create NACL denied all outbound traffic by default
- Instances associated with a security group can't talk to each other unless you add rules allowing it | Each subnet in your VPC must be associated with a NACL. If none is associated, the default NACL is selected
- Security groups are associated with network interfaces | You can associate a NACL with multiple subnets; however, a subnet can be associated with only one NACL at a time