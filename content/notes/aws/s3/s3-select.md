---
title: "S3 Select"
tags:
- aws
- s3
- sql
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [S3](/notes/aws/s3/s3.md)
- [Athena](/notes/aws/athena.md)

## **S3 Select**
- Amazon S3 Select is designed to help analyze and process data within an object in Amazon S3 buckets, faster and cheaper. It works by providing the ability to retrieve a subset of data from an object in Amazon S3 using simple SQL expressions. Your applications no longer have to use compute resource to scan and filter the data from an object, potentially increasing query performance by up to 400%, and reducing query costs as much as 80%. You simple change your application to use SELECT instead of GET to take advantage of S3 Select.
- Reduces situations where you have to download massive objects, and incur a large data OUT transfer cost
- Supports CSV, JSON, Parquet, and BZIP2 compression for CSV and JSON
- Glacier is also supported by Glacier Select