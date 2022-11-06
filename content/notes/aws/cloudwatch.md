---
title: "CloudWatch"
tags:
- aws
- aws-monitoring
- aws-logging
- aws-alarms
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **CloudWatch**
Ingest, store and manage metrics
- Public Service - public space endpoints (can be accessed both on-prem or on AWS)
- Needs public internet access to access the public space endpoint
- AWS Service integration - management place
- Agent integration .. e.g. EC2 - richer metrics
- On-Premises integration via Agent/API (custom metrics)
- Application Integration via API/Agent (custom metrics)
- View data via console UI, CLI, API, dashboards and anomaly detection
- Alarms ... react to metrics, can be used to notify or perform actions
- Instances in a VPC can connect to CloudWatch via an Internet Gateway or Interface Endpoint
- Supports Billing Alarms
- CloudWatch recently was updated so that it can now aggregate data across AWS Regions

### CloudWatch - Data
- Namespace = container for metrics e.g. `AWS/EC2` & `AWS/Lambda` (note: custom namespaces don't start with `AWS`)
- Datapoint, the things that CloudWatch records = Timestamp, value, (optional) unit of measure
- Metrics = time ordered set of data points, like `CPUUtilization`, `NetworkIn, DiskWriteBytes` (EC2)
	- Every metric has a MetricName (`CPUUtilization`) and a Namespace (`AWS/EC2`)
- Dimension .. name/value pair
	- e.g. `CPUUtilization` Name=InstanceID, Value=i=111111111
	- `AutoScalingGroupName`, `ImageId`, `InstanceId`, `InstanceType`
- Resolution .. Standard (60s granularity) .. High (1s - costs more)
	- Retention
		- sub 60s are retained for 3 hours
		- 60s retained for 15 days
		- 300s (5 minutes) retained for 63 days
		- 3600s (1 hour) retained for 455 days
		- As data ages, its aggregated and stored for longer with less resolution (detail matters less as data ages)
- Statistics: aggregation over a period (e.g. `Min`, `Max`, `Sum`, `Average`)
- Percentile - e.g. p95 and p99
- Alarms - watches a metric over a time period
	- ALARM or OK are the two states of an alarm
	- value of a metric vs threshold over time
	- Alarms that trigger to ALARM state can trigger one or more actions, triggered by EventBridge
	- Alarm Resolution

### CloudWatch Logs
- Public Service - usable from AWS or on-premises
- Store, Monitor and access logging data
- AWS Integrations - EC2, VPC Flow Logs, Lambda, CloudTrail, R53 and more
- OS logs on EC2 can be logged using the CloudWatch Agent (this also works for on-premises servers)
- Can generate metrics based on logs - metric filter

### CloudWatch Events and EventBridge
- EventBridge is a superset of CloudWatch Events, and is replacing the service altogether. It can do additional things that CloudWatch Events alone couldn't, such as 3rd party events and custom applications.
- If X happens, or at Y times(s) ... do Z
- EventBridge is ... CloudWatch Events v2, start using EventBridge by default
- A default Event bus for the account
- In CloudWatch Events this is the only bus (implicit)
- EventBridge can have additional buses
- Rules match incoming events (or schedules)
- Route the events to 1+ targets e.g. Lambda

Default Event Bus
- When an EC2 changes state, an event is added to the event bus
- Events themselves are just JSON structures, and can be passed to various targets (like Lambda)

### Misc. Notes
- CloudWatch has available Amazon EC2 Metrics for you to use for monitoring CPU utilization, Network utilization (network packets out of an EC2 instance), Disk performance, and Disk read/writes. In case you need to monitor the below items, you need to prepare a custom metric using a Perl or other shell script, as there are no ready to use metrics for:
	- Memory Utilization
	- Disk swap utilization
	- Disk space utilization
	- Page file utilization
	- Log collection
- Take note that you can't use the default metrics of CloudWatch to monitor the SwapUtilization metric. To monitor custom metrics, you must install the CloudWatch agent on the EC2 instance. After installing the CloudWatch agent, you can now collect system metrics and log files of an EC2 instance.
- Using CloudWatch alarm actions, you can create alarms that automatically stop, terminate, reboot, or recover your EC2 instances. You can use the stop or terminate actions to help you save money when you no longer need an instance to be running. You can use the reboot and recover actions to automatically reboot those instances or recover them onto new hardware if a system impairment occurs. You _could_ do something such as writing a python script that queries the EC2 API for each instance status check, writing a shell script that periodically shuts down and starts instances based on certain stats, etc. but these solutions are all unnecessary to go through such lengths when CloudWatch Alarms already has such a feature for you, offered at a low cost.
- If you need to collect CPU utilization of individuals processes that are running on a Linux-based EC2 server, you should use the Amazon CloudWatch agent `procstat` plugin.
- [[rds]] Enhanced Metrics that you can view in CloudWatch:
	- RDS Child Processes
		- Shows a summary of the RDS processes that support the DB instance, for example aurora for Amazon Aurora DB clusters and mysqld for MySQL DB instances. Process threads appear nested beneath the parent process. Process threads show CPU utilization only as other metrics are the same for all threads for the process. The console displays a maximum of 100 processes and threads. The results are a combination of the top CPU consuming and memory consuming processes and threads. If there are more than 50 processes and more than 50 threads, the console displays the top 50 consumers in each category. This display helps you identify which processes are having the greatest impact on performance.
	- RDS Processes
		- Shows a summary of the resources used by the RDS management agent, diagnostics monitoring processes, and other AWS processes that are required to support RDS DB instances.
	- OS Processes
		- Shows a summary of the kernel and system processes, which generally have minimal impact on performance.

### Basic Monitoring and Detailed Monitoring
- [AWS Docs: Basic monitoring and detailed monitoring](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-metrics-basic-detailed.html)
CloudWatch provides two categories of monitoring: _basic monitoring_ and _detailed monitoring_.

Many AWS services offer basic monitoring by publishing a default set of metrics to CloudWatch with no charge to customers. By default, when you start using one of these AWS services, basic monitoring is automatically enabled. For a list of services that offer basic monitoring, see [AWS services that publish CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/aws-services-cloudwatch-metrics.html).

Detailed monitoring is offered by only some services. It also incurs charges. To use it for an AWS service, you must choose to activate it. For more information about pricing, see [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing).

Detailed monitoring options differ based on the services that offer it. For example, Amazon EC2 detailed monitoring provides more frequent metrics, published at one-minute intervals, instead of the five-minute intervals used in Amazon EC2 basic monitoring. Detailed monitoring for Amazon S3 and Amazon Managed Streaming for Apache Kafka means more fine-grained metrics.

In different AWS services, detailed monitoring also has different names. For example, in Amazon EC2 it is called detailed monitoring, in AWS Elastic Beanstalk it is called enhanced monitoring, and in Amazon S3 it is called request metrics.

Using detailed monitoring for Amazon EC2 helps you better manage your Amazon EC2 resources, so that you can find trends and take action faster. For Amazon S3 request metrics are available at one-minute intervals to help you quickly identify and act on operational issues. On Amazon MSK, when you enable the `PER_BROKER`, `PER_TOPIC_PER_BROKER`, or `PER_TOPIC_PER_PARTITION` level monitoring, you get additional metrics that provide more visibility.

The following list shows the services that offer detailed monitoring.
- Amazon API Gateway
- Amazon CloudFront
- Amazon EC2
- Elastic Beanstalk
- Amazon Kinesis Data Streams
- Amazon MSK
- Amazon S3

## #sysops Scenarios
**Question**: A SysOps team is in the process of automating tasks to expedite the time to recover Amazon instances in the event of underlying hardware failure. The team must ensure that the attached Elastic IP and the private IP address of the original instance are retained after the instance is recovered. Additionally, an automated email notification should be in place to inform everyone in the SysOps team once a recovery process is triggered to run.
**Answer**: You can create an Amazon CloudWatch alarm that monitors an Amazon [EC2](ec2.md) instance and automatically recovers the instance if it becomes impaired due to an underlying hardware failure or a problem that requires AWS involvement to repair. Terminated instances cannot be recovered.
A recovered instance is identical to the original instance, including the instance ID, private IP addresses, Elastic IP addresses, and all instance metadata. If the impaired instance has a public IPv4 address, the instance retains the public IPv4 address after recovery. If the impaired instance is in a placement group, the recovered instance runs in the placement group.
When the `StatusCheckFailed_System` alarm is triggered, and the recovery action is initiated, you will be notified by the Amazon SNS topic that you selected when you created the alarm and associated the recover action. During instance recovery, the instance is migrated during an instance reboot, and any data that is in-memory is lost. When the process is complete, information is published to the SNS topic you've configured for the alarm. Anyone who is subscribed to this SNS topic will receive an email notification that includes the status of the recovery attempt and any further instructions. You will notice an instance reboot on the recovered instance.
It's worth noting here that the `StatusCheckFailed_Instance` metric in CloudWatch just monitors the software and network configuration of your individual instance. It does not detect if the underlying hardware failed.