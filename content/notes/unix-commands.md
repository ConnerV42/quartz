---
title: "Unix Commands "
tags:
- software-engineering
- linux
- shell
- scripting
disableToc: false
---

## Cheat Sheets
- [Stanford's Basic Unix Commands](http://mally.stanford.edu/~sr/computing/basic-unix.html)

## Users / Groups
To get the current user in a shell or script, run the below command
```bash
whoami
```
or
```bash
echo "$USER"
```
## SSH
- Template command to remotely run a script and immediately exit, without caring about the result. This is useful for automation scripts that involve running multiple commands on remote servers.
```
ssh -q $SSH_USER@$SERVER "nohup /appl/bin/start.sh start > /dev/null 2>&1 & "
```

## Verify DNS Resolution
```shell
nslookup www.google.com
```

## IP Address
### Public IP Address of local machine 
```shell
curl ifconfig.me
```

### Private IP Address of local machine
```shell
ifconfig -a
```

## Grand total size of all the subdirectories in the current directory
- `du -sh -- *`

## Process ID Number
`lsof -p PID` will list all the files that have been touched by the currently running process

## egrep
- Search contents of every file for this matching text:
`find . -type f -exec egrep -lH search_for_me '{}' ';'`

## find command
- Find every file named config.txt in your home directory:
`find ~ -name "config.txt"`

## scp file transfer commands
- [scp command cheat sheet](https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/)
* Copy a file from a local to a remote system:
`scp file.txt ssh_user@hostname:/tmp`

## list/installation/deletion of certs with keytool
- List all certs in a keystore: `keytool -list -keystore <PATH_TO_CACERTS> -storepass changeit -noprompt`
- Add a cert to a keystore: `keytool -import -trustcacerts -keystore <PATH_TO_CACERTS> -storepass changeit -noprompt -alias <ALIAS> -file <PATH_TO_NEW_CERT>`
- Delete cert in a keystore by alias: `keytool -delete -alias <ALIAS> -keystore <PATH_TO_CACERTS>`

## OpenSSL commands
- Verify contents of a cert: `openssl x509 -in <PATH_TO_CERT> -text`
- View certs of a client: `openssl s_client -showcerts -connect google.com:636`

## Change Ownership of Symlink
- `chown -h USER:GROUP jre` if you want to change the ownership of the symlink itself, not the destination directory
 
## What's using this port?
- Handly command on MacOS to check what's taking up a port
```
sudo lsof -i :3306
```