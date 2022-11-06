---
title: "EBS"
tags:
- aws
- ec2
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **EBS - Elastic Block Store**
- EBS provides block level storage volumes for use with EC2 instances. EBS volumes behave like raw, unformatted block devices. You can mount these volumes as devices on your instances. EBS volumes that are attached to an instance are exposed as storage volumes that persist independently from the life of the EC2 instances. EBS can be used as persistent storage. It is mainly used as the root volume to store the operating system of an EC2 instance.
- All data moving between the volume and the instance are encrypted.
- You can encrypt an EBS volume at rest, by using AWS KMS customer master keys for the encryption of both the boot and data volumes of an EC2 instance.
- When you create an EBS volume in an AZ, it is automatically replicated within that zone to prevent data loss due to a failure of any single hardware component.
- After you create a volume, you can attach it to any EC2 instance in the same AZ.
- **EBS Multi-Attach** enables you to attach a single Provisioned IOPS SSD (io1) volume to multiple Nitro-based instances that are in the same AZ. However, other EBS types are not supported.
- An EBS volume is off-instance storage that can persist independently from the life of an instance. You can specify not to terminate the EBS volume when you terminate the EC2 instance during instance creation by setting the `DeleteOnTermination` attribute of the EBS volumes to `False`.  By default EBS root device volumes are automatically deleted when the EC2 instance terminates.
- An EBS volume can be used while a snapshot is in progress. An in-progress snapshot is not affected by ongoing reads and writes to the volume hence, you can still use the EBS volume normally.
- EBS is an easy-to-use, high-performance block storage solution designed for use with EC2 for both throughput and transaction-intensive workloads at any scale. A broad range of workloads, such as relational and non-relational databases, enterprises applications, containerized applications, big data analytics engines, file systems, and media workflows are widely deployed on Amazon EBS.
- When choosing an EBS type for your database, keep in mind that certain I/O characteristics drive the performance behavior for your EBS volumes. SSD-backed volumes, such as **General Purpose SSD (gp2)** and **Provisioned IOPS SSD (io1)**, deliver consistent performance whether an I/O operation is random or sequential. HDD-backed volumes like **Throughput Optimized HDD (st1)** (frequent access) and **Cold HDD (sc1)** (infrequent access) deliver optimal performance only when I/O operations are large and sequential.
- SSD's are better for small, random I/O operations. SSD's can be used as a bootable volume. Suitable use cases are transactional workloads, critical business applications that require sustained IOPS performance, and large database workloads such as MongoDB, Oracle, Microsoft SQL Server, etc. Cost is high, and dominant performance attribute is IOPS.
- HDD's are better for large, sequential operations. They can *not* be used as a bootable volume. Suitable use cases are large streaming workloads requiring consistent fast throughput at a low price, Big Data, Data warehouses, Log processing, Throughput-oriented storage for large volumes of data that is infrequently accessed. Cost is low, and dominant performance attribute is Throughput in (mib/s).
- EBS volumes support live configuration changes while in production which means that you can modify the volume type, volume size, and IOPS capacity without service interruptions
- EBS encryption uses 256-bit Advanced Encryption Standard algorithms (AES-256)
- EBS Volumes offer 99.999% SLA.
- EBS provides the *lowest* latency between EFS and S3 because it is attached directly to the EC2 instance that uses it. But it can only be attached to a single EC2 instance, unlike EFS, which can be accessed by multiple instances concurrently.
- **IOPS**
	- Provisioned IOPS SSD (io1) volumes are designed to meet the needs of I/O-intensive workloads, particularly database workloads, that are sensitive to storage performance and consistency. Unlike gp2, which uses a bucket and credit model to calculate performance, an io1 volume allows you to specify a consistent IOPS rate when you create the volume, and Amazon EBS delivers within 10 percent of the provisioned IOPS performance 99.9 percent of the time over a given year. (SSD - small, random I/O operations)
	- An io1 volume can range in size from 4 GiB to 16 TiB. You can provision from 100 IOPS up to 64,000 IOPS per volume on Nitro system instance families and up to 32,000 on other instance families. The maximum ratio of provisioned IOPS to requested volume size (in GiB) is 50:1.
	- For example, a 100 GiB volume can be provisioned with up to 5,000 IOPS. On a supported instance type, any volume 1,280 GiB in size or greater allows provisioning up to the 64,000 IOPS maximum (50 × 1,280 GiB = 64,000).
	- An io1 volume provisioned with up to 32,000 IOPS supports a maximum I/O size of 256 KiB and yields as much as 500 MiB/s of throughput. With the I/O size at the maximum, peak throughput is reached at 2,000 IOPS. A volume provisioned with more than 32,000 IOPS (up to the cap of 64,000 IOPS) supports a maximum I/O size of 16 KiB and yields as much as 1,000 MiB/s of throughput.
	- The volume queue length is the number of pending I/O requests for a device. Latency is the true end-to-end client time of an I/O operation, in other words, the time elapsed between sending an I/O to EBS and receiving an acknowledgment from EBS that the I/O read or write is complete. Queue length must be correctly calibrated with I/O size and latency to avoid creating bottlenecks either on the guest operating system or on the network link to EBS.
	- Optimal queue length varies for each workload, depending on your particular application’s sensitivity to IOPS and latency. If your workload is not delivering enough I/O requests to fully use the performance available to your EBS volume then your volume might not deliver the IOPS or throughput that you have provisioned.
	- Transaction-intensive applications are sensitive to increased I/O latency and are well-suited for SSD-backed io1 and gp2 volumes. You can maintain high IOPS while keeping latency down by maintaining a low queue length and a high number of IOPS available to the volume. Consistently driving more IOPS to a volume than it has available can cause increased I/O latency.
	- Throughput-intensive applications are less sensitive to increased I/O latency, and are well-suited for HDD-backed st1 and sc1 volumes. You can maintain high throughput to HDD-backed volumes by maintaining a high queue length when performing large, sequential I/O. (HDD - large, sequential I/O operations)
	- Therefore, for instance, a 10 GiB volume can be provisioned with up to 500 IOPS. Any volume 640 GiB in size or greater allows provisioning up to a maximum of 32,000 IOPS (50 × 640 GiB = 32,000).
- EBS volumes attached to stopped EC2 instances still incur charges.

### RAID Configurations
With Amazon EBS, you can use any of the standard RAID configurations that you can use with a traditional bare metal server, as long as that particular RAID configuration is supported by the operating system for your instance. This is because all RAID is accomplished at the software level.

For greater I/O performance than you can achieve with a single volume, RAID 0 can stripe multiple volumes together; for on-instance redundancy, RAID 1 can mirror two volumes together which can also offer fault tolerance.

RAID 5 and RAID 6 are not recommended for Amazon EBS because the parity write operations of these RAID modes consume some of the IOPS available to your volumes. Depending on the configuration of your RAID array, these RAID modes provide 20-30% fewer usable IOPS than a RAID 0 configuration. Increased cost is a factor with these RAID modes as well; when using identical volume sizes and speeds, a 2-volume RAID 0 array can outperform a 4-volume RAID 6 array that costs twice as much.