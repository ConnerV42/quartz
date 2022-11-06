---
title: "Kinesis"
tags:
- aws
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Kinesis Data Streams**
- Amazon Kinesis Data Streams (KDS) is a massively scalable and durable real-time data streaming service. KDS can continuously capture gigabytes of data per second from hundreds of thousands of sources. You can use an AWS Lambda function to process records in Amazon KDS. By default, Lambda invokes your function as soon as records are available in the stream. Lambda can process up to 10 batches in each shard simultaneously. If you increase the number of concurrent batches per shard, Lambda still ensures in-order processing at the partition-key level.
- Since Kinesis Data Streams is a real-time data streaming service that requires the provisioning of shards, if there is no requirement for real-time processing in your scenario, replacing Kinesis Data Streams with something like Amazon SQS, a cheaper option, is advantageous because you only pay for what you use.
- By default, the data records are only accessible for 24 hours from the time they are added to a Kinesis stream.
- In Amazon Kinesis Data Streams, consumers (such as a custom application running on EC2, or a Kinesis Data Firehose delivery stream) can store their results using an AWS service such as DynamoDB, Redshift, or Amazon S3) can store their results using an AWS service such as DynamoDB, Redshift, or Amazon S3.
## **Kinesis Data Firehose**
- Amazon Kinesis Data Firehose is the easiest way to load streaming data into data stores and analytic tools. It can capture, transform, and load streaming data into Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk, enabling near real-time analytic with existing business intelligence tools and dashboards you are already using today.
- It is a fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration. It can also batch, compress, and encrypt the data before loading it, minimizing the amount of storage used at the destination and increasing security.
- You can gather data from anywhere, and use Kinesis Data Firehose to prepare and load the data. S3 will be used as a method of durably storing the data for analytic and the eventual ingestion of data for output using analytical tools.
- You can use Amazon Kinesis Data Firehose in conjunction with Amazon Kinesis Data Streams if you need to implement real-time processing of streaming big data. Kinesis Data Streams provides an ordering of records, as well as the ability to read and/or replay records in the same order to multiple Amazon Kinesis Applications. The Amazon Kinesis Client Library (KCL) delivers all records for a given partition key to the same record processor, making it easier to build multiple applications reading from the same Amazon Kinesis Data Stream (for example, to perform counting, aggregation, and filtering).
- Amazon SQS is different from Amazon Kinesis Data Firehose. SQS offers a reliable, highly scalable hosted queue for storing messages as they travel between computers. Amazon SQS lets you easily move data between distributed application components and helps you build applications in which messages are protected independently (with message-level ack/fail semantics), such as automated workflows. Amazon Kinesis Data Firehose is primarily used to load streaming data into data stores and analytic tools.