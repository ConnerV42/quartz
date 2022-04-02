---
title: "Maven Wiki"
tags:
- software-engineering
- java
disableToc: false
---
## Maven Resources
- [Apache Maven Docs: Maven CLI Options Reference](https://maven.apache.org/ref/3.8.1/maven-embedder/cli.html)
- [Apache Maven Docs: Maven CI Friendly Versions](https://maven.apache.org/maven-ci-friendly.html)
- [Apache Maven Docs: Maven Plugins](https://maven.apache.org/plugins/index.html)
	- [Difference b/t Common Maven Plugins](https://stackoverflow.com/questions/38548271/difference-between-maven-plugins-assembly-plugins-jar-plugins-shaded-plugi)
## Maven Build Lifecycle
- [Quick Summary of Maven Build Lifecycle from the Apache Website](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html#maven-phases)
### Clean
- `clean` is it's own build lifecycle phase (which can be thought of as an action or task) in Maven. `mvn clean install` tells Maven to do the `clean` phase in each module before running the `install` phase for each module.

- What this does is clear any compiled files you have, making sure that you're really compiling each module from scratch.

- Once we have the database populated in each environment, we should run a build and test stage.
### Verify
`mvn verify` performs any integration tests that maven finds in the project.
### Install 
- `mvn install` – install the package into the local repository, for use as a dependency in other projects locally.
- `mvn install` implicitly runs `mvn verify` and then copies the resulting artifact into your local maven repository which you usually can find under `C:\Users\username\.m2\repository` if you are using windows.
### Package
- `mvn package` – take the compiled code and package it in its distributable format, such as a JAR, WAR, or Docker image.
- Both of `mvn install` and `mvn package` will compile your code, clean the `/target` folder, and place a new packaged JAR or WAR into that /target folder. The main difference: `mvn install` will also install the package into your local maven repository, for use as a dependency in other projects locally.
## Managing Parent POM and Child POMs
To match a parent POM, Maven uses two rules:
1. There is a pom file in project’s root directory or in given relative path.
2. Reference from child POM file contains the same coordinates as stated in the parent POM file.

Maven parent pom can contain almost everything and those can be inherited into child pom files e.g

- Common data – Developers’ names, SCM address, distribution management etc.
- Constants – Such as version numbers
- Common dependencies – Common to all child. It has same effect as writing them several times in individual pom files.
- Properties – For example plugins, declarations, executions and IDs.
- Configurations
- Resources
### Example Parent POM
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsdr">
 <modelVersion>4.0.0</modelVersion>
					 
 <groupId>com.howtodoinjava.demo</groupId>
 <artifactId>MavenExamples</artifactId>
 <version>0.0.1-SNAPSHOT</version>
 <packaging>pom</packaging>
					 
 <name>MavenExamples Parent</name>
 <url>http://maven.apache.org/</url>
					 
 <properties>
	 <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	 <junit.version>3.8.1</junit.version>
	 <spring.version>4.3.5.RELEASE</spring.version>
 </properties>
					 
 <dependencies>
					 
 <dependency>
	 <groupId>junit</groupId>
	 <artifactId>junit</artifactId>
	 <version>${junit.version}</version>
	 <scope>test</scope>
 </dependency>
					 
 <dependency>
	 <groupId>org.springframework</groupId>
	 <artifactId>spring-core</artifactId>
	 <version>${spring.version}</version>
 </dependency>
					 
 </dependencies>
</project>
```
### Example Child POM
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsdr">
 
<!--The identifier of the parent POM-->
 <parent>
	 <groupId>com.howtodoinjava.demo</groupId>
	 <artifactId>MavenExamples</artifactId>
	 <version>0.0.1-SNAPSHOT</version>
</parent>
	
 <modelVersion>4.0.0</modelVersion>
 <artifactId>MavenExamples</artifactId>
 <name>MavenExamples Child POM</name>
 <packaging>jar</packaging>
					 
 <dependencies>
	 <dependency>
		 <groupId>org.springframework</groupId>
		 <artifactId>spring-security</artifactId>
		 <version>${spring.version}</version>
	 </dependency>
 </dependencies>
</project>
```
## Maven Snapshots
A snapshot version in Maven is one that has not been released.

The idea is that **before** a `1.0` release (or any other release) is done, there exists a `1.0-SNAPSHOT`. That version is what _might become_ `1.0`. It's basically "`1.0` under development".

The difference between a "real" version and a snapshot version is that snapshots might get updates. That means that downloading `1.0-SNAPSHOT` today might give a different file than downloading it yesterday or tomorrow.

Usually, snapshot dependencies should **only** exist during development and no released version (i.e. no non-snapshot) should have a dependency on a snapshot version.

The snapshot is _not_ necessarily more stable: it is just the latest build. The snapshot _precedes_ the actual release, it does not come after it.