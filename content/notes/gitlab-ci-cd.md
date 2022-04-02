---
title: "Gitlab CI/CD"
disableToc: true
---


## Using `gitlab-runner` to test ci locally
- If on macOS, install gitlab-runner with the below command
```
brew install gitlab-runner
```
- To start service, run the below command
```
brew services start gitlab-runner
```
- To restart the service, run the below command
```
brew services restart gitlab-runner
```
- Official Gitlab Runner Docs: [Install GitLab Runner](https://docs.gitlab.com/runner/install/)
- Create a command
	- The following `.gitlab-ci.yml` file defines a task named `build`:
```yaml
build:
  script:
    - echo "Hello World"
```

- Run the command locally [(_limitations apply!_)](https://docs.gitlab.com/runner/commands/index.html#limitations-of-gitlab-runner-exec)

```yaml
gitlab-runner exec shell build
```

### Consuming an npm package from private GitLab Package Registry

1. Set registry and ensure authentication is configured via project access token. Add the following to your .npmrc at the root directory of your project that will be consuming from the private registry.
```bash
@my_scope:registry=https://gitlab.com/api/v4/projects/$PROJECT_ID/packages/npm/
//gitlab.com/api/v4/packages/npm/:_authToken=$PROJECT_ACCESS_TOKEN
//gitlab.com/api/v4/projects/$PROJECT_ID/packages/npm/:_authToken=$PROJECT_ACCESS_TOKEN
```

2. To install the package, run 
```bash
npm install @my-scope/my-package
```

## Links
- [Npalm Terraform Gitlab Runner](https://github.com/npalm/terraform-aws-gitlab-runner/releases)