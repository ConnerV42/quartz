### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Access Logs
- Typically, you'll have a Source Bucket and a Target Bucket
- You must enable access logging inside of the Source Bucket to use this feature
- Log Delivery Group needs write access to the Target Bucket
- Log files are delivered in new line delimited files that contain records. Attributes in a record are space delimited.
- You personally manage the lifecycle of the log files
- Server access logging provides detailed records for the requests that are made to a bucket. Server access logs are useful for many applications. For example, access log information can be useful in security and access audits. It can also help you learn about your customer base and understand your Amazon S3 bill.

#aws #s3 #aws-sysops 