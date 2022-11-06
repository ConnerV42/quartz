---
title: "S3 CORS"
tags:
- aws
- s3
- cors
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 CORS
- Same-origin requests are always allowed, defined on initial request.
	- Example is an S3 bucket that loads an index.html file and some javascript
		- This will be allowed, but if the index.html file or javascript calls ANOTHER origin, such as an API Gateway, or different S3 bucket, those requests will fail.
- Cross origin requests are restricted by default in S3
- CORS configurations need to be defined on the API Gateway or the other S3 bucket
	- The specific origin or \* must be defined or the request will fail
- Access-Control-Allow-Origin: Either contains a wild card or a particular origin
- Access-Control-Max-Age: Indicates how long the results of a preflight request can be cached
- Access-Control-Allow-Methods: Either contains a wild card or a list of methods that can be used for cross origin request - GET, POST, DELETE, PUT, etc.
- Access-Control-Allow-Headers: Contained in a CORS Configuration and within the response to a preflight request. Indicates which HTTP headers can be used within the actual request.
- Preflight & Preflighted requests
	- Your browser sends an HTTP request to the other origin, and it will determine if the request that you're actually making is safe to send