---
title: "AWS KMS"
tags:
- aws
- aws-security
- encryption
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)
- [CloudHSM](/notes/aws/cloudhsm.md)

## **Key Management Service (KMS)**
- Regional and Public Service
	- Every region is isolated when using KMS
- Create, Store and Manage Cryptographic Keys
	- Allows creation of both symmetric and asymmetric keys
	- Allows you to perform cryptographic operations (encryption, decryption, etc.)
		- You don't perform the operations directly, you just pass instructions to KMS in the form of API requests, etc.
	- Cryptographic keys never actually leave KMS
- KMS is very granular with permissions. You need specific permissions for each operation you attempt to do in the service. You also need specific permissions on specific keys in order to use those keys.
- KMS provides FIPS 140-2 (Level 2) compliance
	- This is a US security standard	
- AWS Key Management Service allows you to protect against unauthorized access of your objects in Amazon S3. SSE-KMS provides you with an audit trail that shows when your CMK was used and by whom. Additionally, you can create and manage customer-managed CMKs or use AWS managed CMKs that are unique to you, your service and your Region.

### KMS Keys
- KMS Keys are logical - ID, date, policy, desc & state
- KMS Keys are backed by physical key material
- Generated or Imported
- KMS Keys can be used for up to 4KB of data
- KMS keys are isolated to a region and never leave KMS
	- KMS does support multi-region keys
	- KMS uses AWS owned and Customer owned keys
		- AWS Owned keys are largely used by services in the background and don't concern us
		- Customer Owned Keys come in the form of AWS Managed or Customer Managed Keys
			- Customer Managed Keys are more configurable and must be created explicitly
			- KMS Keys support rotation
			- Rotation is optional with Customer Managed Keys
			- When rotation occurs, backing key and previous backing keys are retained

### Data Encryption Keys
- KMS can generate DEKs, or Data Encryption Keys, which are generated by a KMS key, and can be used on > 4KB of data.
- The `GenerateDataKey` KMS operation can be used to generate a DEK.
- DEKs are linked to the KMS key that it was created with. KMS does NOT store the Data Encryption Key in any way. It provides it to you, or the service using it, and then discards it.
- When DEK is generated, you get a plaintext version, that can be used immediately and discarded, and a Ciphertext version, meant to be saved, that can be decrypted in the future by KMS, so that the plaintext version can be retrieved once again by an API call to KMS.
- KMS doesn't track usage of DEKs, or do the encryption/decryption itself

### Key Policies and Security
- KMS is different than other AWS services with regards to permissions.
- Key Policies (Resource Policy)
	- Every Key has one, and for Customer Managed Keys, it can be changed
	- Unlike other AWS services, KMS has to specifically be told to trust that keys trust the AWS account that they are contained in. You always need this policy to be in place, in order to use KMS.
	- Key Policies are used in tandem with IAM identity policies