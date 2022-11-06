---
title: "S3 Encryption"
tags:
- aws
- s3
- encryption
disableToc: false
---

### Related Notes
- [S3](/notes/aws/s3/s3.md)

## S3 Encryption
- Buckets aren't encrypted, encryption is defined at the object level
- 2 methods of encryption that S3 supports, both are encryption at rest. Encryption at transit is a separate topic altogether, and comes standard with S3.
	- Client side encryption - Objects are encrypted by the client before they are ever uploaded to an S3 bucket. The data is cipher text the entire time. It is received in scrambled form, and stored in scrambled form.
	- Server side encryption - Data is encrypted once it hits the S3 endpoint, and it is stored in it's encrypted form.

### Types of S3 Server Side Encryption

#### Server-Side Encryption with Customer-Provided Keys (SSE-C)
- S3 manages cryptographic operations
- This method offloads the CPU capacity of encryption/decryption to S3
- You need to supply the key that will be used by S3 at the time that you upload an object.
- A hash of the key is taken, this hash cannot be used to generate a new key, but it verifies that the decryption key passed to S3 during a decryption operation is valid. Essentially a safety feature.
- You must provide encryption key information using the following request headers:
	- `x-amz-server-side-encryption-customer-algorithm`
	- `x-amz-server-side-encryption-customer-key`
	- `x-amz-server-side-encryption-customer-key-MD5`

#### Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)
- With this method, S3 handles both the encryption/decryption process, as well as the key generation and management.
- Uses AES-256
- Usually the best choice for most applications
- Bad choice if storing health or medical data that requires strong role separation

#### Server-Side Encryption with KMS Keys (SSE-KMS)
- An AWS Managed KMS Key created, and a unique DEK (Data Encryption Key) is used to encrypt each object.
- Gives fine-grained control over key rotation.
- Allows for role separation, because specific KMS key permissions are required on a per-key basis

### Default Bucket Encryption
- When uploading objects to S3 using the `PutObject` operation, you can specify as specific header, known as `x-amz-server-side-encryption`. This is how you direct S3 to use server side encryption.
- However, you can set this as a default at the bucket level in S3. A specified header will always take priority over a set default.