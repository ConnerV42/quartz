---
title: "Transit Gateway"
tags:
- aws
- aws-networking
- network-gateway
- highly-available
- high-availability
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Direct Connect](/notes/aws/direct-connect.md)

## **Transit Gateway**
- The AWS Transit Gateway is a network gateway which can be used to significantly simplify networking between VPC's, VPN, and Direct Connect
- It can be used to peer VPCs in the same account, different account, same or different region and supports transitive routing between networks
- Network Transit Hub to connect VPCs to on-premises networks
- Significantly reduces network complexity, especially as you scale, reduces number of VPN tunnels
- Single network object - HA and Scalable
- Highly available inter-VPC router, allows VPCs to talk to each other through the Transit Gateway
- Support Transitive Routing
- Share between accounts using AWS RAM
- Transit Gateways can also be peered with Transit Gateways in other regions or cross account
	- This can be used to create a global network within AWS
- Creates attachments to other network types
	- VPC
	- Site-to-Site VPN
	- Direct Connect Gateway
- AWS Transit Gateway is a service that enables customers to connect their VPC Clouds and their on-premises networks to a single gateway. As you grow the number of workloads on AWS you need to be able to scale your networks across multiple accounts and Amazon VPCs to keep up with the growth. Today, you can connect parts of Amazon VPCs without peering. However, managing point-to-point connectivity across many Amazon VPCs without the ability to centrally manage the connectivity policies can be operationally costly and cumbersome. For on-premises connectivity you need to attach your AWS VPN to each individual Amazon VPC. This solution can be time-consuming to build and hard to manage when the number of VPCs grows into the hundreds. With AWS Transit Gateway, you only have to create and manage a single connection from the central gateway to each Amazon VPC, on-premises data center, or remote office across your network. Transit Gateway acts as a hub that controls how traffic is routed among all the connected networks which act like spokes. This hub and spoke model significantly simplifies management and reduces operational costs because each network only has to connect to the Transit Gateway and not to every other network. Any new VPC is simply connected to the Transit Gateway and is then automatically available to every other network that is connected to the Transit Gateway. This ease of connectivity makes it easy to scale your network as you grow.
- AWS Transit Gateway provides a hub and spoke design for connecting VPCs and on-premises networks. You can attach all your hybrid connectivity (VPN and Direct Connect connections) to a single Transit Gateway consolidating and controlling your organization's entire AWS routing configuration in one place. It also controls how traffic is routed among all the connected spoke networks using route tables. This hub and spoke model simplifies management and reduces operational costs because VPCs only connect to the Transit Gateway to gain access to the connected networks.
- By attaching a transit gateway to a Direct Connect gateway using a transit virtual interface, you can manage a single connection for multiple VPCs or VPNs that are in the same AWS Region. You can also advertise prefixes from on-premises to AWS and from AWS to on-premises.
- The AWS Transit Gateway and AWS Direct Connect solution simplify the management of connections between an Amazon VPC and your networks over a private connection. It can also minimize network costs, improve bandwidth throughput, and provide a more reliable network experience than Internet-based connections.
- AWS Transit Gateway simplifies your network and puts an end to complex peering relationships.
- How to improve the speed of slow Site-to-Site VPN connections? Associate the VPCs to an **Equal Cost Multipath Routing (ECMR)-enabled transit gateway** and attach additional VPN tunnels.
- With AWS Transit Gateway, you can simplify the connectivity between multiple VPCs and also connect to any VPC attached to AWS Transit Gateway with a single VPN connection.
- AWS Transit Gateway also enables you to scale the IPsec VPN throughput with equal-cost multi-path (ECMP) routing support over multiple VPN tunnels. A single VPN tunnel still has a maximum throughput of 1.25 Gbps. If you establish multiple VPN tunnels to an ECMP-enabled transit gateway, it can scale beyond the default limit of 1.25 Gbps.
