### Related Notes
- [[aws-wiki]]
- [[aurora]]

### Useful Links
- [RDS Pricing](https://aws.amazon.com/rds/pricing/)
- [RDS Free Tier](https://aws.amazon.com/rds/free/)

### How to connect To RDS with MySQL Client:
- This simplified guide was pulled from this AWS Docs page: [Connecting to a DB instance running the MySQL database engine](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html). If you need your RDS instance's master password, you cannot obtain that information from the console. Use the AWS CLI to obtain such information, guide [here](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/lightsail/get-relational-database-master-user-password.html).
1. Install `mariadb` and `mariadb-client` so that you can connect to the RDS instance:
```
brew install mariadb
```
```
brew install mariadb-client
```
2. Verify install:
```
mysql --version
```
3. Connect to your instance (`-p` prompts for password):
```
mysql -h $WRITER_INSTANCE -P $PORT -u $DB_USER -p
```

### Create User in RDS (MySQL engine)
1. View grants for master user as a reference, carefully determine the permissions that make sense for your user:
```sql
SHOW GRANTS for master_user;
```
2. Create new user and password:
```sql
CREATE USER 'new_user'@'%' IDENTIFIED BY 'password';
```
3. Grant permissions to the new user with the `GRANT` command:
```sql
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, EVENT, TRIGGER ON *.* TO 'new_user'@'%' WITH GRANT OPTION;
```

user: Don't give CREATE USER, INVOKE SAGEMAKER, INVOKE COMPREHEND, SELECT INTO S3, LOAD FROM S3

- [How do I create another master user for my Amazon RDS DB instance that is running MySQL?](https://aws.amazon.com/premiumsupport/knowledge-center/duplicate-master-user-mysql/)

## **RDS - Relational Database Service**

### Storage Options
- Storage is allocated to RDS, much in the same way that it is allocated in EC2. You have 3 options:
  - gp2 (General Purpose SSD, Default) - General Purpose SSD volumes offer cost-effective storage that is ideal for a broad range of workloads. These volumes deliver single-digit millisecond latencies and the ability to burst to 3,000 IOPS for extended periods of time. Baseline performance for these volumes is determined by the volume's size.
	- io1 (Provisioned IOPS) - Provisioned IOPS storage is designed to meet the needs of I/O-intensive workloads, particularly database workloads, that require low I/O latency and consistent I/O throughput.
	- Magnetic (Amazon RDS also supports magnetic storage for backward compatibility, but it is not recommended)

### RDS Backups And Restores
- If Multi-AZ is enabled, backups occur from the standby instance (primary instance is never used)
- If Multi-AZ is disabled, backups occur from the single running instance

#### RPO - Recovery Point Objective
- Time between the last backup and the incident
- Amount of maximum data loss
- Influences technical solution & cost
- Generally lower values cost more (more backups, more often)
- If an incident occurs 8 hours after the last incremental database backup was taken, that's an 8 hour RPO.

#### RTO - Recovery Time Objective
- Time between the DR event and full recovery
- Influenced by process, staff, tech and documentation
- Generally lower values cost more

#### Manual Snapshots
- Ran against an RDS database. The first snapshot is a full copy of the data used on the RDS volume. From then on, each snapshot is incremental.
- There is a brief interruption between the compute resource (think ECS or Lambda) and the storage, when a snapshot is taken.
	- This can impact your application, but only if you're using Single-AZ
	- Incremental backups are usually much quicker, as long as you don't have massive changes in data
	- Manual snapshots live on past the lifetime of the RDS instance
	
#### Automated Backups
- Just snapshots, that occur automatically
- Occurs during a backup window that you define
- Window impacts RPO
- Every 5 minutes, RDS writes transaction logs to S3 every 5 minutes. This allows you to pair with a snapshot, so that you can achieve a very low RPO value. These logs have a retention period from 0 to 35 days.
	- This works by first, restoring the backup, and then "replaying" transaction logs to bring DB to desired point in time

#### RDS Restores
- Creates a new RDS Instance - new address, meaning application code changes
- Snapshots = single point in time, creation time
- Automated = any 5 minute point in time
- RDS Restores aren't fast, especially if the database is large - Think about RTO
- Snapshots are the only way to protect from data corruption, with replication, corrupted data is also replicated

### RDS Read-Replicas
- Read Replicas, unlike the Multi-AZ standby instances, can be used for read operations.
- Kept in sync using asynchronous replication. Written fully to the primary instance first, then propagated to the read replicas, which means there could be a slight lag.
- Read Replicas provide performance improvements
	- 5 dedicated read-replicas per instance
	- Each providing an additional instance of read performance, allows scaling out read capacity
	- Applications code can be configured to perform reads against read replicas
- Cross-region read replicas can be created, AWS handles all networking
	- Provides global performance and availability improvements by deploying cross region read replicas around the world
- Read Replicas offer near zero RPO, if the primary instance fails, the Read Replica can be quickly promoted to be a new primary instance. This can be done in minutes. Works for database failure only - watch for data corruption.

### Enhanced Monitoring
- **Enhanced Monitoring** metrics are useful when you want to see how different processes or threads on a DB instance use the CPU when working with RDS. 

### IAM DB Authentication
- **IAM DB Authentication** is used to authenticate to your DB instance. This works with MySQL and PostgreSQL. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you generate an authentication token, which has a lifetime of 15 minutes.

### Multi-AZ ( #highly-available #high-availability )
- RDS provides high availability and failover support for DB instances using **Multi-AZ** deployments (**Single-AZ** is also supported, if desired).
- Amazon RDS uses several different technologies to provide failover support. Multi-AZ deployments for Oracle, PostgreSQL, MySQL, and MariaDB DB instances use Amazon's failover technology. SQL Server DB instances use SQL Server Database Mirroring (DBM). In a Multi-AZ deployment, Amazon RDS automatically provisions and maintains a synchronous standby replica in a different AZ. The primary DB instance is synchronously replicated across AZs to a standby replica to provide data redundancy, eliminate I/O freezes, and minimize latency spikes during system backups. Running a DB instance with high availability can enhance availability during planned system maintenance, and help protect your databases against DB instance failure and AZ disruption. Amazon RDS detects and automatically recovers from the most common failure scenarios for Multi-AZ deployments so that you can resume database operations as quickly as possible without administrative intervention. The high-availability feature is not a scaling solution for read-only scenarios; you cannot use a standby replicate to serve read traffic. To service read-only traffic, you should use a Read Replica.
- Amazon RDS automatically performs a failover in the event of any of the following:
	1. Loss of availability in the primary Availability Zone.
	2. Loss of network connectivity to primary.
	3. Compute unit failure on primary.
	4. Storage failure on primary.
- In Amazon RDS, failover is automatically handled so that you can resume database operations as quickly as possible without administrative intervention in the event that your primary database instance went down. When failing over, Amazon RDS simply flips the canonical name record (CNAME) for your DB instance to point at the standby, which is in turn promoted to become the new primary.
	- Note that a standby DB instance can't be directly used, unless a failover happens, meaning that it cannot be used to scale read access. A read replica must be used for this purpose.
	- Note that RDS Multi-AZ is highly available, but not fault tolerant. There will be 60-120 seconds of impact during the failover. 
	- Note that RDS Multi-AZ happens in the same region only (other AZs in the VPC)
	- Note that by using RDS Multi-AZ, backups are taken from Standby DB instance, removing any potential performance impact.

### RDS Storage Auto Scaling
- RDS Storage Auto Scaling automatically scales storage capacity in response to growing database workloads, with zero downtime. Under-provisioning could result in application downtime, and over-provisioning could result in underutilized resources and higher costs. With RDS Storage Auto Scaling, you simply set your desired maximum storage limit, and Auto Scaling takes care of the rest. RDS Storage Auto Scaling continuously monitors actual storage consumption, and scales capacity up automatically when actual utilization approaches provisioned storage capacity. Auto Scaling works with new and existing database instances. You can enable Auto Scaling with just a few clicks in the AWS Management Console. There is no additional cost for RDS Storage Auto Scaling. You pay only for the RDS resources needed to run your applications.

### Database Instance Types
- `db.m5` - General Usage Type
- `db.r5` - Memory Optimized Type
- `db.t3` - Burst Instance Type

### Security Considerations
- Access to RDS is controlled via Security Group

### #IAM #database #authentication for MySQL and PostgreSQL
You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with MySQL and PostgreSQL. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token.
Use IAM DB Authentication and create database accounts using the AWS-provided `AWSAuthenticationPlugin` plugin in MySQL.

#aws #relational-database #relational #sql