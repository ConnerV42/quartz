---
title: "EFS - Elastic File System"
tags:
- aws
- efs
- shared-storage
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [EC2](ec2.md)

## **EFS - Elastic File System**
- Moves EC2 to be closer to being stateless
- Implementation of NFSv4
- EFS Filesystems can be mounted in Linux only
	- Uses POSIX permissions, a standard understood by all linux systems
- Shared between many EC2 Instances
- Private service, via mount targets inside a VPC
	- Mount targets must be present in each AZ that a VPC uses to provide high availability
	- Mount targets are what the EC2 instances use to connect to the EFS
- Can be accessed from on-premises via VPN or DX
- General Purpose and Max I/O performance mode
	- Max I/O is good for highly parallel services
- Bursting and Provisioned Throughput Modes
- Storage Classes
	- Standard (default)
	- Infrequent Access (lower cost)
	- Lifecycle Policies can be used with classes
- Amazon EFS is a fully-managed service that makes it easy to set up and scale file storage in the Amazon Cloud. With a few clicks in the AWS Management Console, you can create file systems that are accessible to Amazon EC2 instances via a file system interface (using standard operating system file I/O APIs) and supports full file system access semantics (such as strong consistency and file locking).
- Amazon EFS file systems can automatically scale from gigabytes to petabytes of data without needing to provision storage. Tens, hundreds, or even thousands of Amazon EC2 instances can access an Amazon EFS file system at the same time, and Amazon EFS provides consistent performance to each Amazon EC2 instance. Amazon EFS is designed to be highly durable and highly available.
- Provides simple, scalable, elastic file storage for use with AWS Cloud services and on-premises resources. When mounted on EC2 instances, an EFS file system provides a standard file interface and file system access semantics, allowing you to seamlessly integrate EFS with your existing applications and tools. Multiple EC2 instances can access an EFS file system at the same time, allowing EFS to provide a common data source for workloads and applications running on more than one EC2 instance.
- When to choose EFS over EBS?
	- When the scenario requires concurrently-accessible storage (think multiple EC2 instances accessing the same storage system). In this scenario, EFS is better than EBS. But EBS provides lower latency. But EBS volumes can only be attached to one instance at a time. EFS can be attached to multiple.
- Note of Caution: You cannot, say, modify an existing EFS file system configuration and activate Max I/O performance mode. This is because you cannot change the performance mode configuration of an EFS file system right away. You need to migrate the data to another file system configured with your desired performance mode. [DataSync](/notes/aws/datasync.md) can facilitate this.

### EFS Encryption
- An existing EFS cannot be encrypted. You must create a new EFS with encryption enabled, and copy over the data from the existing EFS.