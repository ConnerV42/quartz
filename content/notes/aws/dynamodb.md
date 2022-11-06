---
title: "DynamoDB"
tags:
- aws
- database
- no-sql
- dynamodb
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [DAX - DynamoDB Accelerator](/notes/aws/dynamodb-accelerator.md)

## **DynamoDB** 
- The partition key portion of a table's primary key determines the logical partitions in which a table's data is stored. This is turn affects the underlying physical partitions. Provisioned I/O capacity for the table is divided evenly among these physical partitions. Therefore a partition key design that doesn't distribute I/O requests evenly can create "hot" partitions that result in throttling and use your provisioned I/O capacity inefficiently. The optimal usage of a table's provisioned throughput depends not only on the workload patterns of individual items, but also on the partition-key design. This doesn't mean that you must access all partition key values to achieve an efficient throughput level, or even that the percentage of accessed partition key values must be high. It does mean that the more distinct partition key values tat your workload accesses, the more those requests will be spread across the partitioned space. In general, you will use your provisioned throughput more efficiently as the ratio of partition key values accessed to the total number of partition key values increases. One example for this is the use of partition keys with high-cardinality attributes, which have a large number of distinct values for each item.
- **Scaling**
	- A relational database system does not scale well for the following reasons:
		- It normalizes data and stores it on multiple tables that require multiple queries to write to disk.
		- It generally incurs the performance costs of an ACID-compliant transaction system.
		- It uses expensive joins to reassemble required views of query results.
	- DynamoDB scales well due to these reasons:
		- It's schema flexibility lets DynamoDB stores complex hierarchical data within a single item. DynamoDB is not a totally *schemaless* database since the very definition of a schema is just the model or structure of your data.
		- Composite key design lets it store related items close together on the same table.
- DynamoDB allows you to store session state data on DynamoDB
- A **DynamoDB Stream** is an ordered flow of information about changes to items in an Amazon DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table. Whenever an application creates, updates, or deletes items in the table, DynamoDB Streams writes a stream record with the primary key attribute(s) of the items that were modified. A *stream record* contains information about a data modification to a single item in a DynamoDB table. You can configure the stream so that the stream records capture additional information such as the "before" and "after" images of modified items. DynamoDB is integrated with Lambda so that you can create *triggers* - pieces of code that automatically respond to events in DynamoDB Streams. With triggers, you can build applications that react to data modifications in DynamoDB tables. If you enable Dynamo Streams on a table, you can associate the stream ARN with a Lambda function that you write. Immediately after an item in the table is modified, a new record appears in the table's stream. AWS Lambda polls the stream and invokes your Lambda function synchronously when it detects new stream records. The Lambda function can perform any actions you specify, such as sending a notification or initiating a workflow.