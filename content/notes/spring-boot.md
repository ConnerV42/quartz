---
title: "Spring Boot"
tags:
- software-engineering
- java
disableToc: false
---

### Custom Banner in Spring Boot
In short, just create a `/src/main/resources/banner.txt` and/or `/src/main/resources/banner.gif`  
- [Spring Banner Tutorial](https://springhow.com/spring-boot-startup-banner/)
- You can even have different banners per environment, by adding the following to your application-\*.yml
	- Having custom banners per env makes it easy to know what you are running.
```yml
spring:
  banner:
    location: banner-crt.txt
```