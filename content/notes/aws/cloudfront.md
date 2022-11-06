---
title: "CloudFront"
tags:
- aws
- aws-content-delivery
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **CloudFront**
- CloudFront is a Content Delivery network (CDN) within AWS
- Improves the delivery of content by caching and using an efficient global network
- When a user makes a request, the following happens in the CloudFront Network:
	1. Checks closest Edge Location. If a cache hit, response is returned.
	2. Else, if a cache miss occurs, checks Regional Edge Cache. If a cache hit, response is returned.
	3. Else, if another cache miss occurs, fetches from Origin. Writes to both Regional Edge Cache, and Edge Location
- CloudFront integrates with ACM (AWS Certificate Manager) so that you can use SSL certificates with CloudFront (enabling HTTPS)
- **CloudFront is for download operations only.** Any uploads go direct to the origin. CloudFront performs read-only caching.

### Helpful Links
- [CloudFront Pricing](https://aws.amazon.com/cloudfront/pricing/)

###  Origin 
- The source location of where your content lives
- Can either be an S3 Origin or a Custom Origin (Anything else which runs a web server, and has a publicly routable IPv4 address)
- Origin Groups provides resiliency across origins in a failover event

#### Origin Types
- S3 buckets (Note: An S3 bucket used for static website hosting, is viewed as a custom origin)
- Webservers (custom origins)
- AWS MediaPackage channel endpoints
- AWS MediaStore container endpoints

##### S3
- Origin Path: Allows specification of a particular path in the bucket to be used as the top level
- Origin Access Identity: Allows you to give CloudFront a virtual identity, which it can use to access the S3 origin. Only works for S3 origins.
- S3 Origins have the same Viewer Protocol and Origin Protocol

##### Custom Origins
Allows you to configure the following (which isn't possible on S3 origins)
- Minimum Origin SSL Protocol
- Origin Protocol Policy (HTTP, HTTPS, Match Viewer Protocol Policy)
- HTTP Port (can be custom)
- HTTPS Port (can be custom)
- Cannot use Origin Access Identities

### Security - Origin Side (Between Origin & Edge Location)
Securing S3 Origins:
- An OAI is a type of identity
- It can be associated with CloudFront Distributions
- CloudFront becomes that OAI
- That OAI can be used in S3 Bucket Policies
- DENY all BUT one or more OAI's

Securing Custom Origins:
- Configuring Origin to require the presence of a Custom Header. Because we use HTTPS, nobody can spy on the headers, and the headers come from the Edge Location
- Use a firewall that is configured to allow requests from CloudFront IPs, but nowhere else.
- These approaches can be used in combination

### Security - Viewer Side (Between Edge Location & Customer)
- CloudFront can run in private or public mode (default is public)
- Requests made to CloudFront must be made with signed URL or signed cookie in private mode
- 1 Behavior - Whole Distribution PUBLIC or PRIVATE
- Multiple Behaviors - each is PUBLIC or PRIVATE
- A CloudFront Key is created by an Account Root User
- That account is added as a TRUSTED SIGNER
- What generates pre-signed cookies or URLs? Usually an application like an API Gateway with a Lambda Signer

#### Signed URL
- URL provides access to one object per URL
- Legacy RTMP distributions can't use cookies
- Use URLs if your client doesn't support cookies

#### Signed Cookies
- Cookies provide access to groups of objects
- Use for groups of files/all files of a type - e.g. all cat gifs
- Use if maintaining URLs is important in your application

### Distribution 
- The configuration unit of CloudFront (Can have multiple origins, configured inside the distribution)
	- Distributions can be configured to use an alternate domain name, like `http://connerverret.com`
	- Once you've configured a Distribution, you can deploy that Distribution to the CloudFront Network, pushing it to all chosen Edge Locations, allowing the Edge Locations to be used by your customers, because the Edge Locations now have the configuration stored within the Distribution.
	- Most important configuration are actually configured with Behaviors, which is a sub-configuration within a Distribution.

### Behavior (Cache Behavior, sub-configuration for a Distribution)
- CloudFront Behaviors control much of the TTL, protocol, and privacy settings within CloudFront
- A single distribution can have multiple behaviors
- Includes a default(`*`) path pattern behavior (matches anything not matched by a more specific behavior)
- For any requests incoming to an Edge Location, these requests are pattern matched against any behaviors in use for that Distribution using the path pattern
- Once a path pattern is matched, it is then subject to any of the options, such as
	- Origin or Origin Group to use
	- Viewer Protocol Policy (HTTP and HTTPs, Redirect HTTP to HTTPS, or HTTPS only)
	- Allowed HTTP Methods (GET + HEAD, GET + HEAD + OPTIONS, GET + HEAD + OPTIONS + PUT + POST + PATCH + DELETE)
	- Field-level encryption
	- Cached HTTP Methods (GET, HEAD are cached by default)
	- Cache and origin request settings
	- Cache based on request headers (none, whitelist, or all)
	- **Restrict viewer access to a behavior**, uses Signed URLs or signed Cookies (**Important for exam**)
		- This requires Trusted Signers, which are accounts that able to generate a signed URL or signed Cookie
	- Associate Lambda @ Edge functions
- Origins are used by behaviors as content sources
- A distribution can have many behaviors which are configured with a path pattern, If requests match that pattern, that behavior is used, otherwise the default is used, for example private (`img/*`)
- Origins, Origin Groups, TTL, Protocol Policies, restricted access are configured via Behaviors
- Each behavior can have a precedence, with the default starting at 0 (lowest priority)

#### TTL and Invalidations
- Used to influence how long objects are stored at Edge Locations, and when they're ejected
- When a cached object expires at an Edge Location, it isn't immediately discarded, but it becomes stale. If another customer requests this object, the request will be forwarded to the origin. If the origin detects that the stale object is still current, the origin returns a `304 Not Modified` to the Edge Location. If there a newer version, a `200 OK` is returned, along with the new version of the object.
- More frequent cache hits = lower origin load (better user performance)
- Default TTL is 24 hours (validity period, defined as a behavior)
- You can set Minimum TTL and Maximum TTL values on a behavior (Lower and upper bounds for any TTLs set on an individual object basis using headers)
- Object specific TTL values can be controlled used headers
	- Origin Header: `Cache-Control max-age` (seconds)
	- Origin Header: `Cache-Control s-maxage` (seconds) (same exact functionality as the above header)
	- Origin Header: `Expires` (Date & Time)
	- If any of these per object TTL values exceed either the Minimum TTL or Maximum TTL thresholds, the per object TTL is ignored, and either the Minimum or Maximum TTL is used
- These values are either set on Custom Origin, or in the case of S3, set via object metadata (Can be set using the S3 API, CLI, or Console UI)
- Cache Invalidation - performed on a Distribution, applies to all edge locations (takes time)
- A specific object can be invalidated using a specific path or wildcard
	- `/images.whiskers1.jpg`
	- `/images.whiskers*`
	- `/images/*`
	- `/*` - Invalidates all objects in a Distribution
- Cache Invalidation has a cost, so it should only be done to correct an error. If you fine yourself using it all the time, look into using versioned file names.
- Versioned file names are better, because even if the objects are cached in a customers browser, you'll still get a new object. Also makes log files more useful.
- Don't confuse versioned file names with S3 versioning

### Edge Locations
- Local cache of your data (Names of the pieces of global infrastructure where your content is cached. Located globally, in or around large cites, over 200 Edge Locations)
	- Smaller than region, generally in 3rd party data centers, primarily used for storage and caching of data
	- Cannot be used for an EC2 instance, for example

### Regional Edge Cache
- Larger versions of an edge location. Provides another layer of caching. Less of these exist, compared to Edge Locations. 
	- These exist in between the Edge Locations and the Origin. 
	- Support a number of Edge Locations in the same geographical area


  - By using AWS WAF, you can configure web access control lists (Web ACLs) on your CloudFront distributions or Application Load Balancers to filter and block requests based on requests signatures. Each Web ACL consists of rules that you can configure to string match or regex match one or more request attributes, such as the URI, query-string, HTTP method, or header key. In addition, by using AWS WAF's rate-based rules, you can automatically block the IP addresses of bad actors when requests matching a rule exceed a threshold you define. It is recommend that you add web ACls with rate-based rules as part of your AWS Shield Advanced protection. These rules can alert you to sudden spikes in traffic that might indicate a potential DDoS.
- CloudFront signed URLs and signed cookies provide the same basic functionality: they allow you to control who can access your content. If you want to serve private content through CloudFront and you're trying to decide whether to use signed URLs or signed cookies, consider the following:
	- Use **signed URLs** for the following cases:
		- You want to use an RMTP distribution. Signed cookies aren't supported for RMTP distributions.
		- You want to restrict access to individual files, for example, an installation download for your application.
		- Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.
	- Use **signed cookies** for the following cases:
		- You want to provide access to multiple restricted files, for example all of the files for a video in HLS format or all the files in the subscriber's area of a website.
		- You don't want to change your current URLs.

### CloudFront SSL
- CloudFront Default Domain Name (CNAME)
	- Appears in this format: `https://d111111abcdef8.cloudfront.net/` or `http://d111111abcdef8.cloudfront.net`
	- SSL is supported by default ... `*.cloudfront.net` certificate (covers all default CF distributions, but most of the time, you'll want your own domain name)
- You must verify ownership (optionally HTTPS) using a matching certificate
- This can be done with the ACM, but it must be done in the us-east-1 region, since it's a global service
- Behaviors can be set to allow HTTP or HTTPS, HTTP to HTTPS redirection, or restricted to HTTPS only (causes HTTP requests to fail)
- Two SSL Connections: Viewer => CloudFront and CloudFront => Origin
	- Both of these connections need valid public certificates (and intermediate certs)
		- Self-signed certificates do not work with CloudFront, only public certificates can be used with CloudFront
- Old browsers don't support SNI ... to support these older browsers, a dedicated IP is required, which CF charges extra for
- SNI mode is free as part of the service, but a dedicated IP can be used at an Edge Location to support HTTPS on older browsers ($600 per month)
- The Viewer Protocol (Browser to CloudFront) and Origin Protocol (CloudFront to Origin) BOTH require a public SSL certificate
- If you use an ALB, it needs a publicly trusted certificate (ACM)
- If you use a Custom Origin, you need a publicly trusted certificate, but it cannot be generated with ACM, because ACM doesn't support Custom Origins
- The certificate needs to match the DNS name of the origin

#### History of SSL
- Historically (before 2003), every SSL enabled site needed its own IP
- Encryption starts at the TCP connection
- Host headers happens after that - Layer 7 / Application Layer
- In 2003, an extension was added to TLS called SNI, which allows a host to be included
- Server Name Indication adds the ability for a client to tell a server which domain name its attempting to access. This occurs within the TLS handshake, so before HTTP even gets involved
- Resulting in many SSL certs/hosts using a shared IP

### CloudFront Geo Restriction
- Restrict content to a particular location
- CF - Whitelist or Blacklist - Only works with countries. Uses GeoIP Database 99.8%+
- Applies to the entire distribution

### CloudFront 3rd Party Geolocation
- Completely customizable
- Requires a piece of compute in the architecture that acts as a decider
- Requires CloudFront distribution to be configured as private
- Requires either a signed URL or signed cookie
- Can be used to allow/deny based on anything the application has exposure to

### Lambda@Edge
- Allows you to run lightweight Lambda functions at edge locations
- Adjust data between the Viewer & Origin
- Don't have full Lambda feature set
- Only supports Node and Python
- Runs in the AWS Public Space (not VPC)
- Layers are not supported
- Lambda can run during any of these periods
	- Viewer Request (Runs after CloudFront receives a request from a viewer)
	- Origin Request (Runs before CloudFront forwards a request to an origin)
	- Origin Response (Runs after CloudFront receives a response from an origin)
	- Viewer Response (Runs before response is forwarded to viewer)

Use Cases:
- A/B testing - Viewer Request - 2 different versions of an image
- Migration between S3 Origins - Origin Request - Based on a weighted value
- Different objects based on device - Origin Request
- Content By Country - Origin Request