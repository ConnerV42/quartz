---
title: "CloudFormation"
tags:
- aws
- infrastructure-as-code
disableToc: false
---

### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## Resources
- [CloudFormation Docs](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/Welcome.html)
- [CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
	- [CloudFormation Template Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-reference.html)

## CloudFormation
- CloudFormation begins with a template, which is a document written in JSON or YAML.
```yaml
Resources:
  Instance: ## Name of the logical resource (Can be anything)
    Type: 'AWS::EC2::Instance'
		Properties:
		  ImageId: !Ref LatestAmiId
		  InstanceType: "t3.micro"
			KeyName: 'A4L' ## SSH key to use
```
- The above CloudFormation Template can be used to create many different Stacks. A Stack creates, updates, and deletes physical resources based on logical resources in the template.
- Once a logical resource moves to `create_complete` (meaning the physical resource is active), it can be queried for attributes of the physical resource within the template.
- Keep in mind that when a Stack is deleted, the physical resources that it manages in the AWS Cloud are also deleted.

### Non-Portable Template Example
```yaml
Resources:
  Bucket: 
    Type: 'AWS::S3::Bucket'
		Properties:
		  BucketName: 'accatpics13333337'
  Instance:
    Type: 'AWS::EC2::Instance'
		Properties:
			KeyName: 'A4L'
		  InstanceType: 't2.micro'
		  ImageId: 'ami-04d29b6f966df1537' ## AMI ID for Amazon Linux 2
```

### Template Parameters and Pseudo Parameters
- Both parameter types allow inputs into CloudFormation templates
- These parameters can be used in a complementary way

#### Template Parameters
- Template Parameters accept input from the console / CLI / API when the stack is created or updated
	- Can be referenced from within Logical Resources to influence Physical Resources
	- Can be configured with Defaults, AllowedValues, Min and Max length, AllowedPatterns, NoEcho (masked passwords) & Type
	- Example of Template Parameters below
```yaml
Parameters:
  InstanceType:
    Type: String
    Default: 't3.micro'
		AllowedValues:
		  - 't3.micro'
		  - 't3.medium'
		  - 't3.large'
		Description: 'Pick a supported InstanceType'
	InstanceAmiId:
	  Type: String
		Description: 'AMI ID For Instances.'
```

#### Pseudo Parameters
- Injected by AWS into the template or stack
- `AWS::Region` and  `AWS::AccountId` are examples of Pseudo Parameters.

#### Best Practices
- Wherever possible, use Default Template Parameters and Pseudo Parameters to reduce user input. For example, hardcoding region names is a bad practice. Use `Fn::GetAZs`.

### Intrinsic Functions
- Allows you to gain access to data at runtime
- Functions can be used together or in isolation
- `Ref` & `Fn::GetAtt`
	- Using `!Ref` on template or pseudo parameters returns their value. When used with logical resources - the physical ID is usually returned, the primary value.
		- For an EC2 instance, `!Ref Instance` would return the physical ID of the resource: `i-1234567890abcdef0`
	- `!GetAtt` can be used to retrieve any attribute associated with the resource. Most logical resources return detailed configuration of the physical resource.
		- For an EC2 instance, `!GetAtt` LogicalResource.Attribute could be used to return the PublicIP `52.91.129.183` or PublicDnsName `52.91.129.183`.
- `Fn::Join` & `Fn::Split`
	- Split accepts a single value delimiter and a string, and returns a list. Classic split function.
	- Join is the reverse, you provide a delimiter and list of values, and a string is returned.
		- Example Usage (to form a URL): `Value: !Join [ '', 'https://', !GetAtt Instance.DNSName ] ]` 
- `Fn::GetAZs` & `Fn::Select` (commonly used to pick from a list of availability zones in a given region)
  - `Fn::GetAZs` is an environment aware function. 
	  - Example Usage: `AvailabilityZone: !Select [ 0, !GetAZs '' ]`
- Conditions (Fn::IF, AND, Equals, Not & Or)
- `Fn::Base64` & `Fn::Sub`
  - Base64 converted plaintext to Base64 encoded text
	- Sub replaces variables inside of a string with a variable
		- You can't do self references with Sub
- `Fn::Cidr`
  - In the below example, We're referencing the CIDR Block that we've just created to create a subnets. The 16 is how many subnet to generate, and the 12 is Bits per CIDR (32 - 12 = /20)
```yaml
VPC:
  Type: AWS::EC2::VPC
	Properties:
	  CidrBlock: "10.16.0.0/16"
Subnet1:
  Type: AWS::EC2::Subnet
  Properties:
    CidrBlock: !Select [ "0", !Cidr [ !GetAtt VPC.CidrBlock, "16", "12" ] ]
		VpcId: !Ref VPC
```

### Mappings
- Templates can contain a Mappings object which contain many mappings, which map keys to values, allowing lookup
- Can have one key, or Top & Second Level
- Mappings use the !FindInMap intrinsic function
- Common use is to retrieve AMI for given region & architecture
- Improves Template Portability
- Example Syntax: `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]` might be used like this `!FindInMap [ "RegionMap", !Ref "AWS::Region", "HVM64"]` to retrieve an AMI for a certain region and architecture type for an EC2 instance.
	- The third parameter is not required, if omitted, the entire map object that corresponds to the TopLevelKey will be returned.

### Outputs
- Optional, but useful for providing status information
- Values can be declared in this section that are visible as outputs when using the CLI
- Visible as outputs in the console UI
- Accessible from a parent stack when using nesting
- Can be exported, allowing cross-stack references

Example Output
```yaml
Outputs:
  WordpressURL:
    Description: "Instance Web URL"
    Value: !Join [ '', 'https://', !GetAtt Instance.DNSName ] ]
```
- In this example, description will be visible from the CLI and Console UI & passed back to parent stack when nested stacks are used

### Conditions
- Allows a stack to react to certain conditions and change infrastructure when deployed, or specific configurations of that infrastructure
- Declared in an optional section of the template, the `Conditions` sections. This section is processed before resources are created
- Uses the other intrinsic functions of AND, EQUALS, IF, NOT, OR
- Associated with logical resources to control if they are created or not
- For example, PROD or DEV could control the size of instances created within a stack
- Conditions can be nested, so that a condition is only true if two other conditions are true, for example

```yaml
Parameters:
  EnvType:
    Default: 'dev'
    Type: String
    AllowedValues:
      - 'dev'
      - 'prod'
```

```yaml
Conditions:
  IsProd: !Equals
    - !Ref EnvType
    - 'prod'
```

```yaml
Resources:
  Wordpress:
    Type: 'AWS::EC2::Instance'
		Condition: IsProd
    Properties:
      ImageId: 'whatever'
```

### Depends On
- CloudFormation tries to run in parallel for create, update and delete operations
- While doing this, it tries to determine dependency order automatically
- This cannot always be done however. !Ref, for example, creates an implicit dependency
- An Elastic IP requires an IGW attached to a VPC in order to work, but if there is no explicit dependency in the template using DependsOn, it won't work (This is on the #sysops exam).

### Wait Conditions, Creation Policies & cfn-signal
- Allows systems to provide more details about resource completion to CloudFormation
- The cfn-signal command is included with the AWS CFN bootstrap package
- Allows you to configure CloudFormation to wait for N number of success signals
- Wait for Timeout H:M:S for those signals (12 hours max)
- If all success signals are received, the CREATE_COMPLETE signal goes out!
- cfn-signal is a utility running on the EC2 instance itself, explicitly sends a signal or signals to back to the CloudFormation service. If it communicates a failure, then the creation of the resource in the stack fails, and the creation of the stack itself fails.
- If the timeout period is reached, the creation of the resource and the stack itself fails.
- For provisioning EC2 or AutoScaling Groups, it's recommended to use a CreationPolicy. CreationPolicies are generally simpler to manage

##### CreationPolicy Example:
- In the below example, the CreationPolicy applies signal requirement of 3 and a timeout of 15 minutes.
- The ASG provisions 3 EC2 instances, each signalling once via cfn-signal
```yaml
AutoScalingGroup:
Type: AWS::AutoScaling::AutoScalingGroup
Properties:
  ... (stuff here)
  DesiredCapacity: '3'
	MinSize: '1'
	MaxSize: '4'
CreationPolicy:
  ResourceSignal:
	  Count: '3'
		Timeout: PT15M 
```

```yaml
LaunchConfig:
  Type:
  AWS::AutoScaling::LaunchConfiguration  
	Properties:
	  ... (stuff here)
		UserData:
		  "Fn::Base64"
			  !Sub |
			    #/bin/bash -xe
					yum update -y aws-cfn-bootstrap
					... ('some bootstrapping')
					/opt/aws/bin/cfn-signal -e $?
					--stack ${AWS::StackName}
					--resource AutoScalingGroup
					--region ${AWS::Region}

```

##### WaitCondition Example:
- WaitCondition can depend on other resources. Other resources can depend on the WaitCondition.
- The WaitHandle generates a PreSigned URL for resource signals
```yaml
WaitCondition:
  Type: AWS::CloudFormation::WaitCondition
  DependsOn: "someresource"
	Properties:
	  Handle: !Ref "WaitHandle"
		Timeout: "300"
		Count: '1'
```

```yaml
WaitHandle:
  Type: AWS::CloudFormation::WaitConditionHandle
```

### Nested Stacks (reuses the code, not the resources)
- CloudFormation stacks are isolated by default. You cannot reuse resources in another stack or reference other stacks by default.
- A CloudFormation stack is usually isolated, meaning it contains all the resources that are needed for an application.
- In this paradigm, all resources in a single stack share a lifecycle.
- However, there is a limit of 500 resources per stack.
- Additionally, you can't easily reuse resources (e.g. A VPC)

##### CFN Nested Stacks
- Parent Stack / Root Stack is anything that has it's own nested stack
- There is nothing special about this stack type. It is the same as any other stack, but it creates additional stacks like the one below.
```yaml
VPCSTACK:
  Type: AWS::CloudFormation::Stack
  Properties:
    TemplateURL: https://someurl.com/template.yaml
    Parameters:
      Param1: !Ref SomeParam1
      Param2: !Ref SomeParam2
      Param3: !Ref SomeParam3
```
- By doing this, you can reference outputs from your nested VPC stack (NOTE: You cannot reference logical resources created in any of the nested stacks).
- You can many many nested stacks created in your root stack, and if another stack depends on the VPCSTACK, for example, you can pass outputs from that stack into another nested stack that exists within the root stack.
- By breaking up solutions into modular templates, it means these templates can be resources again and again for different deployments. Many nested stack architectures can use that template. Note that the resources themselves aren't being reused, just the template. A separate VPC would be created, for example, if you were to use this template in a different stack.
- Only use Nested Stacks when the stacks are lifecycle linked.
- Nested Stacks are used when lifecycles are linked. If they aren't you may want to use cross-stack references instead.

### Cross-Stack References (reuse the actual resources in another stack)
- If you want to use the same VPC across multiple architectures, cross-stack references are probably better suited for this aim.
- Because of the isolation of stacks, outputs are normally not visible from other stacks
- Outputs can be exported, making them visible from other stacks
- Export is used to export the output of a stack. Exported outputs must have a unique name in the region
- Anything that we want to use outside of the stack, we need to set as an export:
```yaml
Outputs:
  SHAREDVPCID:
    Description: Shared Services VPC
		Value: !Ref VPC
		Export:
		  Name: SHAREDVPC
```
- To reference an exported output in another stack, use the `Fn::ImportValue` instead of `Ref`. This can only be done in the same region and AWS account.

### Stack Sets (CloudFormation stacks across AWS accounts & regions)
- Deploy CFN stacks across many accounts & regions
- StackSets are containers that live in an admin AWS account
- StackSets can contain many Stack Instances, which are not Stacks. They are "Reference Stacks", which is a container for an individual stacks that run in a particular region, in a particular account.
- Stack instances & stacks are in "Target Accounts"
- Each stack = 1 region in 1 account
- Permissions for these kind of cross account operations are granted via self-managed IAM Roles or service managed IAM roles within an organization
- TERM: Concurrent Accounts, an integer
	- This defines how many individual AWS accounts can be deployed into, at the same time
	- The higher this value, the faster your cross account deployment
- TERM: Failure Tolerance, an integer
  - The amount of individual deployments that can fail, before the entire deployment is considered a failure
- TERM: Retain Stacks, boolean
  - Remove stack instances from a stack set, by default, any stacks will be deleted from the account.
	- If true, stacks will be retained when you remove the StackSet from an AWS account
Scenario: Enable AWS Config
Scenario: AWS Config Rules - MFA, EIPS, EBS Encryption
Scenario: Create IAM Roles for cross-account access

### Deletion Policy: Tune resource deletion to take backups, to prevent data loss during stack delete operations
- If you delete a logical resource from a template, by default, the physical resource is deleted
- This can cause data loss, with RDS, example
- With deletion policy, you can define on each resource
- Delete (Default), Retain or (if supported) Snapshot
- Some of the resources that support snapshots are:
	- EBS Volumes
	- Elasticache
	- Neptune
	- RDS
	- Redshift
- If you delete a stack, with Snapshot selected, the Snapshot will persist past the deletion of the stack. It is your responsibility to clean up these snapshot resources.
- The above applies to Delete, not Replace!

### Stack Roles
- CloudFormation uses the permissions of the logged in identity, which means you or the role you've assumed, need permissions to interact with Stacks, and the resources that the stacks create themselves
- CloudFormation can assume a role to gain the permissions, which lets you implement role separation
- The identity creating the stack doesn't need the resource permissions - only `PassRole`
- Stack roles allow an IAM role to be passed into the stack via `PassRole`
- A stack uses this role, rather than the identity interacting with the stack to create, update and delete AWS resources.
- It allows role separation, and is a powerful security feature.

### cfn-init: Run once to bootstrap an EC2 instance (configure, install dependencies, etc.)
- `cfn-init` is run once as part of bootstrapping (user data) an EC2 instance (only run once, even if you update the template)
- CloudFormation init is a native CloudFormation feature
- Configuration directives stored in template
- `AWS::CloudFormation::Init` part of logical resource, and here you can specify directives of what can happen on the system
- User data is procedural - the HOW
- Init on the other hand, is a desired state - the WHAT (works across platform, even across linux/windows)
- If something already exists on instance that CloudFormation init wants, it will ignore it
  - If apache is installed for example, and CloudFormation init wants to install apache, nothing will happen, since it's already installed
- `cfn-init` helper script is installed on EC2 OS (makes it so). This is executed via user data
```yaml
EC2Instance:
  Type: AWS::EC2::Instance
  CreationPolicy: ...
  Metadata:
    AWS::CloudFormation::Init:
      configSets: ... # defines which configkeys to use and in which order to apply
      install_cfn: ...
      software_install: ...
      configure_instance: ...
      install_wordpress: ...
      configure_wordpress: ... # this is an example of a config key
```
The below is ran on EC2 startup:
```yaml
UserData:
  Fn::Base64: !Sub |
    #!/bin/bash -xe
    yum -y update
    /opt/aws/bin/cfn-init -v --stack ${AWS::StackId} --resource EC2Instance --configsets wordpress_install --region ${AWS::Region}
    /opt/aws/bin/cfn-signal -e --stack ${AWS::StackId} --resource EC2Instance --region ${AWS::Region}
```

ConfigKey could contain the following:
```
packages:
 .. 'packages to install'
groups:
 .. 'local group mgmt'
users:
 .. 'local user mgmt'
sources:
 .. 'download and extract archives'
files:
 .. 'files to create'
commands:
 .. 'commands to execute'
services:
 .. 'services to enable'
```

### cfn-hup: Trigger a `cfn-init` operation when EC2 metadata changes are detected during stack update operations
- `cfn-hup` helper is a daemon which must be installed and configured yourself (unlike `cfn-init`, which is natively available on EC2)
- Detects changes in resource metadata, and runs configurable actions when a change is detected at stack update time
- When a template is changed, an `UpdateStack` operation is ran, `cfn-hup` then checks metadata periodically, when updated, it calls `cfn-init`, and `cfn-init` applies the new configuration.
- Normal user data is, by default, only executed once per stack. If you want to monitor the logical resource and initiate a new `cfn-init` when configuration updates occur in the metadata, you must install and configure `cfn-hup`.

### ChangeSets: Preview changes to a template
- When a stack is updated, 1 of 3 things may happen. No interruption, some interruption, or full replacement of resources. ChangeSets, let you apply a new template to a stack, such that when you're about to apply a stack, you can preview what is about to happen when the template is updated.
- After creating a ChangeSet, you can then choose to apply the changes by executing the change set.
- Similar to the Terraform plan command, which outputs a plan file that then can be passed to a Terraform apply command.

### Custom Resources: Extend the functionality of CloudFormation beyond things it natively supports
- CloudFormation doesn't support everything in AWS
- Custom Resources are a type of logical resource that allows CloudFormation to do things that it doesn't natively support
- Passes event data to something, and gets data back from something. This could be a Lambda function or SNS topic, for example.
- The compute that is backing that custom resource, can pass a success or failure code back to CloudFormation.
- The data that is sent back from the custom resource can then be referenced elsewhere in your template, just like any other resource.
- One application of this, is automating the process by which objects are uploaded to an S3 bucket. Normally, S3 has a limitation such that it does not want to let CloudFormation delete a bucket that is not empty. How to use Custom Resources to get around this? We would use a Custom Resource, backed by a Lambda function, that uploads resources that we want inside of the S3 bucket. Then, we apply the template. Now, when we go to delete the bucket, CloudFormation knows the the CustomResource depends on the bucket. When CloudFormation deletes the stack, the Lambda now knows that the stack is being deleted, so it use that information to delete the objects in the bucket. At that point, the stack will delete the S3 bucket, which will succeed, because it is empty.

## #sysops Scenarios

**Question**: A SysOps Administrator needs to install and configure software applications to an EC2 instance that will be deployed using CloudFormation. The Administrator has to ensure that the applications are properly running before the stack creation proceeds. Which of the following options can satisfy the given requirement?

**Answer**: Add a `CreationPolicy` attribute to the instance then send a success signal after the applications are installed and configured. Use the `cfn-signal` helper script to signal a resource.

You can associate the `CreationPolicy` attribute with a resource to prevent its status from reaching create complete until AWS CloudFormation receives a specified number of success signals or the timeout period is exceeded. To signal a resource, you can use the `cfn-signal` helper script or SignalResource API. AWS CloudFormation publishes valid signals to the stack events so that you track the number of signals sent.

The creation policy is invoked only when AWS CloudFormation creates the associated resource. Currently, the only AWS CloudFormation resources that support creation policies are `AWS::AutoScaling::AutoScalingGroup`, `AWS::EC2::Instance`, and `AWS::CloudFormation::WaitCondition`.

Use the `CreationPolicy` attribute when you want to wait on resource configuration actions before the stack creation proceeds. For example, if you install and configure software applications running on an EC2 instance, you might want those applications to be running before proceeding. In such cases, you can add a `CreationPolicy` attribute to the instance, and then send a success signal to the instance after the applications are installed and configured.

---

**Question**: A SysOps Administrator needs to create a CloudFormation template that should automatically rollback in the event that the entire stack failed to launch. The application stack requires the pre-requisite packages to be installed first in order for it to run properly, which could take about an hour or so to complete.

What should the Administrator add in the template to accomplish this requirement?

**Answer**: In the `ResourceSignal` parameter of the `CreationPolicy` resource attribute, add a `Timeout` property with a value of 2 hours.

Associate the `CreationPolicy` attribute with a resource to prevent its status from reaching create complete until AWS CloudFormation receives a specified number of success signals or the timeout period is exceeded. To signal a resource, you can use the `cfn-signal` helper script or `ScriptResource` API. AWS CloudFormation publishes valid signals to the stack events so that you track the number of signals sent.

The creation policy is invoked only when AWS CloudFormation creates the associated resource. Currently, the only AWS CloudFormation resources that support creation policies `AWS::AutoScaling::AutoScalingGroup`, `AWS::EC2::Instance`, `AWS::CloudFormation::WaitCondition`.

Use the `CreationPolicy` attribute when you want to wait on resource configuration actions before stack creation proceeds. For example, if you install and configure software applications on an EC2 instance, you might want those applications to be running before proceeding. In such cases, you can add a `CreationPolicy` attribute to the instance, and then send a success signal to the instance after the applications are installed and configured.
```
CreationPolicy:
  AutoScalingCreationPolicy:
    MinSuccessfulInstancesPercent: Integer
  ResourceSignal:
    Count: Integer
    Timeout: String
```

The `Timeout` property is the length of time that AWS CloudFormation waits for the number of signals that were specified in the `Count` property. The timeout period starts after AWS CloudFormation starts creating the resource, and the timeout expires no sooner than the time you specify but can occur shortly thereafter. The maximum time that you can specify is 12 hours.

---
**Question**: A SysOps Administrator has been instructed to handle the deployment of the cloud resources in a single AWS account using CloudFormation. The Administrator must develop a unified template that can be reused for multiple environments instead of manually copying and pasting the same configurations into the template. The dedicated template will be used and referenced from within other templates in the same AWS Region. If the template has been updated, any stack that is referencing it will automatically use the updated configuration.

How can the Administrator meet this requirement?

**Answer**: Use Nested Stacks.

Nested stacks are stacks created as part of other stacks. You can create a nested stack within another stack by using the `AWS::CloudFormation::Stack` resource.

As your infrastructure grows, common patterns can emerge in which you declare the same components in multiple templates. You can separate out these commons components and create dedicated templates for them. Then use the resource in your template to reference other templates, creating nested stacks.

For example, assume that you have a load balancer configuration that you use for most of your stacks. Instead of copying and pasting the same configurations into your templates, you can create a dedicated template for the load balancer. Then, you just use the `AWS::CloudFormation::Stack` resource to reference that template from within other templates.

If the load balancer template is updated, any stack that is referencing it will use the updated load balancer (only after you update the stack). In addition to simplifying updates, this approach lets you use experts to create and maintain components that you might not necessarily be familiar with. All you need to do is reference their templates.

StackSets would be incorrect here because a stack set simply lets you create stacks in AWS accounts across regions by using a single AWS CloudFormation template. The scenario mentioned that the cloud resources are in a single AWS account and also, the dedicated template will be referenced from within other templates in the same AWS Region. For this kind of situation, using a nested stack is more suitable than StackSets.

ChangeSets would also be incorrect because a ChangeSet is primarily used to preview how the proposed changes to a stack might impact your running resources in AWS.

StackPolicies is also incorrect because a StackPolicy is commonly used to prevent stack resources from being unintentionally updated or deleted during a stack update. A stack policy is a JSON document that defines the update actions that can performed on designated resources.