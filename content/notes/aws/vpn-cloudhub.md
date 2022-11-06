---
title: "VPN CloudHub"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **AWS VPN CloudHub**
- Capable of wiring multiple AWS Site-to-Site VPN connections together on a virtual private gateway. This is useful if you want to enable communication between different remote networks that uses a Site-to-Site VPN connection.
- A Note About VPNs: Although it is true that a VPN provides a cost-effective, hybrid connection from your VPC to your on-premises data centers, it certainly does not bypass the public Internet. A VPN connection actually goes through the public Internet, unlike the AWS Direct Connect connection which has a direct and dedicated connection to your on-premises network.
- A VPN connection is not capable of providing consistent and dedicated access to on-premises network services. Use Direct Connect for hybrid applications.
- You can connect your VPC to remote networks by using a VPN connection which can be IPSec VPN connection, AWS VPN CloudHub, or a third party software VPN appliance. A VPC VPN Connection utilizes IPSec to establish encrypted network connectivity between your intranet and Amazon VPC over the Internet.