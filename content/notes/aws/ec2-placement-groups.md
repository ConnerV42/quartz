---
title: "EC2 Placement Groups"
tags:
- aws
- ec2
disableToc: false
---

### Related Notes
- [EC2](/notes/aws/ec2.md)

## EC2 Placement Group
- Allows you to influence physical placement of EC2 instances within AWS

### Cluster Placement Groups (Performance)
- Generally, instances in this placement group will use the same rack, and sometime even the same host.
- Maximizes performance, all members have direct connections to each other.
- 10 Gbps per stream compared to the 5 Gbps which is achievable normally
- Lowest latency and max PPS possible in AWS
- Offers little to no resilience
- Can't span AZs - ONE AZ ONLY - locked when launching first instance
- Can span VPC peers - but impacts performance
- Requires a supported instance type
- Use the same type of instance (not mandatory)
- Launch at the same time (not mandatory ... very recommended)
- Use case: Performance, fast speeds, low latency

### Spread Placement Groups (Resilience)
- Keep instances separated
- Ensure maximum amount of availability and resiliency for an application
- Located on isolated, distinct racks per AZ
	- Max of 7 instance per AZ, due to distinct rack placement
- Provides infrastructure isolation
- Each instance runs from a different rack
- Each rack has its own network and power source
- Not supported for Dedicated Instances or Hosts
- Use Case: Small number of critical instances that need to be kept separated from each other

### Partition Placement Groups (Topology Awareness)
- Groups of instances, spread apart
- Similar to spread placement groups, but for situations where you have more than 7 instances per AZ. There is no instance limit per AZ.
- Divided into partitions, max 7 partitions per AZ
- Each partition has its own racks, no sharing between partitions
- Designed for huge scale parallel processing systems
- You can control which instances are placed in which partitions - Topology Aware
- Instances can be placed in a specific partition or auto placed
- HDFS, HBase, and Cassandra