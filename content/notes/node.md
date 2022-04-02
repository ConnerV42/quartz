---
title: "Node"
tags:
- software-engineering
- node
- javascript
- npm
disableToc: false
---

### Writing an array of JSON to a file from an API in Node
- Requires axios
```javascript
const axios = require('axios');
const { writeFile } = require('fs');
const API_BASE_URL = 'https://my-api.com';
const PROFILE_ENDPOINT = `${API_BASE_URL}/v1/profile`;

let failedUsers = 0;

const fileOptions = {
    flag: 'a'
};

const writeUser = (response) => {
    const { data: { profile: { email, password } } } = response;

    const obj = {
        email,
        password
    };

    writeFile(outputFileName, JSON.stringify(obj, null, 2) + ',' + '\n', fileOptions, err => err ? console.error(err) : '');
};

const logError = (err) => {
    if (err) {
        console.error(err);
        failedUsers++;
    }
}

const seedUsers = async () => {
    console.log(`Creating ${desiredProfileCount} profiles and writing login credentials to ${outputFileName}`);
    const promises = [];

    const requestBody = {
        serviceData: [
            "CREDIT_CARD"
        ]
    };

    for (let i = 0; i < desiredProfileCount; i++) {
        const promise = axios.post(PROFILE_ENDPOINT, requestBody)
            .then(writeUser)
            .catch(logError);

        promises.push(promise);
    }

    await Promise.all(promises);

    console.log(`\nUsers have been added. ${failedUsers} request(s) failed.`);
}

const validateInput = (args) => {
    let result = true;

    if (args[0] === undefined || isNaN(args[0])) {
        console.log("ERROR: 1st argument is not a number or is missing, please pass desired number of profiles");
        result = false;
    }

    if (args[1] === undefined) {
        console.log("ERROR: 2nd argument missing, please pass an output file name.");
        result = false;
    }

    if (!result) {
        console.log("Example Usage:   \"node asyncCreateUsers.js 10 output.json\"");
        console.log("The above example writes 10 profiles to output.json\n");
    }

    return result;
};

const args = process.argv.slice(2);
const validInput = validateInput(args);

const desiredProfileCount = args[0];
const outputFileName = args[1];

if (validInput) {
    seedUsers();
}

```

### `n` – Interactively Manage Your NodeJS Versions
[**n**](https://github.com/tj/n), is an extremely simple Node version manager that can be installed via npm.

Say you want Node.js v0.10.x to build [Atom](https://github.com/atom/atom).

```bash
npm install -g n   # Install n globally
n 0.10.33          # Install and use v0.10.33
```

```bash
Usage:
n                            # Output versions installed
n latest                     # Install or activate the latest node release
n stable                     # Install or activate the latest stable node release
n <version>                  # Install node <version>
n use <version> [args ...]   # Execute node <version> with [args ...]
n bin <version>              # Output bin path for <version>
n rm <version ...>           # Remove the given version(s)
n --latest                   # Output the latest node version available
n --stable                   # Output the latest stable node version available
n ls                         # Output the versions of node available
```
