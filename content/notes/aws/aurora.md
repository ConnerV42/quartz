---
title: "Aurora"
tags:
- aws
- database
- relational-databases
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [Relational Database Service](/notes/aws/rds.md)

## **Aurora** 

### Key Differences Compared to RDS
- Aurora is hosted within RDS, but it has a different enough architecture that it deserves a dedicated page.
- Uses a cluster, made up of a single primary instances, that has 0 or more replicas.
	- At first, this appears similar to normal RDS, however, the replicas within Aurora can be used for reads during normal operation.
	- In this way, it is *better* than the standby replica inside RDS. The replicas inside of Aurora can actually provide the benefits of both RDS Multi-AZ and RDS Read Replicas.
		- They can be used to improve availability, but they can also be used for read operations during normal operation of a cluster.
	- Replicas are deployed across multiple AZs.
- Aurora doesn't use local storage for the compute instances. It uses a shared cluster storage volumes, which are SSD based, and available to all compute instance within a cluster.
	- This provides faster provisioning, improved availability and improved performance.
	- In a 3 AZ setup, the cluster volume has 6 replicas in total across multiple AZs
		- When data is written to the primary DB instance, aurora synchronously replicates that data across all of these 6 storage nodes spread across the AZs associated with your cluster.
		- This replication happens at the storage level, so no extra resources are consumed on the primary instance or the replicas instances.
		- The chances of disk related failure is greatly minimized with this architecture, and Aurora automatically detects failures in the disk volumes that make up the cluster shared storage, so that when a segments or part of a disk volume fails, Aurora immediately repairs that area of the disk, using the data inside the other storage nodes to bring it back into an operational state with no corruption.
		- The need to perform point-in-time restores or snapshot restores to recover from disk failure is greatly reduced.
- Aurora allows up to 15 replicas, and any of them can be the fail over target for a fail over operation. This is much more than the single standby instance normal RDS allows. The failover operation will also be much faster, because it doesn't have to make any storage modifications.
- Cluster volume storage is all SSD based, which provides high IOPS and low latency. No magnetic storage option
- Storage is billed based on what is consumed, based on a high water mark
- To reduce the high water mark, you have to migrate to a new cluster to lose that high water mark.
	- NOTE: This is no longer part of Aurora in newer versions
- Aurora clusters, like RDS, use reader endpoints
	- The reader endpoints automatically load balance across read replicas
		- In addition, each read replica has a dedicated endpoint if needed

### General Notes
  - **Amazon Aurora Global Database** is specifically designed for globally distributed applications, allowing a single Amazon Aurora database to span multiple AWS regions. It replicates your data with no impact on database performance, enables fast local reads with low latency in each region, and provides disaster recovery from region-wide outages. Aurora Global Database supports storage-based replication that has a latency of less than 1 second. If there is an unplanned outage, one of the secondary regions you assigned can be promoted to read and write capabilities in less than 1 minute. This feature is called **Cross-Region Disaster Recovery**.
  - Aurora typically involves a cluster of DB instances instead of a single instance. Each connection is handled by a specific DB instance. When you connect to an Aurora cluster, the hostname and port that you specify point to an intermediate handler called an endpoint. Aurora uses the endpoint mechanism to abstract these connections. Thus, you don't have to hardcode all the hostnames or write your own logic for load-balancing and rerouting connections when some DB instances aren't available.
  - **Aurora Serverless** is an on-demand, auto-scaling configuration for Amazon Aurora. An Aurora Serverless DB cluster is a DB cluster that automatically starts up, shuts down, and scales up or down its compute capacity based on your application's needs. Aurora Serverless provides a relatively simple, cost-effective option for infrequent, intermittent, sporadic, or unpredictable workloads. It can provide this because it automatically starts up, scales compute capacity to match your application's usage and shuts down when it's not in use
  - **Load Balancing Across Aurora DB Instances**
    - Amazon Aurora typically involves a cluster of DB instances instead of a single instance. Each connection is handled by a specific DB instance. When you connect to an Aurora cluster, the hostname and port that you specify point to an intermediate handler called an endpoint. Aurora uses the endpoint mechanism to abstract these connections. Thus, you don’t have to hardcode all the hostnames or write your own logic for load-balancing and rerouting connections when some DB instances aren't available.
    - For certain Aurora tasks, different instances or groups of instances perform different roles. For example, the primary instance handles all data definition language (DDL) and data manipulation language (DML) statements. Up to 15 Aurora Replicas handle read-only query traffic.
    - Using endpoints, you can map each connection to the appropriate instance or group of instances based on your use case. For example, to perform DDL statements you can connect to whichever instance is the primary instance. To perform queries, you can connect to the reader endpoint, with Aurora automatically performing load-balancing among all the Aurora Replicas. For clusters with DB instances of different capacities or configurations, you can connect to custom endpoints associated with different subsets of DB instances. For diagnosis or tuning, you can connect to a specific instance endpoint to examine details about a specific DB instance.
  - **Failover** is automatically handled by Amazon Aurora so that your applications can resume database operations as quickly as possible without manual administrative intervention.
    - If you have an Amazon Aurora Replica in the same or a different Availability Zone, when failing over, Amazon Aurora flips the canonical name record (CNAME) for your DB Instance to point at the healthy replica, which in turn is promoted to become the new primary. Start-to-finish, failover typically completes within 30 seconds.
    - If you are running Aurora Serverless and the DB instance or AZ become unavailable, Aurora will automatically recreate the DB instance in a different AZ.
    - If you do not have an Amazon Aurora Replica (i.e. single instance) and are not running Aurora Serverless, Aurora will attempt to create a new DB Instance in the same Availability Zone as the original instance. This replacement of the original instance is done on a best-effort basis and may not succeed, for example, if there is an issue that is broadly affecting the Availability Zone.
  - Handles highly transactional (OLTP) workloads

### Cost Considerations
- Aurora has no free-tier option
- Aurora doesn't support micro instances
- Beyond RDS single-AZ (micro) Aurora offers better value
- Compute - hourly charge, billed per second, with a 10 minute minimum
- Storage - GB-Month consumed, IO cost per request
- 100% DB size in backups are included

### Aurora Restore, Clone & Backtrack
- Backups in Aurora work in the same way as RDS
- Restores create a new cluster
- Backtrack can be used which allow in-place rewinds to a previous point in time
	- Needs to be enabled on a per-cluster basis
	- Exclusive to Aurora at the time that this note was taken
- Fast clones make a new database much faster than copying all the data - copy-on-write
  