---
title: "S3 Inventory"
tags:
- aws
- s3
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Inventory
Amazon S3 Inventory is one of the tools Amazon S3 provides to help manage your storage. You can use it to audit and report on the replication and encryption status of your objects for business, compliance, and regulatory needs. You can also simplify and speed up business workflows and big data jobs using Amazon S3 Inventory, which provides a scheduled alternative to the Amazon S3 synchronous `List` API operation. Amazon S3 Inventory does not use the `List` API to audit your objects and does not affect the request rate of your bucket.

Amazon S3 Inventory provides comma-separated values (CSV), [Apache optimized row columnar (ORC)](https://orc.apache.org/) or [Apache Parquet](https://parquet.apache.org/) output files that list your objects and their corresponding metadata on a daily or weekly basis for an S3 bucket or a shared prefix (that is, objects that have names that begin with a common string). If weekly, a report is generated every Sunday (UTC) after the initial report. For information about Amazon S3 Inventory pricing, see [Amazon S3 pricing](https://aws.amazon.com/s3/pricing/).

You can configure multiple inventory lists for a bucket. You can configure what object metadata to include in the inventory, whether to list all object versions or only current versions, where to store the inventory list file output, and whether to generate the inventory on a daily or weekly basis. You can also specify that the inventory list file be encrypted.

You can query Amazon S3 Inventory using standard SQL by using [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/what-is.html), Amazon Redshift Spectrum, and other tools such as [Presto](https://prestodb.io/), [Apache Hive](https://hive.apache.org/), and [Apache Spark](https://databricks.com/spark/about/). You can use Athena to run queries on your inventory files. You can use it for Amazon S3 Inventory queries in all Regions where Athena is available.

- Helps you manage (at a high level) your storage
- Inventory of objects and various optional fields
- Some of the fields include
	- Encryption
	- Size
	- Last Modified
	- Storage Class
	- Version ID
	- Replication Status
	- Object Lock
	- etc.
- You can generate reports daily or weekly. You cannot "force" generate a report, either. You have to create a configuration, specify whether you want daily or weekly, and then that process will run in the background. It can take up to 48 hours to get the first inventory report.
- Outputs are CSV, Apache ORC, or Apache 1Parquet
- Multiple inventories can be setup, and they go to a target bucket in the same or a different account. Needs a bucket policy.
- Used for Audit, Compliance, Cost Management or any specific regulations