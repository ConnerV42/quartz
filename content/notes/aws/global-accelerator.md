---
title: "Global Accelerator"
tags:
- aws
- aws-networking
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [CloudFront](/notes/aws/cloudfront.md)

## **Global Accelerator**
- Similar to CloudFront, in that they both improve performance, but they each do so in different ways, for different reasons.
- AWS Global Accelerator is a networking service that improves the performance of your users' traffic by up to 60% using AWS' global network infrastructure. When the internet is congested, AWS Global Accelerator optimizes the path to your application to keep packet loss, jitter and latency consistency low.
- With Global Accelerator, you are provided 2 global static public Anycast IP addresses that act as a fixed entry point to your application, improving availability. On the back end, add or remove your AWS application endpoints, such as Application Load Balancers, Network Load Balancers, EC2 instances, and Elastic IP's without making user-facing changes. Global Accelerator automatically re-routes your traffic to your nearest healthy available endpoint to mitigate endpoint failure.
	- Anycast IP addresses allow a single IP to be in multiple locations. Routing moves traffic to the closest Edge Locations.
- Customer traffic initially traverses the public internet, but only until it reaches the closest Edge Location. At that point, AWS Global Accelerator takes users' traffic off of the public internet, and onto Amazon's private global network through 90+ global edge locations, which are then directed to your application origins.
	- Traversing the AWS global network yields significantly better performance.
- You can improve the latency and availability of single region applications, allow for simplified and resilient routing for multi-Region applications, as well as accelerated and simplified storage for multi-Region applications.

### Differences From CloudFront
- Global Accelerator moves the AWS network itself closer to customers, while CloudFront specifically moves the content closer by caching it at the Edge Locations.
- Connections enter at edge, using Anycast IPs
- Transit over AWS backbone to 1+ locations
- Global Accelerator is a network product, can can be used for NON HTTP/S (TCP/UDP) - This is a difference from CloudFront
- Global Accelerator doesn't cache anything, and it doesn't understand the protocol for HTTP/S