---
title: "AWS Shield"
tags:
- aws
- aws-security
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [AWS Web Application Firewall](/notes/aws/aws-web-application-firewall.md)

## **AWS Shield**

### Types of Attack
- Network Volumetric Attacks (L3) - Saturate Capacity
	- These types of attacks overwhelm a system by directing as much raw network data at a target as possible
- Network Protocol Attacks (L4) - TCP SYN Flood
	- Flood large number of connections, leave connections open, preventing new ones
	- Analogy is people calling a call center, and staying on the phone lines, preventing real customers from talking to call center employees
	- L4 can also have a volumetric component
- Application Layer Attacks (L7) - e.g. web request floods

### Shield Standard
- Free for all AWS customers, and enabled by default
- Protection at the perimeter (region/VPC or at the AWS edge)
- Common Network (L3) or Transport (L4) layer attacks
- Best protection using R53, CloudFront, AWS Global Accelerator
- No proactive or configurable protection

### Shield Advanced
- Commercial product, costs $3,000 per month, per organization (1 year lock-in + data (OUT) per month)
- Protects CloudFront, R53, Global Accelerator, Anything associated with EIPs (i.e EC2), ALBs, CLBs, NLBs
- Not automatic - must be explicitly enabled in Shield Advanced or AWS Firewall Manager Shield Advanced policy
- Cost protection (i.e EC2 scaling) for unmitigated attacks (reimbursement for something Shield Advanced can cover, and should have covered)
- Proactive Engagement & AWS Shield Response Team (SRT) access
- **AWS Shield Advanced** also gives you 24x7 access to the AWS DDoS Response Team (DRT).
- Integrates with AWS WAF - includes basic AWS WAF fees for Web ACLs, rules, and web requests.
- Application Layer (L7) DDOS protection (uses WAF)
- Real time visibility of DDOS events and attacks
- Health-based detection - application specific health checks, used by proactive engagement team
- Protection groups - creates groupings of resources which Shield Advanced protects, manage protection at group level, decreases admin overhead

#aws #aws-security #aws-sysops #ddos