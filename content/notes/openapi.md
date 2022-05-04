---
title: "OpenAPI"
tags:
- software-engineering
disableToc: false
---

### Why use OpenAPI?
As a tool, OpenAPI allows you to generate client SDKs for your API across dozens of supported languages. Building SDKs are an integral part of the developer experience because they: 
- Save huge amounts of development time
- Dramatically reduce "time to first API call"
- Embed optimization & best practices into API usage

### Generate Node SDK with OpenAPI and Publish to GitLab Registry
This documentation was developed using this GitLab [documentation](https://docs.gitlab.com/ee/user/packages/npm_registry/#use-the-gitlab-endpoint-for-npm-packages)

1. Create a Temporary Project Access Token with full permissions for testing the SDK upload to Gitlab Registry locally
2. Generate the SDK
```bash
mkdir javascript-sdk
cd javascript-sdk
export JS_POST_PROCESS_FILE="/usr/local/bin/js-beautify -r -f"
openapi-generator generate -i $SWAGGER_URL -g javascript -o .
```
3. Install packages and build
```bash
npm install
```
4. Ensure secure communication for private registries
```bash
npm config set always-auth true
```
5. Deploying to GitLab Registry
	1. For local GitLab Registry uploads, we need a personal access token, with full registry write permissions.
```bash
npm config set @your_scope/npm_package_name:registry https://gitlab.com/api/v4/projects/$PROJECT_ID/packages/npm/
```
```bash
npm config set -- '//gitlab.com/api/v4/projects/$PROJECT_ID/packages/npm/:_authToken' $AUTH_TOKEN
```
6. Publish to GitLab Registry
```bash
npm publish
```

### Publishing an npm package to Gitlab locally
1. Add the following to your generated SDK's package.json file
```json
{  
  "name": "@scope/package-name"
}	
```
```json
 "publishConfig": {  
    "@scope:registry": "$GITLAB_API_V4/projects/$PROJECT_ID/packages/npm/"
 } 
```
2. Add the following to your `.npmrc` file
```bash
//gitlab.com/api/v4/projects/$PROJECT_ID/packages/npm/:_authToken=$PERSONAL_ACCESS_TOKEN
//gitlab.com/api/v4/packages/npm/:_authToken=$PERSONAL_ACCESS_TOKEN
@scope:registry=https://gitlab.com/api/v4/packages/npm/
```