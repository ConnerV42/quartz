---
title: "S3 Static Website Hosting"
tags:
- aws
- s3
- hosting
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Static Website Hosting
- Normally, you'd use AWS S3 APIs to access S3
- However, when S3 Static Website Hosting is enabled, you can access S3 via standard HTTP
	- This allows you to host almost anything
- When enabling, you must set an Index (default page) and Error (default, when something goes wrong) HTML document
- When enabled, a Website Endpoint is created. This name is automatically generated.
- Custom Domain via R53 - Bucket Name matters in this case, it needs to match domain name

### Other Use Cases: Offloading
- Hosting static media for your websites to take pressure off of your compute service
- Cheaper for storage and delivery of static assets

### Other Use Cases: Out-of-band pages
- When scheduled maintenance is happening, and your server is down, customers can be pointed at an out of band page until the server is back up

### Costs
- When S3 Static Website Hosting is enabled, you're charged a certain amount for requests