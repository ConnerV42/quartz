### Related Notes
- [AWS Wiki](/notes/aws/aws-wiki.md)

## Amazon Inspector
- Scans EC2 instance & the instance OS
- Also containers
- Detects vulnerabilities and deviations against best practice
- Length: 15 minutes, 1 hour, 8/12 hours or 1 day
- Provides a report of findings ordered by priority
- Network Assessment (no agent required)
- Network & Host Assessment (requires agent)
	- Host Assessment Rules Packages (Important for exam):
		- Common vulnerabilities and exposures (CVE)
			- List of CVE ids that are generated as part of a report for an instance or container
		- Center for Internet Security (CIS) Benchmarks
			- Best practices from CIS test against instance or container
		- Security best practices for Amazon Inspector
- Network Reachability (no agent required)
	- Check readability end to end of EC2, ALB, DX, ELB, ENI, IGW, ACLs, RT's, SG's, Subnets, VPCs, VGWs & VPC Peering 
	- Network Reachability Rules Packages (Important for exam):
		- `RecognizedPortWithListener`
		- `RecognizedPortNoListener`
		- `RecognizedPortNoAgent`
		- `UnrecognizedPortWithListener`
- With Inspector, rules packages determine what is checked 
- Agent can provide additional OS visibility, such as 


Amazon Inspector is a vulnerability management service that continuously scans your AWS Workloads for vulnerabilities. Amazon Inspector automatically discovers and scans EC2 instances and container images residing in ECR for software vulnerabilities and unintended network exposure.

When a software vulnerability or network issue is discovered, Amazon Inspector creates a finding. A finding describes the vulnerability, identifies the affected resource, rates the severity of the vulnerability, and provides remediation guidance. Details of a finding for your account can be analyzed in multiple ways using the Amazon Inspector console, or you can view and process your findings through other AWS services. For more information, see [Understanding findings in Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/findings-understanding.html).