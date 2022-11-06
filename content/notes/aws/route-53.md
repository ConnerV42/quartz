---
title: "Route 53"
tags:
- aws
- route-53
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Route 53 Health Checks](/notes/aws/route-53-health-checks.md)

## **Route 53**
Route 53 provides 2 primary services
1. Register Domains
2. Host Zone files on managed name servers
	- A zone file is just a database which contains all of the DNS information for a particular domain
	- The zone file, known as a "Hosted Zone", in AWS terminology, is placed on 4 managed database name servers that are distributed globally by AWS
- Global service
- Globally resilient
- Route 53 is basically DNS-as-a-Service
- Hosted Zones can be public or private (linked to one or more VPCs)

### Hosted Zones
- A R53 Hosted Zone is a DNS DB for a domain e.g. connerverret.com
- Globally resilient (multiple DNS servers)
- Created with domain registration via R53 - can be created separately if you want to register a domain elsewhere
- Host DNS Records (e.g. A, AAAA, MX, NS, TXT)
- Hosted Zones are what the DNS system references - Authoritative for a domain e.g. connerverret.com

#### Public Hosted Zone
- A public hosted zone is a container that holds information about how you want to route traffic on the internet for a specific domain which is accessible from the public internet
- DNS Database (zone file) hosted by R53 (Public Name Servers)
- Accessible from the public internet (& VPCs, using Route 53 Resolver)
- Hosted on "4" R53 Name servers (NS) specific for the zone
- Use "NS records" to point at theses NS (connect to global DNS)
- Resource Records (RR) created within the Hosted Zone
- Externally registered domains can point at the R53 Public Zone
- Monthly cost for running a public hosted zone

#### Private Hosted Zone
- A private hosted zone is a container that holds information about how you want Amazon Route 53 to respond to DNS queries for a domain and its subdomains within one or more VPCs that you create with the Amazon VPC service
- A Hosted Zone that isn't public
- Associated with VPCs - Only accessible in those VPCs
- Using different accounts is supported via CLI/API
- Split-view (overlapping public and private) for PUBLIC and INTERNAL use with the same zone name

### CNAME vs R53 Alias
- An "A" records maps a NAME to an IP Address
	- connerverret.com => 1.3.3.7
- CNAME maps a NAME to another NAME
	- www.connerverret.com => connerverret.com
- CNAME is invalid for naked/apex (connerverret.com)
- Many AWS services use a DNS Name (ELBs)
- With just CNAME - connerverret.come => ELB would be invalid

### Route 53 Health Checks
- #wip - on the study guide for the #aws-sysops exams
- Cantrill lecture: https://learn.cantrill.io/courses/aws-certified-sysops-administrator-associate/lectures/27288053

#### Alias Records
- Implemented by AWS, outside the normal DNS standard (only usable if route 53 is hosting your domains)
- ALIAS records map a NAME to an AWS resource
- Can be used for both naked/apex and normal records
- For non apex/naked - functions like CNAME
- There is no charge for ALIAS requests pointing at AWS resource
- For AWS Services - default to picking ALIAS
- Should be the same "Type" as what the record is pointing at
- API Gateway, CloudFront, Elastic Beanstalk, ELB, Global Accelerator & S3

### Geolocation Routing VS. Geoproximity Routing
- **Geolocation Routing** lets you choose the resources that serve your traffic based on the geographical location of your users, meaning the location that DNS queries originate from. For example, you might want all queries from Europe to be routed to an ELB load balancer in the Frankfurt region. When you use geolocation routing, you can localize your content and present some or all of your website in the language of your users. You can also use geolocation routing to restrict distribution of content to only the locations in which you have distribution rights. Another possible use is for balancing load across endpoints in a predictable, easy-to-manage way, so that each user location is consistently routed to the same endpoint.
-  lets you choose the resources that serve your traffic based on the geographical location of the resources.
- **Geoproximity Routing** lets Amazon Route 53 route traffic to your resources based on the geographic location of your users and your resources. You can also optionally choose to route more traffic or less to a given resource by specifying a value, known as a bias. A bias expands or shrinks the size of the geographic region from which traffic is routed to a resource.
### Weighted Routing
- **Weighted Routing** lets you associate multiple resources with a single domain name (conner.com) or subdomain name (subdomain.conner.com) and choose how much traffic is routed to each resource.
	- This can be useful for a variety of purposes, including load balancing and testing new versions of software. You can set a specific percentage of how much traffic will be allocated to the resource by specifying the weights.
	- For example, if you want to send a tiny portion of your traffic to one resource and the rest to another resource, you might specify weights of 1 and 255. The resources with a weight of 1 gets 1/256th of the traffic (1/1+255), and the other resource get 255/256ths (255/1+255).
	- You can gradually change the balance by changing the weights. If you want to stop sending traffic to a resource, you can change the weight for that record to 0.
### Latency Routing
- Latency Routing lets Amazon Route 53 serve user requests from the AWS Region that provides the lowest latency. It does not, however, guarantee that users in the same geographic region will be served from the same location.
### S3 Bucket Website Routing
- Here are the prerequisites for routing traffic to a website that is hosted in an Amazon S3 Bucket:
	- An S3 bucket that is configured to host a static website. The bucket must have the same name as your domain or subdomain. For example, if you want to use the subdomain portal.conner.com, the name of the bucket must be portal.conner.com
	- A registered domain name. You can use Route 53 as your domain registrar, or you can use a different registrar.
	- Route 53 as the DNS service for the domain. If you register your domain name by using Route 53, we automatically configure Route 53 as the DNS service for the domain.
- You can create a new Route 53 with the failover option to a static S3 website bucket or CloudFront distribution as an alternative.
### Routing Traffic to a Load Balancer
- To route domain traffic to an ELB load balancer, use Amazon Route 53 to create an alias record that points to your load balancer. An alias record is a Route 53 extension to DNS. It's similar to a CNAME record, but you can create an alias record both for the root domain, such as conner.com, and for subdomains, such as portal.conner.com. (You can create CNAME records only for subdomains). To enable IPv6 resolution, you would need to create a second resource record, conner.com ALIAS AAAA -> myelb.us-west-2.elb.amazonnaws.com, this is assuming your Elastic Load Balancer has IPv6 support.## **Route 53**
### Geolocation Routing VS. Geoproximity Routing
- **Geolocation Routing** lets you choose the resources that serve your traffic based on the geographical location of your users, meaning the location that DNS queries originate from. For example, you might want all queries from Europe to be routed to an ELB load balancer in the Frankfurt region. When you use geolocation routing, you can localize your content and present some or all of your website in the language of your users. You can also use geolocation routing to restrict distribution of content to only the locations in which you have distribution rights. Another possible use is for balancing load across endpoints in a predictable, easy-to-manage way, so that each user location is consistently routed to the same endpoint.
-  lets you choose the resources that serve your traffic based on the geographical location of the resources.
- **Geoproximity Routing** lets Amazon Route 53 route traffic to your resources based on the geographic location of your users and your resources. You can also optionally choose to route more traffic or less to a given resource by specifying a value, known as a bias. A bias expands or shrinks the size of the geographic region from which traffic is routed to a resource.
### Weighted Routing
- **Weighted Routing** lets you associate multiple resources with a single domain name (conner.com) or subdomain name (subdomain.conner.com) and choose how much traffic is routed to each resource.
	- This can be useful for a variety of purposes, including load balancing and testing new versions of software. You can set a specific percentage of how much traffic will be allocated to the resource by specifying the weights.
	- For example, if you want to send a tiny portion of your traffic to one resource and the rest to another resource, you might specify weights of 1 and 255. The resources with a weight of 1 gets 1/256th of the traffic (1/1+255), and the other resource get 255/256ths (255/1+255).
	- You can gradually change the balance by changing the weights. If you want to stop sending traffic to a resource, you can change the weight for that record to 0.
### Latency Routing
- Latency Routing lets Amazon Route 53 serve user requests from the AWS Region that provides the lowest latency. It does not, however, guarantee that users in the same geographic region will be served from the same location.
### S3 Bucket Website Routing
- Here are the prerequisites for routing traffic to a website that is hosted in an Amazon S3 Bucket:
	- An S3 bucket that is configured to host a static website. The bucket must have the same name as your domain or subdomain. For example, if you want to use the subdomain portal.conner.com, the name of the bucket must be portal.conner.com
	- A registered domain name. You can use Route 53 as your domain registrar, or you can use a different registrar.
	- Route 53 as the DNS service for the domain. If you register your domain name by using Route 53, we automatically configure Route 53 as the DNS service for the domain.
- You can create a new Route 53 with the failover option to a static S3 website bucket or CloudFront distribution as an alternative.

### Routing Traffic to a Load Balancer
- To route domain traffic to an ELB load balancer, use Amazon Route 53 to create an alias record that points to your load balancer. An alias record is a Route 53 extension to DNS. It's similar to a CNAME record, but you can create an alias record both for the root domain, such as conner.com, and for subdomains, such as portal.conner.com. (You can create CNAME records only for subdomains). To enable IPv6 resolution, you would need to create a second resource record, conner.com ALIAS AAAA -> myelb.us-west-2.elb.amazonnaws.com, this is assuming your Elastic Load Balancer has IPv6 support.

### DNS Record Types
- Nameserver (NS) Records - How delegations works
- A and AAAA Records - Map hostnames to IP addresses
	- A maps host to IPv4 addresses
	- AAAA maps host to IPv6 addresses
- CNAME Records - Creates host to host records
	- For example, you might have 3 CNAME records for a single IP address, that all resolve to the same server. This is typically done to reduce admin overhead. CNAME records map to an A or AAAA record.
		- ftp
		- mail
		- www
- MX Records - Find a mail server (SMTP) for a domain
	- Consists of 2 parts
		- Priority
		- Value - A host inside the zone, or a host outside the zone
- TXT Records - Prove domain ownership by adding a record to the domain so that a 3rd party can verify domain ownership, fights spam by indicating which identities are authorized
- DNS records also have TTL values, which determine how long DNS is cached on the resolver server. High TTL values are recommended to be lowered prior to doing any DNS migration.