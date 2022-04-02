---
title: "jq - json parsing cli tool"
tags:
- software-engineering
- json
- clitools
disableToc: false
---

##### Modifying a key-value in a JSON file using jq in-place

```bash
contents="$(jq '.name = "newValue"' package.json)" && \
echo "${contents}" > package.json
```

##### Bash Script that utilizes jq to alter package.json to prep it for GitLab Registry NPM Package Upload.
```bash
#!/bin/bash  
  
# Update package.json "name" property, which is required  
# to include the GitLab Registry scope "@scope"  
# and add a "publicConfig" object property that contains  
# a reference to the scope registry that the sdk will be  
# uploaded to.  
  
PACKAGE_DIR="app"  
PACKAGE_JSON_FILE_NAME="package.json"  
GITLAB_REGISTRY_LOCATION="https://gitlab.com/api/v4/projects/$PROJECT_ID/packages/npm/"  
  
cd ../$PACKAGE_DIR  
  
# Prepend GitLab Registry scope to .name property in package.json  
CONTENTS="$(jq '.name = "@scope/" + .name' $PACKAGE_JSON_FILE_NAME)" && \  
echo "${CONTENTS}" > temp1.json  
  
PUBLISH_CONFIG=$(echo '{ "publishConfig": { "@scope:registry": "https://gitlab.com/api/v4/projects/$PROJECT_ID/packages/npm/"} }' | jq .)  
echo "${PUBLISH_CONFIG}" > temp2.json  
  
# Performs a jq slurp merge, merging the altered contents of the package.json  
# with the publishConfig object  
OUTPUT="$(jq -s 'add' temp1.json temp2.json)"  
  
# Output new package.json for GitLab Registry upload  
echo "${OUTPUT}" > package.json  
  
# clean up temporary files used in jq slurp merge  
rm temp1.json temp2.json
```