---
title: "EC2"
tags:
- aws
- aws-compute
- ec2
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [EC2 Purchase Options](/notes/aws/ec2-purchase-options.md)
- [EC2 Placement Groups](/notes/aws/ec2-placement-groups.md)

## **EC2 - Elastic Compute Cloud**

### Instance Types
- **Instance Types**
	- C: Compute Optimized Instances
		- Cost-effective high performance at a low price per compute ratio
		- Note that `c1` type instance do not support IPv6
	- D: Storage Optimized Instances
		- High disk throughput
	- G: Accelerated Computing Instances
		- Graphics-intensive GPU instances
	- H: Storage Optimized Instances
		- HDD-based local storage for high disk throughput
	- I: Storage Optimized Instances
		- High storage instances, low latency, high random I/O performance, high sequential read throughput, and high IOPS
	- M: General Purpose Instances
		- Fixed performance
		- Note that `m3` type instance do not support IPv6
	- P: Accelerated Computing Instances
		- General purpose GPU instances
	- F: Accelerated Computing Instances
		-  Reconfigurable FPGA instances
	- R: Memory Optimized Instances
		- Memory-intensive applications
	- T: General Purpose Instances
		- Burstable performance instances
	- X: Memory Optimized Instances
		- Large-scale, enterprise-class, in-memory applications, and high-performance databases

### EC2 Storage
- Some EC2 instance types come with a form of directly attached, block-storage known as the instance store. The instance store is ideal for temporary storage, because the data stored in an instance store is not persistent through instance stops, terminations, or hardware failures. For data that you want to retain for longer, or if you want to encrypt the data, use EBS volumes instead. EBS volumes preserve their data through instance stops and terminations, can easily be backed up with EBS snapshots, can be removed from one instance and reattached to another, and support full-volume encryption.

### EC2 Instance Lifecycle States
- Below are the valid EC2 lifecycle instance states:
	- `pending`: The instance is preparing to enter the running state. An instance enters the pending state when it launches for the first time, or when it is restarted after being in the stopped state.
	- `running`: The instance is running and ready for use.
	- `stopping`: The instance is preparing to be stopped. Take note that you will not be billed if it is preparing to stop however, you will still be billed if it is just preparing to hibernate.
	- `stopped`: The instance is shut down and cannot be used. The instance can be restarted at any time.
	- `shutting-down`: The instance is preparing to be terminated.
	- `terminated`: The instance has been permanently deleted and cannot be restarted. Take note that Reserved Instances that applied to terminated instances are still billed until the end of their term according to their payment option.



### Connecting to EC2 Instances
- If connecting to an EC2 instance running windows, you'd use the Remote Desktop Protocol, or RDP. This runs on port 3389.
- If connecting to an EC2 instance running linux, you'd use SSH. this runs on port 22.

### General EC2 Notes
- If you have the requirement that an EC2 instance can only be accessed from this IP: 110.238.98.71 via ssh, your Security Group Inbound Rule would have the following configuration: Protocol - TCP, Port Range - 22, Source 110.238.98.81/32 (NOTE: The /32 denotes one IP address, if you were to use /0, it would refer to the entire network. Take note that the SSH protocol uses TCP and port 22)
- Instance metadata is the data about your instance that you can use to configure or manage the running instance. You can get the instance ID, public keys, public IP address and many other information from the instance metadata by firing a URL command in your instance to this URL: http://169.254.169.254/latest/meta-data
- By default, a new EC2 instance uses an IPv4 addressing protocol.
- By default, Amazon EC2 sends metric data to CloudWatch in 5-minute periods. To send metric data for your instance to CloudWatch in 1-minute periods, you can enable detailed monitoring on the instance.
- A FinTech startup deployed an application on an Amazon EC2 instance with attached Instance Store volumes and an Elastic IP address. The server is only accessed from 8 AM to 6 PM and can be stopped from 6 PM to 8 AM for cost efficiency using Lambda with the script that automates this based on tags. What occurs when the EC2 instance is stopped?
	- All data on the attached instance store devices will be lost.
	- The underlying host for the instance is possibly changed
	- **What doesn't change?**
		- The ENI (Elastic Network Interface) is not detached
		- If you stopped an EBS-backed EC2 instance, the volume is preserved but the data in any attached instance store volume will be erased. Keep in mind that an EC2 instance has an underlying physical host computer. If the instance is stopped, AWS usually moves the instance to a new host computer. Your instance may stay on the same host computer if there are no problems with the host computer. In addition, its Elastic IP address is disassociated from the instance if it is an EC2-Classic instance. Otherwise, if it is an EC2-VPC instance, the Elastic IP address remains associated.
- **Elastic Fabric Adapter** is a network device that you can attach to your EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications. EFA enables you to achieve the application performance of an on-premises HPC cluster, with the scalability, flexibility, and elasticity provided by AWS. (Not supported on Windows instances)


- The **Reserved Instance Marketplace** is a platform that supports the sale of third-party and AWS customers' unused Standard Reserved Instance, which vary in terms of lengths and pricing options. For example, you may want to sell Reserved Instances after moving instances to a new AWS region, changing to a new instance type, ending projects before the term expiration, when your business needs change, or if you have unneeded capacity.
- You can use *placement groups* to influence the placement of a group of *interdependent* instances to meet the needs of your workload. Depending on the type of workload, you can create a placement group using one of the following placement strategies:
	- Cluster - packs instances close together inside an AZ. This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typical of HPC applications.
	- Partition - spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.
	- Spread - strictly places a small group of instances across distinct underlying hardware to reduce correlated failures.
	- When working with instance store volumes and temporary data, RAID-0 is more efficient than RAID-1. RAID-0 configuration enables you to improve your storage volumes' performance by distributing the I/O across the volumes in a stripe. Therefore, if you add a storage volume, you get the straight addition of throughput and IOPS. This configuration can be implemented on both EBS or instance store volumes. Since the main requirement in the scenario is storage performance, you need to use an instance store volume. It uses NVMe or SATA-based SSD to deliver high random I/O performance. This type of storage is a good option when you need storage with very low latency, and you don't need the data to persist when the instance terminates. RAID-1 is used for data mirroring and fault tolerance.
- A *bastion host* is a special purpose computer on a network specifically designed and configured to withstand attacks. If you have a bastion host in AWS, it is basically just an EC2 instance. It should be in a public subnet with either a public or Elastic IP address with sufficient RDP or SSH access defined in the security group. Users log on to the bastion host via SSH or RDP and then use that session to manage other hosts in the private subnets.

### Amazon Machine Images (AMI)
- The Images of EC2. AMI's can be used to launch EC2 instances, they're used by the console UI itself, when you launch an EC2 instance. These are AWS or community provided, but you can create custom AMIs.
- Contains the Boot Volume of an EC2 instance
	- The C volume in Windows
	- The root volume in linux
- Contains a Block Device Mapping, a configuration which specifies which volume is the boot volume and which volume(s) are the data volumes
- Marketplace AMIs can include commercial software, which can cost extra, due to licenses for commercial softwaer
- Each region has a unique ID in the following format: `ami-0a887e401f7654935`
- Permissions Options (Who can use an AMI?)
	- Public
	- Your Account
	- Specific Accounts
- You can also create an AMI from an EC2 instance that you want to template
- AMI = One Region, only works in that one region, but can be used in AZs all over that region
- AMI Baking = creating an AMI from a configured instance + application
- An AMI cannot be edited, you must launch an instance, update configuration and make a new AMI
- Can be copied between regions (includes its snapshots)
- Remember permissions (default = your account)
- AMI Billing: AMIs contain EBS Snapshots, so you are billed for this storage capacity

#### Lifecycle of an AMI
1. Launch: Use an AMI to launch an EC2 instance, and/or add an EBS volume
2. Configure: Take instance and attached EBS volumes, and apply customizations, like an OS that is heavily configured with an application
3. Create Image: Take previously configured instance to produce an AMI
4. Launch: The new instance will have new EBS volumes that are perfect copies of the original EBS volume snapshots from S3. They will have exactly the same data.

## Scenarios
A DevOps Engineer reported a problem accessing his EC2 instance with a private IP address of 172.31.8.11 from his corporate laptop. The EC2 instance is hosting a web application which works well but he is still experiencing an issue establishing a connection to manage the instance. As the SysOps Administrator, which of the following options is the most suitable solution in this scenario based on the VPC flow log entries below?
```
2 123456789010 eni-abc123de 110.217.100.70 172.31.8.11 49761 3389 6 20 4249 1418530010 1418530070 REJECT OK
```
Answer: Based on the VPC flow log record provided, the RDP traffic (destination port 3389) to network interface `eni-abc123de` in the AWS account `123456789010` was rejected. The RDP connection request came from the DevOps engineer's laptop (with an IP address of `110.217.100.70`) and it is trying to access the EC2 instance with a private IP address of `172.31.8.11`.

Although the scenario did not explicitly say what type of remote connection protocol the DevOps engineer used, it is quite clear in the VPC flow logs that the user is using Remove Desktop Protocol (RDP). The root cause of this issue is because the security group and the Network ACL of the EC2 instance do not allow RDP traffic. To solve this issue, you would simply have to configure the security group of the EC2 instance to allow incoming RDP traffic including the inbound and outbound rules in the Network ACL.

A VPC flow log record is a space-separated string that has the following format:
```
<version> <account-id> <interface-id> <srcaddr> <dstaddr> <srcport> <dstport> <protocol> <packets> <bytes> <start> <end> <action> <log-status>
```

#aws #aws-compute 