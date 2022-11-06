---
title: "AWS Wiki"
tags:
- aws
disableToc: false
---

## AWS Wiki

### Helpful Links
- [AWS Pricing Calculator](https://calculator.aws/#/)
- [AWS Documentation](https://docs.aws.amazon.com/index.html?nc2=h_ql_doc_do_v)
- [AWS Extend Switch Roles](https://github.com/tilfinltd/aws-extend-switch-roles)

---

## AWS Services
- [AWS Artifact](/notes/aws/aws-artifact.md)
- [API Gateway](/notes/aws/api-gateway.md)
- [AWS AppSync](/notes/aws/aws-appsync.md)
- [Athena](/notes/aws/athena.md)
- [Aurora](/notes/aws/aurora.md)
- [AWS Backup](/notes/aws/aws-backup.md)
- [AWS Batch](/notes/aws/aws-batch.md)
- [AWS Cost Explorer](/notes/aws/aws-cost-explorer.md)
- [AWS Certificate Manager](/notes/aws/aws-certificate-manager.md)
- [Cloud Map](/notes/aws/cloud-map.md)
- [CloudFront](/notes/aws/cloudfront.md)
- [CloudHSM](/notes/aws/cloudhsm.md)
- CloudEndure
- [Data Lifecycle Manager](/notes/aws/data-lifecycle-manager.md)
- [DataSync](/notes/aws/datasync.md)
- [AWS Directory Service](/notes/aws/aws-directory-service.md)
- [EBS](/notes/aws/ebs.md)
- Elasticsearch
- [EFS](/notes/aws/efs.md)
- [EKS](/notes/aws/eks.md)
- [Elastic Beanstalk](/notes/aws/elastic-beanstalk.md)
- [Elastic Map Reduce](/notes/aws/elastic-map-reduce.md)
- [FSx for Windows File Server](/notes/aws/fsx-for-windows-file-server.md)
- [FSx for Lustre](/notes/aws/fsx-for-lustre.md)
- [AWS Glue](/notes/aws/aws-glue.md)
- [IoT core](/notes/aws/iot-core.md)
- [Kinesis](/notes/aws/kinesis.md)
- [MQ](/notes/aws/mq.md)
- Amazon Neptune
- [Redshift](/notes/aws/redshift.md)
- [Resource Access Manager](/notes/aws/resource-access-manager.md)
- [Route 53](/notes/aws/route-53.md)
- [AWS Service Catalog](/notes/aws/aws-service-catalog.md)
- [AWS Shield](/notes/aws/aws-shield.md)
- [AWS SWF](/notes/aws/aws-swf.md)
- [Simple Queue Service](/notes/aws/simple-queue-service.md)
- [AWS Snowball](/notes/aws/aws-snowball.md)
- [Simple Notification Service](/notes/aws/simple-notification-service.md)
- [Storage Gateway](/notes/aws/storage-gateway.md)
- [WorkDocs](/notes/aws/workdocs.md)
- [AWS WorkSpaces](/notes/aws/aws-workspaces.md)

### Compute
- [Lambda](/notes/aws/lambda.md)
- [EC2](/notes/aws/ec2.md)
- [ECS](/notes/aws/ecs.md)
- [Fargate](/notes/aws/fargate.md)

### Serverless Workflows
- [AWS Step Functions](/notes/aws/aws-step-functions.md)

### S3
- [S3](/notes/aws/s3/s3.md)
- [S3 Select](/notes/aws/s3/s3-select.md)
- [Macie](/notes/aws/macie.md) - ML-based Data Classification in S3

### Resource Provisioning and Deployment Automation
- [CloudFormation](/notes/aws/cloudformation.md)
- [CodeBuild](/notes/aws/codebuild.md)
- CodePipeline
- CodeDeploy
- [CDK](https://docs.aws.amazon.com/cdk/index.html)
- [Systems Manager](/notes/aws/systems-manager.md)
- [OpsWorks](/notes/aws/opsworks.md)

### Cache
- [Elasticache](/notes/aws/elasticache.md)

### Database
- [DynamoDB](/notes/aws/dynamodb.md)
- [DynamoDB Accelerator](/notes/aws/dynamodb-accelerator.md)
- [RDS](/notes/aws/rds.md)

### Monitoring
- [CloudWatch](/notes/aws/cloudwatch.md)
- [AWS Config](/notes/aws/aws-config.md)
- [CloudTrail](/notes/aws/cloudtrail.md)
- [AWS X-Ray](/notes/aws/aws-x-ray.md)

### User Management & Security
- [IAM](/notes/aws/iam.md)
- [Organizations](/notes/aws/organizations.md)
- [IAM Identity Center](/notes/aws/iam-identity-center.md)
- Cognito
- [AWS Control Tower](/notes/aws/aws-control-tower.md)

### Security
- [Amazon GuardDuty](/notes/aws/amazon-guardduty.md)
- [AWS Firewall Manager](/notes/aws/aws-firewall-manager.md)
- [AWS Web Application Firewall](/notes/aws/aws-web-application-firewall.md)
- [AWS KMS](/notes/aws/aws-kms.md)

### Networking
- [Direct Connect](/notes/aws/direct-connect.md)
- [VPC](/notes/aws/vpc.md)
- [NACLs and Security Groups](/notes/aws/nacls-and-security-groups.md)
- [VPC Router](/notes/aws/vpc-router.md)
- [VPC Peering](/notes/aws/vpc-peering.md)
- [Internet Gateway](/notes/aws/internet-gateway.md)
- [VPC Gateway Endpoints](/notes/aws/vpc-gateway-endpoints.md)
- [VPC Interface Endpoints](/notes/aws/vpc-interface-endpoints.md)
- [Egress Only Internet Gateway](/notes/aws/egress-only-internet-gateway.md)
- [NAT Gateway](/notes/aws/nat-gateway.md)
- [VPN CloudHub](/notes/aws/vpn-cloudhub.md)
- [Transit Gateway](/notes/aws/transit-gateway.md)
- [PrivateLink](/notes/aws/privatelink.md)
- [Site-to-Site VPN](/notes/aws/site-to-site-vpn.md)
- [Global Accelerator](/notes/aws/global-accelerator.md)

# Miscellaneous

## Public vs Private vs Multi vs Hybrid Cloud
- Public Cloud = using 1 public cloud
- Private Cloud = using on-premises *real* cloud
- Multi-Cloud = using more than 1 public cloud
- Hybrid Cloud = Public and Private Clouds
	- Hybrid Cloud is NOT public cloud + legacy on-premises

## **Security Best Practices**
- Q: An application is hosted in an Auto Scaling group of EC2 instances and a Microsoft SQL Server on Amazon RDS. This is a requirement that all in-flight data between your web servers and RDS should be secured.
	- Force all connections between your DB instance to use SSL by setting the rds.force_ssl parameter to true. Once done, reboot your DB instance.
	- Download the Amazon Root CA certificate. Import the certificate to your servers and configure your application to use SSL to encrypt the connection to RDS.
## **Establishing a site-to-site VPN connection**
  - By default, instances that you launch into a virtual private cloud (VPC) can't communicate with your own network. You can enable access to your network from your VPC by attaching a virtual private gateway to the VPC, creating a custom route table, updating your security group rules, and creating an AWS managed VPN connection.
  - Although the term VPN connection is a general term, in the Amazon VPC documentation, a VPN connection refers to the connection between your VPC and your own network. AWS supports Internet Protocol security (IP sec) VPN connections.
  - A **customer gateway** is a physical device or software application on your side of the VPN connection.
  - To create a VPN connection, you must create a customer gateway resource in AWS, which provides information to AWS about your customer gateway device. Next, you have to set up an Internet-routable IP address (static) of the customer gateway's external interface.
  - An AWS VPC needs an attached virtual private gateway, and your remote network includes a customer gateway, which you must configure to enable the VPN connection. You set up the routing so that any traffic from the VPC bound for your network is routed to the virtual private gateway.
### **Auto Scaling**
  - **What is the default termination policy for Auto Scaling groups?**
    - The default termination policy is designed to ensure that your network architectures spans AZ's evenly. The default behavior is as follows:
      - If there are instances in multiple AZ's, choose the AZ with the most instances, and at least one instance that is not protected from scale in. If there is more than one AZ with this number of instances, choose the AZ with the instances that use the oldest launch configuration.
      - Determine which unprotected instances in the selected AZ use the oldest launch configuration. If there is one such instance, terminate it.
      - If there are multiple instances to terminate based on the above criteria, determine which unprotected instances are closest to the next billing hour. If there is one such instance, terminate it.
      - If there is more than one unprotected instance closest to the next billing hour, choose one of these instances at random.
  - An Auto Scaling Group contains a collection of EC2 instances that are treated as a logical grouping for the purposes of automatic scaling and management. An Auto Scaling group also enables you to use EC2 Auto Scaling features such as health check replacements and scaling policies. Both maintaining the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the EC2 Auto Scaling service. The size of an Auto Scaling group depends on the number of instances that you set as the desired capacity. You can adjust its size to meet demand, either manually or by using automatic scaling. Step scaling polices and simple scaling polices are two of the dynamic scaling options available for you to use. Both requires you to create CloudWatch alarms for the scaling policies. Both require you to specify the high and low thresholds for the alarms. Both require you to define whether to add or remove instances, and how many, or set the group to an exact size. The main differences between the policy types is the step adjustments that you get with step scaling policies. When step adjustments are applied, and they increase or decrease the current capacity of your Auto Scaling group, the adjustments vary based on the size of the alarm breach.
  - **Simple Scaling** lets you increase or decrease the current capacity of the group based on a single scaling adjustment. The primary issue with **simple scaling** is that after a scaling activity is started, the policy must wait for the scaling activity or health check replacement to complete and the cooldown period to expire before responding to additional alarms. Cooldown periods help to prevent the initiation of additional scaling activities before the effects of previous activities are visible. 
  - With a **Target Tracking Scaling** policy, you can increase or decrease the current capacity of the group based on a target value for a specific metric. The policy will help resolve the over-provisioning of your resources. The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value. In addition to keeping the metric close to the target value, a target tracking scaling policy also adjusts to changes in the metric due to a changing load pattern.
  - In Auto Scaling, the following statements are correct regarding the cooldown period:
    1. It ensures that the Auto Scaling group does not launch or terminate additional EC2 instances before the previous scaling activity takes effect.
    2. Its default value is 300 seconds.
    3. It is a configurable setting for your Auto Scaling group.
  - A launch configuration is a template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances such as the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping.
  - You can only have one region per load balancer. ELB is designed to only run in one region, not multiple regions.
  - You can use the dynamic and predictive scaling features of EC2 Auto Scaling to add or remove EC2 instances. Dynamic scaling responds to changing demand and predictive scaling automatically schedules the right number of EC2 instances based on predicted demand. Dynamic scaling and predictive scaling can be used together to scale faster.
	- **Step Scaling** allows you to increase or decrease the current capacity of the group based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach. You can set multiple actions to vary the scaling depending on the size of the alarm breach. When you create a step scaling policy, you can also specify the number of seconds that it takes for a newly launched instance to warm up.
## **Elastic Load Balancing**
- [What is Elastic Load Balancing?](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)
### Multi-Region support
- Load Balancers do not support multi-region. However, this can be accomplished with Route 53 and Load Balancers together. #multi-region
### **Application Load Balancers**
- Application Load Balancers support Weighted Target Groups routing. With this feature, you will be able to do weighted routing of the traffic forwarded by a rule to multiple target groups. This enables various use cases like blue-green, canary, and hybrid deployments without the need for multiple load balancers. It even enables zero-downtime migration between on-premises and cloud or between different compute types like EC2 and Lambda.
	- When you create a target group in your Application Load Balancer, you specify its target type. This determines the type of target you specify when registering with this target group. You can select the following target types:
		1. instance - The targets are specified by instance ID.
		2. ip - The targets are IP addresses.
		3. Lambda - The target is a Lambda function.
- Host-based Routing: You can route a client request based on the Host field of the HTTP header allowing you to route to multiple domains from the same load balancer.
- Path-based Routing: You can route a client request based on the URL path of the HTTP header.
- HTTP header-based routing: You can route a client request based on the value of any standard of custom HTTP header.
- HTTP method-based routing: You can route a client request based on any standard or custom HTTP method.
- Query string parameter-based routing: You can route a client request based on query string or query parameters.
- Source IP address CIDR-based routing: You can route a client request based on source IP address CIDR from where the request originates.
- Handles WebSocket connections.
- **Path-based Routing**: If your application is composed of several individual services, an ALB can route a request to a service based on the content of the request such as Host field, Path URL, HTTP header, HTTP method, Query string, or Source IP address. Path-based routing allows you to route a client request based on the URL path of the HTTP header. Each path condition has one path pattern. If the URL in a request matches the path pattern in a listener rule exactly, the request is routed using that rule.
### **Network Load Balancers**
- Handles WebSocket connections.
- Does not support path-based routing or host-based routing.
### **ALB vs NLB**   
- You typically want an ALB for an HTTP/HTTPS web application
- NLB's work at layer 4 only and can handle TCP and UDP. Its main feature is very high performance. It uses static IP addresses and can be assigned Elastic IPs, not possible with with ALB or ELB. NLB would be used for anything that ALBs don't cover, like near real-time data streaming services (video, stock quotes, etc.). Another use case is if your application uses non-HTTP protocols.
- Q: A client is hosting their company website on a cluster of web servers that are behind a public-facing load balancer. The client also uses Amazon Route 53 to manage their public DNS. How should the client configure the DNS zone apex record to point to the load balancer?
	- Create an A record aliased to the load balancer DNS name.
	- _Why?_: Route 53's DNS implementation connects user requests to infrastructure running inside (and outside) of AWS. For example, if you have multiple web servers running on EC2 instances behind an Elastic Load Balancing load balancer, Route 53 will route all traffic addressed to your website (e.g. www.tutorialsdojo.com) to the load balancer DNS name (e.g. elbtutorialsdojo123.elb.amazonaws.com). Additionally, Route 53 supports the alias resource record set, which lets you map your **zone apex** (e.g. tutorialsdojo.com) DNS name to your load balancer DNS name. IP addresses associated with Elastic Load Balancing can change at any time due to scaling or software updates. Route 53 responds to each request for an Alias record set with one IP address for the load balancer.
### **Alias records**
In the response to a dig or nslookup query, an alias record is listed as the record type that you specified when you created the record, such as A or AAAA.

## #sysops Scenarios
#### Concepts I don't understand:
- route tables, what are they for?
- difference between virtual private gateway and a customer gateway.
- What is a network acl?
- Networking in AWS is a huge hole in my knowledge honestly... Gotta complete the entire video course on this section and really go deep.

**Question**: As part of the yearly AWS data cleanup, you need to delete all unused S3 buckets and their contents. The `tutorialsdojo` bucket, which contains several educational video files, has both the Versioning and MFA Delete features enabled. One of your Systems Engineers who has an Administrator account tried to delete an S3 bucket using the `aws s3 rb s3://tutorialsdojo` command. However, the operation fails even after repeated attempts.
**Answer**: You can delete a bucket that contains objects using the AWS CLI only if the bucket does not have versioning enabled. If your bucket does not have versioning enabled, you can use the `rb` (remove bucket) AWS CLI command with `--force` parameter to remove a non-empty bucket. An IAM Administrator account can suspend Versioning on an S3 bucket but only the bucket owner can enable/suspend the MFA-Delete on the objects. You can configure lifecycle on your bucket to expire objects and request that Amazon S3 delete expired objects. You can add lifecycle configuration rules to expire all or a subset of objects with a specific key name prefix. For example, to remove all objects in a bucket, you can set a lifecycle rule to expire objects one day after creation. If your bucket has versioning enabled, you can also configure the rule to expire non-current objects.
After your objects expire, Amazon S3 deletes the expired objects. If you just want to empty the bucket and not delete it, make sure you remove the lifecycle configuration rule you added to empty the bucket so that any new objects you create in the bucket will remain in the bucket.

---
**Question**: A leading energy company is trying to establish a static VPN connection between an on-premises network and their VPC in AWS. As their SysOps Administrator, you created the required virtual private gateway, customer gateway and the VPN connection, including the router configuration on the customer side. Although the VPN connection status seems okay in the console, the connection is not entirely working when you connect to an EC2 instance in their VPC from one of the on-premises virtual machines.
**Answer**: To enable instances in your VPC to reach your customer gateway, you must configure your route table to include the routes used by your VPN connection and point them to your virtual private gateway. You can enable route propagation for your route table to automatically propagate those routes to the table for you.
For static routing, the static IP prefixes that you specify for your VPN configuration are propagated to the route table when the status of the VPN connection is UP. Similarly, for dynamic routing, the BGP-advertised routes from your customer gateway are propagated to the route table when the status of the VPN connection is UP. 

---
**Question**: A digital advertising company is planning to migrate its web-based data analytics application from its on-premises data center to AWS. You designed the architecture to use an Application Load Balancer and an Auto Scaling group of On-Demand EC2 Instances which are deployed on a private subnet. The instances will be fetching data analytics from various API services over the Internet every 5 minutes. For security reasons, the EC2 instances should not allow any connections initiated from the Internet. What is the most scalable and highly available solution which should be implemented?
**Answer**: You can use a network address translation (NAT) gateway to enable instances in a private subnet to connect to the Internet or other AWS services, but prevent the Internet from initiating a connection with those instances.
To create a NAT gateway:
1. You must specify the public subnet in which the NAT gateway should reside.
2. You must also specify an Elastic IP address to associate with the NAT gateway when you create it.
After you've created a NAT gateway, you must update the route table associated with one or more of your private subnets to point Internet-bound traffic to the NAT gateway. This enables instances in your private subnets to communicate with the internet.

---
**Question**: A document management system of a legal firm is hosted in AWS Cloud with an S3 bucket as the primary storage service. To comply with the security requirements, you are instructed to ensure that the confidential documents and files stored in AWS are secured.   
Which features can be used to restrict access to data in S3?
**Answer**: By default, all Amazon S3 resources - buckets, objects, and related subresources (for example, lifecycle configuration and website configuration) are private: only the resource owner, an AWS account that created it, can access the resource. The resource owner can optionally grant access permissions to others by writing an access policy.
Amazon S3 offers access policy options broadly categorized as resource-based policies and user policies. Access policies you attach to your resources (buckets and objects) are referred to as resource-based policies. you can also attach access policies to users in your account. These are called user policies. You may choose to use resource-based policies, user policies, or some combination of these to manage permissions to your Amazon S3 resources.
Hence, configuring the S3 bucket policy to only allow access to authorized personnel and configuring the S3 ACL on the bucket of each individual object are both correct answers.

---
**Question**: A company has a newly-hired DevOps Engineer that will assist the IT Manager in developing a fault-tolerant and highly available architecture, which is comprised of an Elastic Load Balancer and an Auto Scaling group of EC2 instances deployed on multiple AZ's. This will be used by a forex trading application that requires WebSockets, host-based and path-based routing, and support for containerized applications.

Which of the following is the most suitable type of Elastic Load Balancer that the DevOps Engineer should recommend to the IT Manager?

**Answer**: Application Load Balancers support WebSockets, path-based routing, host-based routing, and support for containerized applications. Network Load Balancer are incorrect because it doesn't support path-based and host-based routing.


---
**Question**: An organization hosts an application across multiple Amazon EC2 instances backed by an Amazon Elastic File System (Amazon EFS) file system. While monitoring the instances, the SysOps administrator noticed that the file system's `PercentIOLimit` metric consistently hit 100% for 20 minutes or longer. This issue resulted in the poor performance of the application that reads and writes data into the file system. The SysOps admin needs to ensure high throughput and IOPS while accessing the file system.

What step should the SysOps administrator perform to resolve the high `PercentIOLimit` metric on the file system?
**Answer**:
`PercentIOLimit` - Shows how close a file system is to reaching the I/O limit of the General Purpose performance mode. If this metric is at 100 percent more often than not, consider moving your application to a file system using the Max I/O performance mode.
If the `PercentIOLimit` percentage returned was at or near 100 percent for a significant amount of time during the test, your application should use the Max I/O performance mode. Otherwise, it should use the default General Purpose mode.
To move to a different performance mode, migrate the data to a different file system that was created in the other performance mode. You can use [[datasync]] to transfer files between 2 EFS file systems.

---
**Question**: An IT solutions company offers a service that allows users to upload and download files when needed. The files are retrievable for one year and are stored in Amazon S3 Standard. The SysOps administrator noticed that users frequently access the files stored on the bucket for the first 30 days, and from then on, the files are rarely accessed.

The SysOps administrator needs to implement a cost-effective S3 Lifecycle policy that maintains the object availability for users.

Which action should the SysOps administrator perform to achieve the requirements?

**Answer**: Configure all buckets to transition objects to S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days. You may think the answer would be to configure an S3 Lifecycle policy that moves objects to S3 One Zone-Infrequent Access (S3 One Zone-IA) class after 30 days, but this is incorrect because moving an object S3 One Zone-Infrequent will not maintain object availability for users. Amazon S3 Standard replicates data across a minimum of three AZs to protect against the loss of one entire AZ while the Amazon S3 One Zone-IA storage class replicates data within a single AZ only.

---
**Question**: A financial company is launching an online web portal that will be hosted in an Auto Scaling group of Amazon EC2 instances across multiple Availability Zones behind an Application Load Balancer (ALB). To allow HTTP and HTTPS traffic, the SysOps Administrator configured the Network ACL and the Security Group of both the ALB and EC2 instances to allow inbound traffic on ports 80 and 443. The EC2 cluster also connects to a third-party API that provides additional information on the site. However, the online portal is still unreachable over the public internet after the deployment.

How can the Administrator fix this issue?

**Answer**: Allow ephemeral ports in the Network ACL by adding a new rule to allow outbound traffic on port 1024-65535.

To enable the connection to a service running on an instance, the associated network ACL must allow both inbound traffic on the port that the service is listening on as well as allow outbound traffic from ephemeral ports. When a client connects to a service, a random port from the ephemeral port range (1024-65535) becomes the client's source port.
The designated ephemeral port then becomes the destination port for return traffic from the service, so outbound traffic from the ephemeral port must be allowed in the network ACL. By default, network ACLs allow all inbound and outbound traffic. If your network ACL is more restrictive then you need to explicitly allow traffic from the ephemeral port range.

---
**Question**: A live chat application is hosted in AWS which can be embedded as a widget in any website. It uses WebSockets to provide full-duplex communication between the users. The application is hosted on an Auto Scaling group of On-Demand EC2 instances across multiple Availability Zones with an Application Load Balancer in front to balance the incoming traffic. As part of the security audit of the company, there is a requirement that the client's IP address, latencies, request paths, and server responses are properly logged.

How can you meet the given requirement in this scenario?

**Answer**: Do the following
- Set up a standard S3 bucket where the load balancer will store the logs.
- Enable access logging for the Application Load Balancer.

Elastic Load Balancing provides access logs that capture detailed information about requests sent to your load balancer. Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses. You can use these access logs to analyze traffic patterns and troubleshoot issues.

Access logging is an optional feature of Elastic Load Balancing that is disabled by default. After you enable access logging for your application load balancer, Elastic Load Balancing captures the logs and stores them in the Amazon S3 bucket that you specify as compressed files. You can disable access logging at any time.

When you enable access logging, you must set up a standard S3 bucket where the load balancer will store the logs. The bucket must be located in the same region as the load balancer.

---
**Question**: A leading tech consultancy firm has an AWS Virtual Private Cloud (VPC) with one public subnet and a new blockchain application that is deployed to an m3.large EC2 instance. After a month, your manager instructed you to ensure that the application can support IPv6 address.

Which of the following should you do to satisfy the requirement?

**Answer**:
1. Associate an IPv6 CIDR Block with the VPC and Subnets - Associate an Amazon-provided IPv6 CIDR block with your VPC and with your subnets.
2. Update the Route Tables - Update your route tables to route your IPv6 traffic. For a public subnet, create a route that routes all IPv6 traffic from the subnet to the Internet gateway. For a private subnet, create a route that routes all Internet-bound IPv6 traffic from the subnet to an egress-only Internet gateway.
3. Update the Security Group Rules - Update your security group rules to includes rules for IPv6 addresses. This enables IPv6 traffic to flow to and from your instances. If you've created custom network ACL rules to control the flow of traffic to and from your subnet, you must include rules for IPv6 traffic.
4. Change the instance type to m4.large - If your instance type does not support IPv6, change the instance type. If your instance type does not support IPv6, you must resize the instance to a supported instance type. In the example, the instance is an m3.large instance type, which does not support IPv6. you must resize the instance to a supported instance type, for example, m4.large.
5. Assign IPv6 Addresses to the EC2 Instance - Assign IPv6 addresses to your instances from the IPv6 address range of your subnet.
6. (Optional) Configure IPv6 on your Instances - If your instances was launched from an AMI that is not configured to use DHCPv6, you must manually configure your instance to recognize an IPv6 address assigned to the instance.

Take note that the EC2 instance is an m3.large instance type, which does not support IPv6. you must resize the instance to a supported instance type, for example, m4.large. Remember that configuring an IPv6 is just an optional step.
If you have an existing VPC that supports IPv4 only, and resources in your subnet that are configured to use IPv4 only, you can enable IPv6 support for you VPC and resources. Your VPC can operate in dual-stack mode - your resources can communicate over IPv4, or IPv6, or both. IPv4 and IPv6 communication are independent of each other. You cannot disable IPv4 support for your VPC and subnets; this is the default IP addressing system for Amazon VPC and Amazon EC2.

---
%%%%

---

**Question**: A SysOps Administrator is managing a web application hosted in an Amazon EC2 instance. The security groups and network ACLs are configured to allow HTTP and HTTPS traffic in your instance. A manager has received a report that a customer cannot access the application. The Administrator is instructed to investigate if the traffic is reaching the instance. What is the best way to satisfy this requirement?

**Answer**: Use Amazon VPC Flow Logs.

VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data can be published to AmazonCloudWatch Logs and Amazon S3. After you've created a flow log, you can retrieve and view it's data in the chosen destination.

Flow logs can help you with a number of tasks, such as:
- Diagnosing overly restrictive security group rules
- Monitoring the traffic that is reaching your instance
- Determining the direction of the traffic to and from the network interfaces

To ensure that the customer cannot access the application, you can use VPC flow logs. If you create a flow log for a subnet or VPC, each network interface in that subnet or VPC is monitored. Flow log data is collected outside of your network traffic path, and therefore does not affect network throughput or latency.

---

**Question**: An online stock trading application is extensively using an S3 bucket to store client data. To comply with the financial regulatory requirements, you need to generate a report on the replication and encryption status of all of the objects stored in your bucket. The report should show which type of server-side encryption is being used by each object.   

As the Systems Administrator of the company, how can you meet the above requirement with the least amount of effort?

**Answer**: Use S3 Inventory to generate the required report.

Amazon S3 inventory is one of the tools Amazon S3 provides to help manage your storage. You can use it to audit and report on the replication and encryption status of your objects for business, compliance, and regulatory needs. You can also simplify and speed up business workflows and big data jobs using Amazon S3 inventory, which provides a scheduled alternative to the Amazon S3 synchronous List API operation.

Do not use S3 Analytics, because S3 Analytics is primarily used to analyze storage access patterns to help you decide when to transition the right data to the right storage class. It does not provide a report containing the replication and encryption status of your objects.

Do not use S3 Select, because S3 Select is only used to retrieve specific data from the contents of an object using simple SQL expressions without having to retrieve the entire object. It does not generate a detailed report, unlike S3 Inventory.

---

**Question**: A financial start-up has recently adopted a hybrid cloud infrastructure with AWS Cloud. They are planning to migrate their online payments system that supports an IPv6 address and uses an Oracle database in a RAC configuration. As the AWS Consultant, you have to make sure that the application can initiate outgoing traffic to the Internet but blocks any incoming connection from the Internet.

Which of the following options would you do to properly migrate the application to AWS?

**Answer**: Migrate the Oracle database to an EC2 instance. Launch the application on a separate EC2 instance and then set up an egress-only Internet gateway.

An egress-only Internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows outbound communication over IPv6 from instances in your VPC to the Internet, and prevents the Internet from initiating an IPv6 connection with your instances.

An instance in your public subnet can connect to the Internet through the Internet gateway if it has a public IPv4 address or an IPv6 address. Similarly, resources on the Internet can initiate a connection to your instance using its public IPv4 address or its IPv6 address; for example, when you connect to your instance using your local computer.

IPv6 addresses are globally unique, and are therefore public by default. If you want your instance to be able to access the Internet but want to prevent resources on the Internet from initiating communication with your instance, you can use an egress-only Internet gateway. To do this, create an egress-only Internet gateway in your VPC, and then add a route to your route table that points all IPv6 traffic (::/0) or a specific range of IPv6 address to the egress-only Internet gateway. IPv6 traffic in the subnet that's associated with the route table is routed to the egress-only Internet gateway.

Remember that a NAT device in your private subnet does not support IPv6 traffic. As an alternative, create an egress-only Internet gateway for your private subnet to enable outbound communication to the internet over IPv6 and prevent inbound communication. An egress-only Internet gateway supports IPv6 traffic only.

Take note that the application that will be migrated is using an Oracle database on a RAC configuration which is not supported by RDS.

---

**Question**: A company has several applications and workloads running on AWS that are managed by various teams. The SysOps Administrator has been instructed to configure alerts to notify the teams in the event that the resource utilization exceeded the defined threshold.

Which of the following is the MOST suitable AWS service that the Administrator should use?

**Answer**: AWS Budgets.

---

**Question**: A leading national bank migrated its on-premises infrastructure to AWS. The SysOps Administrator noticed that the cache hit ratio of the CloudFront web distribution is less than 15%.

**Answer**:
- In the Cache Behavior settings of your distribution, configure to forward only the query string parameters for which your origin will return unique object.
- Configure your origin to add a `Cache-Control max-age` directive to your object, and specify the longest practical value for `max-age` to increase your TTL.

---

**Question**: A microservice application is being hosted in the ap-southeast-1 and ap-northeast-1 regions. The ap-southeast-1 region accounts for 80% of traffic, with the rest from ap-northeast-1. As part of the company's business continuity plan, all traffic must be rerouted to the other region if one of the regions' servers fails.

Which solution can comply with the requirement?

**Answer**: Set up an 80/20 weighted routing policy in the network load balancer and enable health checks.

Do not set up a failover routing policy in AWS Route 53. This routing policy does not let you control how much traffic is routed across your resources.

---

**Question**: A leading media company plans to launch a data analytics application. The SysOps Administrator designed an architecture to use On-Demand EC2 instances in an Auto Scaling group that read messages from an SQS queue. A month after, the new application has been deployed to production but the Operations team noticed that when the incoming message traffic increases, the EC2 instances fall behind and it takes too long to process the messages.

How can the SysOps Administrator configure the current cloud architecture to reduce the latency during traffic spikes?

**Answer**: Configure the Auto Scaling group to scale out based on the number of messages in the SQS queue.
