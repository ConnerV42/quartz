---
title: "Spring Boot"
tags:
- software-engineering
- java
disableToc: false
---

- [Links to each Spring project](https://spring.io/projects)

## Spring Data JPA
- [JPQL Docs - Apache](https://openjpa.apache.org/builds/1.0.1/apache-openjpa-1.0.1/docs/manual/jpa_overview_query.html)
- [Spring Data JPA Official Docs](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- Sorting With Spring Data
	- [Baeldung: Sorting Query Results with Spring Data](https://www.baeldung.com/spring-data-sorting)
	- [Baeldung: Pagination and Sorting](https://www.baeldung.com/spring-data-jpa-pagination-sorting)
	- [Dynamic sorting with the Spring Data Sort object](https://attacomsian.com/blog/spring-data-jpa-sorting): A replacement for the `ORDER BY` clause used in classic SQL
- Implementation Guides and Tutorials
	- [Baeldung: @Query](https://www.baeldung.com/spring-data-jpa-query)
	- [Medium: Pagination example queries in JPQL](https://medium.com/@sindepal/spring-data-jpa-query-and-pageable-15f8c3e7fe4e)
	- [Baeldung: Transaction Management](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring)
	- [Wildcard Queries](https://www.amitph.com/spring-data-jpa-wildcard-query/)
	- [Baeldung: Setting up MetaModel](https://www.baeldung.com/hibernate-criteria-queries-metamodel)
	- [JPA Specifications - Interactive Tutorial](https://www.logicbig.com/tutorials/spring-framework/spring-data/combined-specifications.html)
	- [JPA Specifications - Interactive Tutorial](https://www.logicbig.com/tutorials/spring-framework/spring-data/combined-specifications.html)
	- [Hibernate docs - JPA MetaModel](https://docs.jboss.org/hibernate/jpamodelgen/1.0/reference/en-US/html_single/#whatisit)
- Helpful Stack Overflow Posts
	- [Difference Between Criteria, Predicate and Specification](https://stackoverflow.com/questions/47469861/what-is-the-difference-between-a-criteria-a-predicate-and-a-specification)
	- [Criteria API vs QueryDSL vs JPA MetaModel](https://stackoverflow.com/questions/53325506/criteria-api-vs-querydsl-vs-jpa-metamodel)
	- [JPA Specifications](https://stackoverflow.com/questions/48647847/jpa-specifications-by-example)


## Spring Boot Actuator Management Endpoints
- [Spring Boot Actuator Endpoints Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#actuator.endpoints)
- [A useful tutorial for Spring Boot Actuator](https://howtodoinjava.com/spring-boot/actuator-endpoints-example/)

## jEnv
- [For easily managing Java versions across projects](https://www.jenv.be/)

## Custom Banner in Spring Boot
In short, just create a `/src/main/resources/banner.txt` and/or `/src/main/resources/banner.gif`  
- [Spring Banner Tutorial](https://springhow.com/spring-boot-startup-banner/)
- You can even have different banners per environment, by adding the following to your application-\*.yml
	- Having custom banners per env makes it easy to know what you are running.
```yml
spring:
  banner:
    location: banner-crt.txt
```