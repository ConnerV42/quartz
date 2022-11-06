---
title: "AWS Service Catalog"
tags:
- aws
- aws-security
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## AWS Service Catalog
AWS Service Catalog enables a (usually internal user) to directly users or customers to deploy infrastructure with tight control in a self-service way

### What is a Service Catalog?
- A Document or Database created by an IT team
- Organized collection of products
- Offered by the IT Team
- Key Product Information: Product Owner, Cost, Requirements, Support Information, Dependencies
- Manage costs and scale service delivery

### AWS Service Catalog
- Self-Service Portal for 'end users'
- Launch predefined (by admin) products
- End user permissions can be controlled
- Admins can define those products using CloudFormation Templates, as well as configuration of the Service Catalog product
- Admins also define the permissions required to launch the infrastructure that the products use, so this is AWS permissions to, say, launch an EC2 instance or provision a load balancer, or any other permissions used by your products.
- You can also define any end user permissions
	- Who can launch the products
	- Who has visibility of what, within the product
- Build products into portfolios

### Tag Management
To allow administrators to easily manage tags on provisioned products, AWS Service Catalog provides a TagOption library. A TagOption is a key-value pair managed in AWS Service Catalog. It is not an AWS tag but serves as a template for creating an AWS tag based on the TagOption.

The TagOption library makes it easier to enforce the following:
- A consistent taxonomy
- Proper tagging of AWS Service Catalog resources
- Defined, user-selectable options for allowed tags

Administrators can associate TagOptions with portfolios and products. During a product launch (provisioning), AWS Service Catalog aggregates the associated portfolio and product TagOptions, and applies them to the provisioned product