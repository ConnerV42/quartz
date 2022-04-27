---
title: "Curl"
tags:
- software-engineering
- linux
- shell
- scripting
disableToc: false
---

## curl resources
- [Curl Man Page](https://curl.se/docs/manpage.html)
- [Curl Command Cheat Sheet](https://reqbin.com/req/c-kdnocjul/curl-commands)

## useful curl commands
- Pipe curl command output to `json_pp` to pretty print json response
```
$CURL_COMMAND | json_pp
```
- Curl Timeouts
  - `curl --max-time <seconds>` or `curl -m <seconds>`
- There is no difference between `-v`, `-vv`, and `-vvv` in curl
  - [Explanation via Stack Overflow](https://stackoverflow.com/questions/24402473/what-is-meaning-of-vvv-option-in-curl-request)
```
curl -vvv -sSL -o /dev/null 'https://google.com/'
```