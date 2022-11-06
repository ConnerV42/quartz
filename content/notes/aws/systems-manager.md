---
title: "Systems Manager"
tags:
- aws
- aws-sysops
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## **Systems Manager**

### Helpful Links
- [Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)

### Overview
- AWS Systems Manager Run Command lets you remotely and securely manage the configuration of your managed instances. A managed instance is any Amazon EC2 instance or on-premises machine in your hybrid environment that has been configured for Systems Manager. Run Command enables you to automate common administrative tasks and perform ad-hoc configuration changes at scale. You can use Run Command from the AWS console, the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDKs. Run Command is offered at no additional cost.

- Allows you to view and control AWS and on-premises infrastructure
- Especially helpful for hybrid environments
- Agent based - installed on windows and Linux AWS AMI's
	- Handled manually on on-premises environments
- Manages Inventory on instances that have the agent installed
	- What applications are installed
	- The files on the instance
	- The Network configuration
	- Operating System patches and hotfixes
	- Running services on the instance
	- Instance hardware details
	- Even allows custom configuration
- Patching Automation
  - Defines maintenance windows
	- Automatic patching
- Runs commands and managed desired state
- Securely connect to EC2, even in private VPCs

### SSM configuration requirements for AWS fleet
- Agent installed on each AWS instance
- Connectivity to the AWS Public Zone endpoint (Systems Manager Endpoint)
- IAM role providing permissions

### SSM configuration for on-premises fleet
- Managed instance activation: Process of adding on-premises servers to SSM
	- Activation Code
	- Activation ID
	- IAM Role
- Activations securely join on-premises servers to Systems Manager and configure the IAM Role to use

### SSM Run Command
- In short, run any command you can define, across your entire fleet
- Also, it supports other Systems Manager features, like Patch Management
- Allows you to run [Command Documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html) on managed instances
- No SSH/RDP access required
- Instances, Tags, or Resource Groups
- Command Documents define what steps a command has
	- Run Shell commands
	- Joining machine to a domain
	- Anything you can define
- Command Documents can be reused and can have parameters
- Rate Control allows you to control the following:
	- Concurrency (how many instances should you run the command on at a single time)
	- Error Threshold (how many individual commands running on individual instance can fail, before the whole command fails)
- Output Options - S3 or SNS
- EventBridge (CloudWatch Events) Targets

### SSM Documents
- JSON or YAML documents, similar to CloudFormation Templates, but for configuration specific to Systems Manager itself
- Systems Manager includes more than 100 pre-configured documents that you can use by specifying parameters at runtime
- Stored in a place called the `SSM Document Store`
- Ask for Parameters and include steps
- SSM Documents includes Command Documents - used by Run Command, State Manager & Maintenance WIndows
- Automation Documents - used by Automation, State Manager & Maintenance Windows
	- Used for creating/updating AMI's, for example
- Package Documents - Distributor
	- Represents software or any other assets installed on instances
	- For example, there exists a commonly used SSM Document called `AWS-ConfigureAWSPackage`, which is commonly used to install the CloudWatch Agent on SSM managed EC2 instances

### SSM Inventory & SSM Patching
#### Patch Manager
Patch Manager, a capability of AWS Systems Manager, automates the process of patching managed instance with both security related and other types of updates
- Patch Baseline
	- Defines what should be installed
		- What patches, and what hotfixes
- Patch Groups
	- Which specific resources should be patched?
- Maintenance Windows
	- Define the time slot when patching can take place
- Run Command - Used as the base level functionality to manage the patching process, actually performs the process
- Concurrency and Error Threshold
	- Concurrency (how many instances should you run the command on at a single time)
	- Error Threshold (how many individual commands running on individual instance can fail, before the whole command fails)
- Compliance: Systems Manager can determine after the fact, whether a patch has been applied successfully

#### Patch Manager - Key Terms to Memorize for #aws-sysops #sysops
- Predefined Patch Baselines - Various OS (you can also create your own) 
- For Linux - AWS-OSDefaultPatchBaseline, explicitly define patches
- .. AWS-AmazonLinux2DefaultPatchBaseline
- .. AWS-UbuntuDefaultPatchBaseline
- Windows - AWS-DefaultPatchBaseline - Critical and Security Updates
- AWS-WindowsPredefinedPatchBaseline-OS - Same as above
- AWS-WindowsPredefinedPatchBaseline-OS-Applications = + MS App Updates
- `AWS-RunPatchbaseline` is the Run Command that runs with a baseline and target to actually patch the machines

### SSM Parameter Store
- Stores configuration and secrets

### Session Manager
- Session Manager is a fully managed AWS Systems Manager capability. With Session Manager, you can manage your Amazon Elastic Compute Cloud (Amazon EC2) instances, edge devices, and on-premises servers and virtual machines (VMs). You can use either an interactive one-click browser-based shell or the AWS Command Line Interface (AWS CLI). Session Manager provides secure and auditable node management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys. Session Manager also allows you to comply with corporate policies that require controlled access to managed nodes, strict security practices, and fully auditable logs with node access details, while providing end users with simple one-click cross-platform access to your managed nodes. To get started with Session Manager, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/session-manager). In the navigation pane, choose **Session Manager**.

### Systems Manager Inventory
- Collects a list of software running on managed instances
AWS Systems Manager Inventory provides visibility into your AWS computing environment. You can use Inventory to collect _metadata_ from your managed nodes. You can store this metadata in a central Amazon Simple Storage Service (Amazon S3) bucket, and then use built-in tools to query the data and quickly determine which nodes are running the software and configurations required by your software policy, and which nodes need to be updated. You can configure Inventory on all of your managed nodes by using a one-click procedure. You can also configure and view inventory data from multiple AWS Regions and AWS accounts.To get started with Inventory, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/inventory). In the navigation pane, choose **Inventory**.

If the pre-configured metadata types collected by Systems Manager Inventory don't meet your needs, then you can create custom inventory. Custom inventory is simply a JSON file with information that you provide and add to the managed node in a specific directory. When Systems Manager Inventory collects data, it captures this custom inventory data. For example, if you run a large data center, you can specify the rack location of each of your servers as custom inventory. You can then view the rack space data when you view other inventory data.