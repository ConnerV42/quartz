---
title: "Bash"
tags:
- software-engineering
- bash
- linux
- shell
- scripting
disableToc: false
---

## Helpful Links
- [Bash Hacker's Wiki](https://wiki.bash-hackers.org/)
- [Wooledge's Wiki](https://mywiki.wooledge.org/BashFAQ)
- [Shell Script Best Practices, from a decade of scripting things](https://sharats.me/posts/shell-script-best-practices/)

## Infinite Loop
Because sometimes it's just necessary to have an infinite loop.

```bash
#!/bin/bash

while true
do
	echo "Running some command"
	sleep 30 # sleep for 30 seconds
done
```

## Auto-complete Shell script name via Terminal
1. Write your script. This is a random example script that just performs some basic input validation, and then runs a jar.
```bash
#!/bin/sh

arg1=$1
arg2=$2

## Directory where jar file is located    
dir=/directory-path/to/jar-file/

## Jar file name
jar_name=app.jar

## Permform some validation on input arguments, one example below
if [ -z "$1" ] || [ -z "$2" ]; then
        echo "Missing arguments, exiting.."
        echo "Usage : $0 arg1 arg2"
        exit 1
fi

java -jar $dir/$jar_name arg1 arg2
```
2. Copy your Bash script to the `/usr/local/bin` directory.
```
cp run.sh /usr/local/bin
```
3. Give execute permission to the script.
```
chmod u+x /usr/local/bin/test.sh
```
4. Now you can type just the word `run` or `run.sh` on command line, followed by any arguments necessary to run your script. The shell will auto-complete the script name and allow execution by pressing the enter key.

## Pipefail
[Set -euxo pipefail](http://blog.kablamo.org/2015/11/08/bash-tricks-eux/)

## Bash script
Bash script for quickly generating AWS session credentials for a role in a spring boot application.properties file for local development.

```bash
#!/bin/bash  
  
# Create an application-OVERRIDES.properties file in 
# environment/src/main/resources, then this script will
# load credentials for AWS SDK usage in your environment.
#
# Usage: run `./setAWSProperties.sh` from scripts or root app directory  
  
AMAZON_ACCESS_KEY_PROPERTY_NAME="amazon.access.key="  
AMAZON_SECRET_KEY_PROPERTY_NAME="amazon.access.secretkey="  
AMAZON_SESSION_TOKEN_PROPERTY_NAME="amazon.access.sessiontoken="  
  
write_aws_credentials () {  
 # get vault creds as json  
 JSON=$(aws-vault exec <YOUR_AWS_ROLE_NAME> --json)  
 ACCESS_KEY_ID=$(echo "$JSON" | jq -r .AccessKeyId)  
 SECRET_ACCESS_KEY=$(echo "$JSON" | jq -r .SecretAccessKey)  
 SESSION_TOKEN=$(echo "$JSON" | jq -r .SessionToken)  
  
 # write a newline so the credentials are not appended onto an existing property  
 echo "" >> "$1"  
  
 # find and delete existing aws session credentials  
 sed -i -e "/$AMAZON_ACCESS_KEY_PROPERTY_NAME/d" "$1"  
 sed -i -e "/$AMAZON_SECRET_KEY_PROPERTY_NAME/d" "$1"  
 sed -i -e "/$AMAZON_SESSION_TOKEN_PROPERTY_NAME/d" "$1"  
  
 # set new aws sessions credentials  
 echo "$AMAZON_ACCESS_KEY_PROPERTY_NAME;$ACCESS_KEY_ID" >> "$1"  
 echo "$AMAZON_SECRET_KEY_PROPERTY_NAME$SECRET_ACCESS_KEY" >> "$1"  
 echo "$AMAZON_SESSION_TOKEN_PROPERTY_NAME$SESSION_TOKEN" >> "$1"  
 echo "wrote aws session credentials to application.OVERRIDES.properties"
}  
  
OVERRIDES_FILE_DIR="environment/src/main/resources/application-OVERRIDES.properties"
  
if [ -e "$OVERRIDES_FILE_DIR" ]; then  
 write_aws_credentials $OVERRIDES_FILE_DIR  
elif [ -e "../$OVERRIDES_FILE_DIR" ]; then  
 write_aws_credentials "../$OVERRIDES_FILE_DIR"  
else  
 echo "application-OVERRIDES.properties does not exist. Create it and re-run the script."  
fi
```

## Print scripting language
```
echo $0
```