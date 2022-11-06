---
title: "IAM"
tags:
- aws
- aws-permissions
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Identity Access Management**

### IAM Identity Policy Document
- List of statements that grant or deny permissions to AWS services
	- SID: Optional, but best practice to use
	- Effect
	- Action
	- Resource
- The identity needs to prove to AWS who it is (authentication)
- Explicit allows take effect, unless there is also an explicit deny
- Explicit deny statements overrule everything, nothing can overrule it
	- A deny statement on an IAM policy always takes precedence over an allow statement. For example, if your policy allows PUT and DELETE actions to a DynamoDB table, but your policy also denies all DynamoDB actions, the policy will not allow any actions on your AWS accounts DynamoDB tables.
- Default is a implicit deny (If you're not allowed, you're denied)

### Managed Policies
- Reusable
- Low management overhead
- Good for granting access across many people that will be using AWS in your organization
- AWS Managed Policies can be used, as well as Custom Managed Policies

### IAM Users
- IAM Users are an identity used for anything requiring long term AWS access e.g. Humans, Applications or service accounts
- Principal = An unidentified entity, not yet authenticated or authorized, attempting to access an AWS account
- Principals make requests to IAM in order to be authenticated
- Principal claims to be Sally, and proves by authenticating using a Username/Password (Console UI) or Access Key (application/cli)
- Authenticated Identity is an entity that has proved it is authenticated
- Once an authenticated Identity tries to do something like terminate an EC2 instance, or upload to an S3 bucket, AWS checks that that Identity is authorized to perform that action, and that is the process of authorization

#### IAM User Limits
- 5,000 IAM Users per account
- IAM Users can only be a member of 10 groups
- This has systems design impacts...
- Internet-scale applications, large orgs & org merges
- IAM Roles & Identity Federation fix this

### IAM Groups
- IAM Groups are containers for IAM Users
- You cannot log into a group
- Used solely for organization of IAM Users
- There is no "All Users" group in IAM, unless you create one and manage it yourself
- No nested groups
- Groups are not a true identity. They can't be referenced as a principal in a policy
- Groups offer less functionality than you would think, keep this in mind

### IAM Roles
- A role is a type of identity that exists in AWS
- If you cannot identify the number of principals that use an IAM User, it could be a good candidate for a role
	- One example is a `company-microservice-dev` role, which is used by multiple IAM Users (in the form of software engineers, that want to use that role to access resources related to that particular service, in a development account)
- Roles are generally used on a temporary basis, to use the role's permissions to perform some actions, and then stop using that role
- IAM users can have identity permissions policies attached to them via inline JSON or managed policies
- IAM roles have 2 types of policies that can be attached
	- Trust Policy - Controls which identities can assume the role
	- Permissions Policy - Controls which permissions the role has authorization to perform
- If a role is assumed by something which is allowed to assume it, AWS generates a temporary set of security credentials using the AWS STS (Secure Token Service), which are time limited
- Roles can be referenced within resource policies

When might you use roles?
- AWS Lambda (Lambda assumes Lambda Execution Role at invocation time to read/write to an S3 bucket)
- Single Sign-on or >5000 Identities
- External accounts can't be used in AWS Directly
- Designing an architecture for a popular mobile application with millions of users
	- Because of the 5000 user limit, your users would need a role to interact
		- This is good because there are no AWS credentials on the app
			- Scales to 100,000,000's of accounts
- Cross account permissions

### Service Linked Roles
- An IAM Role linked to a specific AWS service
- Provides a set of permissions, which are predefined by a service
- Provides permissions that a service needs to interact with other AWS services on your behalf
- Service might create/delete the role or allow you to create the role during the setup or within IAM
- Key Difference: You cannot delete a service linked role until it's no longer required
- To create a role, you need `iam:CreateServiceLinkedRole` attached to an AWS service role
- You also need `iam:AttachRolePolicy` and `iam:PutRolePolicy`

### SAML2.0 Identity Federation
- Identity Federation is the process of using an identity from another identity provider to access AWS resources.
- In AWS though, this can't be direct. Only AWS credentials can be used to access AWS resources.
- So, some form of exchange is required.
- SAML 2.0 stands for Security Assertion Markup Language (V2 of the standard)
- Open standard used by many ldP's (e.g. MS ADFS)
- Indirectly use on-premises IDs with AWS (Console & CLI)
- SAML is used, when you currently use an Enterprise Identity Provider that is also SAML 2.0 Compatible
- You wouldn't use it with Google, Facebook, Twitter
- You would use it if you have an existing identity management team
- You would use it if you require a single source of truth with more than 5,000 users
- Uses IAM Roles & AWS Temporary Credentials (12 hour validity)
- YOU CANNOT USE EXTERNAL IDENTITIES TO DIRECTLY ACCESS AWS. There is always an exchange, and you get temporary role based credentials.

#### API/CLI Credential Process
- The Identity Provider (e.g ADFS) establishes a two way trust with IAM
	- AWS registered in IDP, SAML IdP created in IAM - two way trust
	- AWS Account is configured to allow SAML2.0 based federation
- SAML communicates with IAM to fetch credentials by using the `sts:AssumeRoleWithSAML` permission
- IAM Role Assumed and Temporary Security Credentials are returned
- Now the on-premises application can access, say, a DynamoDB using said credentials

#### Console Credential Process
- Mostly the same, but now it's a user who wants to access the API console
- There is still a trust configured, but this time it is between the Identity Provider (e.g ADFS) and the SAML/SSO endpoint in AWS Account
- Temporary security credentials are created, and a temporary set of sign on credentials are created, as well as a console sign-in URL with credentials
- SAML endpoint constructs this sign-in URL, and gives it to the user/client

### ARNs
- Uniquely identify resources within any AWS accounts
- `arn:partition:service:region:account-id:resource-type/resource-id`

## #aws-sysops Scenarios
---
**Question**: A SysOps Administrator needs to grant a user the ability to pass any of the approved set of roles to the Amazon EC2 service upon launching an instance. This will enable the user to start an EC2 instance with an assigned role. In effect, the applications running on the instance can access temporary credentials for the role through the instance profile metadata. What must the Administrator do to accomplish this requirement?
**Answer**: To configure many AWS services, you must pass an IAM role to the service. This allows the service to later assume the role and perform actions on your behalf. You only have to pass the role to the service once during setup, and not every time the service assumes the role. For example, assume that you have an application running on an Amazon EC2 instance. That application requires temporary credentials for authentication, and permissions to authorize the application to perform actions in AWS. When you set up the application you must pass a role to EC2 to use with the instance that provides those credentials. You define the permissions for the applications running on the instance by attaching an IAM policy to the role. The application assumes the role every time it needs to perform the actions that are allowed by the role.
To pass a role (and it's permissions) to an AWS service, a user must have permissions to pass the role to the service. This helps administrators ensure that only approved users can configure a service with a role that grants permissions. To allow a user to pass a role to an AWS service, you must grant the `PassRole` permission to the user's IAM user, role, or group.
If you want to grant a user the ability to pass any of an approved set of roles to the Amazon EC2 service upon launching an instance, you need these three elements:
1. An IAM permissions policy attached to the role determines what the role can do. You should scope permissions to only the actions that the role must perform, and to only the resources that the role needs for those actions.
2. A trust policy for the role that allows the service to assume the role. You could attach a trust policy to the role with the `UpdateAssumeRolePolicy` action. With a trust policy, it allows Amazon EC2 to use the role and the permissions attached to the role.
3. Another IAM permissions policy which is attached to the IAM user allows the user to pass only those roles that are approved. The `iam:PassRole` permission usually is accompanied by `iam:GetRole` permission so that the user can get the details of the role to be passed.

---
**Question**: An administrator has launched new AWS accounts. Management wants that IAM users across all accounts be able to sign in using a single login URL as shown below:

`https://connerv.signin.aws.amazon.com/console`

How can the administrator meet the requirement?

**Answer**: Having a single login URL for different AWS accounts is not possible.

The AWS account root user and AWS Identity and Access Management (IAM) users in the account sign in using a web URL. The sign-in page URL for your account's IAM users has the following format, by default: `https://123456789123.signin.aws.amazon.com/console/`

If you create an AWS account alias for your AWS account ID, the IAM user sign-in page URL looks like the following example: `https://connerv.signin.aws.amazon.com/console`

Your AWS account can only have one alias. If you create a new alias for your AWS account, the new alias overwrites the previous alias. The URL containing the previous alias stops working. Also, the account alias must be unique across all AWS products and must contain only lowercase letters, digits, and hyphens.