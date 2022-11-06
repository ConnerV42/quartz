---
title: "Data Lifecycle Manager"
tags:
- aws
- ebs
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **DLM - Data Lifecycle Manager** (EBS Volume Snapshot Management)
- You can use **Amazon Data Lifecycle Manager (Amazon DLM)** to automate the creation, retention, and deletion of snapshots taken to back up your Amazon [EBS](/notes/aws/ebs.md) volumes. Automating snapshot management helps you to:
	- Protect valuable data by enforcing a regular backup schedule.
	- Retain backups as required by auditors or internal compliance.
	- Reduce storage costs by deleting outdated backups.
- Combined with the monitoring features of Amazon CloudWatch Events and AWS CloudTrail, Amazon DLM provides a complete backup solution for EBS volumes at no additional cost. It is the fastest and most cost effective way to solution for automatically backing up your EBS volumes.