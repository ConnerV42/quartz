---
title: "Intellij"
tags:
- software-engineering
disableToc: true
---

- Open/Close Terminal
```
option + fn + f12
```
 
- Open/Close Project Explorer
```
cmd + 1
```

- If you'd like to view files in an open Intellij project across multiple monitors, simply drag a file off screen onto another monitor screen to initiate that behavior.

- Views -> Tool Windows -> Database

- In a running debugger, you can add a new Watcher, with the following command, to allow you to pretty print your object as JSON. Good for generating test data for Unit Tests when working in IntelliJ

```java
new ObjectMapper()
	.setSerializationInclusion(Include.NON_NULL)
	.writerWithDefaultPrettyPrinter()
	.writeValueAsString(myObj)
```

##### Intellij Unable to Find Imports
- Try File -> Invalidate Caches

##### Force Maven Dependencies Re-install
If you're unable to find maven dependencies that you know are installed, try the following:

1. Close out of IntelliJ
2. Delete the .idea directory at the root of your project.
3. Reopen IntelliJ