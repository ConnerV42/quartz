---
title: "AWS Batch"
tags:
- aws
- serverless
- aws-compute
- containers
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Batch**
- AWS Batch is aimed at the specific use case of executing batch jobs that are pulled from a queue. You would generally use Batch in your backend processes to take some data and then process it using containerized processes. The batch jobs in AWS Batch should run to completion then exit. AWS Batch is hyper focused on providing a good experience for backend batch workloads.