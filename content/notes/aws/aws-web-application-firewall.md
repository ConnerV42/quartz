---
title: "AWS Web Application Firewall"
tags:
- aws
- aws-security
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [AWS Firewall Manager](/notes/aws/aws-firewall-manager.md)

## **Web Application Firewall (WAF)**

- AWS Implementation of Layer 7 firewall (HTTP/HTTPS)
- The WAF Web ACL can be associated with Global resources like CloudFront, and regional resources, like ALB, API Gateway, etc. to integrate with WAF.

### Web Access Control Lists
- Controls if traffic is blocked or allowed
- Web ACL has a default action of ALLOW or BLOCK
- Resource Type - CloudFront or Regional Service
	- ALB, API GW, AppSync requires picking a region
	- CloudFront is global
- Add Rule Groups or Rules, which are processed in order
- Web ACL Capacity Units (WCU) - Default 1500
	- Can be increased via support ticket
- Web ACL's are associated with resources (this can take time)
	- Adjusting a Web ACL takes less time than associating one
	- A resource can have one Web ACL, but one Web ACL can be associated with many resources
	- You cannot associate a CloudFront Web ACL with a regional resource
- Can be updated manually, or in an automated fashion using EventBridge to achieve Lambda Event Driven processing of logs or event subscriptions
	- CloudWatch Logs
	- S3
	- Firehose

### Rule Groups
- Contain rules, used by Web ACL as an admin container for rules
- They don't have default actions, they're added to Web ACL's and the Web ACL itself has a default action.
- Managed (AWS or Marketplace), Yours, Service Owned (i.e. Shield & Firewall Manager)
	- Managed Rule Groups are free for AWS WAF customers
- Rule Groups obtained via Marketplace usually have subscriptions
- Rule Groups can be referenced by multiple Web ACL (separate entity)
- Have a WCU capacity (defined upfront, max 1500)

### Rules
- Structure
	- Type
		- Regular - Designed to match if something occurs
		- Rate-based - Designed to match if something occurs at a regular rate
			- Do something if someone tries to connect via SSH 5000 times within an interval
	- Statement: One or more things that match traffic or not
		- Defines (WHAT to match) or (COUNT ALL) or (WHAT & COUNT)
			- origin country, IP, label, header, cookies, query parameter, URI path, query string, body (first 8,192 bytes ONLY), HTTP method
			- Single, AND, OR, NOT
			- Example: Incoming TCP port 80
	- Action: What does WAF do if a rule is matched
		- Allow* (not valid for rate-based rules, custom header only)
		- Block (custom header and custom response)
		- Count (custom header only)
		- Captcha (custom header only)
		- Custom Response (optional, prefixed with `x-amzn-waf-` - used so that application can react to traffic which has been affected by your rule)
		- Label - Can be referenced later in the same Web ACL (internal to WAF only, don't persist outside)
		- ALLOW & BLOCK stop processing, Count/Captcha actions continue
		
### Pricing
- Web ACL - Monthly ($5 per month per Web ACL)
- Rule on Web ACL - Monthly ($1 per month per rule)
- Requests per Web ACL - Monthly ($0.60 per month per 1 million requests)
- Intelligent Threat Mitigation
- Bot Control - ($10 per month) & ($1 per 1 million requests per month)
- Captcha - ($0.40 / 1,000 challenge attempts)
- Fraud Control/Account Takeover ($10 per month & $1 per 1,000 login attempts)
- Marketplace Rule Groups - Extra costs

### Misc. Notes
- AWS WAF is a web application firewall that lets you monitor HTTP and HTTPS requests that are forwarded to an API Gateway API, CloudFront, or an Application Load Balancer. AWS WAF also lets you control access to your content. Based on conditions that you specify, such as the IP addresses that requests originate from or the values of query strings, API Gateway, CloudFront or an ALB response to requests either with their requested content or with an HTTP 403 status code (forbidden). You also can configure CloudFront to return a custom error page when a request is blocked.
- At the simplest level, AWS WAF lets you choose one of the following behaviors:
	- Allow all requests except the ones that you specify - This is useful when you want CloudFront or an Application Load Balancer to serve content for a public website, but you also want to block requests from attackers.
	- Block all requests except the ones that you specify - This is useful when you want to serve content for a restricted website whose users are readily identifiable by properties in web requests, such as the IP address that they use to browse the website.
	- Count the requests that match the properties that you specify - When you want to allow or block requests based on new properties in web requests, you first can configure AWS WAF to count the requests that match those properties without allowing or blocking those requests. This lets you confirm that you didn't accidentally configure AWS WAF to block all the traffic to your website. When you're confident that you specified the correct properties, you can change the behavior to allow or block requests.
- AWS WAF is tightly integrated with Amazon CloudFront, the Application Load Balancer (ALB), API Gateway, AWS AppSync - services that AWS customers commonly use to deliver content for their websites and applications. When you use AWS WAF on CloudFront, your rules run in all AWS Edge Locations, located around the world close to your end-users. This means security doesn't come at the expense of performance. Blocked requests are stopped before they reach your web servers. When you use AWS WAF on regional services, such as Application Load Balancer, Amazon API Gateway, and AWS AppSync, your rules run in the region and can be used to protect Internet-facing resources as well as internal resources.
- A rate-based rule tracks the rate of requests for each originating IP address and triggers the rule action on IPs with rates that go over a limit. You set the limit as the number of requests per 5-minute time span. You can use this type of rule to put a temporary block on requests from an IP address that's sending excessive requests.