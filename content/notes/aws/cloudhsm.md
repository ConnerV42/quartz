---
title: "CloudHSM"
tags:
- aws
- aws-security
- encryption
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **CloudHSM** 
- A cloud based hardware security modules (HSM) that enables you to easily generate and use your own encryption keys on the AWS Cloud. With CloudHSM, you can manage your own encryption keys using FIPS 140-2 Level 3 validated HSMs. CloudHSM offers you the flexibility to integrate with your applications using industry-standard APIs, such as PKCS#11, Java Cryptography Extensions (JCE), and Microsoft CryptoNG (CNG) libraries.
  - If the HSM is zeroized (which can happen if an administrator attempts to login 3 times and fails), the keys will be lost permanently if you do not have a copy.