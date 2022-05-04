---
title: "Terraform Wiki"
tags:
- software-engineering
- infrastructure-as-code
disableToc: false
---
## Terraform Resources
- [Resource Dependencies](https://www.terraform.io/language/resources/behavior#resource-dependencies)
- [Terraform Workspaces](https://www.terraform.io/language/state/workspaces#workspaces)
> ### When to use Multiple Workspaces
> Named workspaces allow conveniently switching between multiple instances of a _single_ configuration within its _single_ backend. They are convenient in a number of situations, but cannot solve all problems.
> A common use for multiple workspaces is to create a parallel, distinct copy of a set of infrastructure in order to test a set of changes before modifying the main production infrastructure. For example, a developer working on a complex set of infrastructure changes might create a new temporary workspace in order to freely experiment with changes without affecting the default workspace.
## Automation
- [Automating Terraform](https://learn.hashicorp.com/tutorials/terraform/automate-terraform)
> ### Pass `terraform plan` output to `terraform apply` in CI
> When running in an orchestration tool, it can be difficult or impossible to ensure that the `plan` and `apply` subcommands are run on the same machine, in the same directory, with all of the same files present.
## Terraform Commands
- `terraform refresh` is effectively an alias for `terraform apply -refresh-only -auto-approve` which is why it should _NEVER_ be used. It is far too risky to run `terraform refresh` without first reviewing the proposed state changes.
- Instead, use `terraform apply -refresh-only`. This alternative command will present an interactive prompt for you to review and confirm the proposed state changes. After confirming, your Terraform remote state will be updated to match the settings of your managed remote objects. More info can be found about this in the Hashicorp documentation [here](https://www.terraform.io/cli/commands/refresh#command-refresh).
- `terraform init`
	- The `terraform init` command is used to initialize a working directory containing Terraform configuration files. This is the first command that should be run after writing a new Terraform configuration or cloning an existing one from version control. It is safe to run this command multiple times.
- Terraform prints output values to the screen when a configuration is applied, but Terraform can also query all output values with the `terraform output` command, or selectively query certain output values using the below command template:
```
terraform output $RESOURCE_NAME
```
## Terraform Tips and Tricks
- You can use `-target` to plan/apply a [specific Terraform resource](https://devops.stackexchange.com/questions/4292/terraform-apply-only-one-tf-file) within a module. **Be careful**, this is an advanced way to use Terraform and could break your infrastructure if done improperly.
```
terraform apply -target=aws_security_group.my_sg
```
- Turn on Verbose Logging by setting the TF_LOG environment variable to 1. 0 turns off logging.
```
export TF_LOG=1
```
- `tfenv` is a useful cli tool that can be used to easily switch between different versions of Terraform.
```
brew install tfenv
tfenv install 1.0.0
tfenv use 1.0.0
terraform version # verify you're using the right version
```