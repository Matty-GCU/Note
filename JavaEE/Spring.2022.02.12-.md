# Spring5学习笔记

## 零. 前言

**基于教程**

[【狂神说Java】Spring5最新完整教程IDEA版通俗易懂_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1WE411d7Dv?spm_id_from=333.999.0.0)（发布时间：2019-10-13）

**参考教程**

[Spring 5.X系列教程:满足你对Spring5的一切想象-持续更新 - flydean - 博客园](https://www.cnblogs.com/flydean/p/spring5.html)（发布时间：2020-05-20）

**本机环境版本**

* *Spring Framework 5.3.15*

* JDK 11.0.3

**参考资料**

[Spring Framework](https://spring.io/projects/spring-framework#learn)

* [Spring Framework Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/)
  * [Core Technologies](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core)

[Newest 'spring+or+spring-mvc+or+spring-aop' Questions - Stack Overflow](https://stackoverflow.com/questions/tagged/spring+or+spring-mvc+or+spring-aop)

[Spring | broken's blog](https://guopeixiong.github.io/2021/10/21/Spring/)（基于同一教程的学习笔记，值得参考）

## 一. 简介

### 1.1 spring是什么

**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架。**

> # [Spring Framework Overview](https://docs.spring.io/spring-framework/docs/current/reference/html/overview.html#overview)
>
> Version 5.3.15
>
> <u>Spring makes it easy to create Java enterprise applications.</u> It provides everything you need to embrace the Java language in an enterprise environment, with support for Groovy and Kotlin as alternative languages on the JVM, and with the flexibility to create many kinds of architectures depending on an application’s needs. ...
>
> <u>Spring supports a wide range of application scenarios.</u> ...
>
> <u>Spring is open source. It has a large and active community</u> that provides continuous feedback based on a diverse range of real-world use cases. This has helped Spring to successfully evolve over a very long time.
>
> ## 1. What We Mean by "Spring"
>
> The term "Spring" means different things in different contexts. <u>It can be used to refer to the Spring Framework project itself, which is where it all started. Over time, other Spring projects have been built on top of the Spring Framework.</u> Most often, when people say "Spring", they mean the entire family of projects. <u>This reference documentation focuses on the foundation: the Spring Framework itself.</u>
>
> The Spring Framework is divided into modules. Applications can choose which modules they need. <u>At the heart are the modules of the core container</u>, including a configuration model and a dependency injection mechanism. <u>Beyond that, the Spring Framework provides foundational support for different application architectures,</u> including messaging, transactional data and persistence, and web. It also includes the Servlet-based Spring MVC web framework and, in parallel, the Spring WebFlux reactive web framework.
>
> ...
>
> ## 2. History of Spring and the Spring Framework
>
> <u>Spring came into being in 2003 as a response to the complexity of the early [J2EE](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition) specifications.</u> While some consider Java EE and Spring to be in competition, Spring is, in fact, complementary to Java EE. The Spring programming model does not embrace the Java EE platform specification; rather, it integrates with carefully selected individual specifications from the EE umbrella:
>
> - Servlet API ([JSR 340](https://jcp.org/en/jsr/detail?id=340))
> - WebSocket API ([JSR 356](https://www.jcp.org/en/jsr/detail?id=356))
> - Concurrency Utilities ([JSR 236](https://www.jcp.org/en/jsr/detail?id=236))
> - JSON Binding API ([JSR 367](https://jcp.org/en/jsr/detail?id=367))
> - Bean Validation ([JSR 303](https://jcp.org/en/jsr/detail?id=303))
> - JPA ([JSR 338](https://jcp.org/en/jsr/detail?id=338))
> - JMS ([JSR 914](https://jcp.org/en/jsr/detail?id=914))
> - as well as JTA/JCA setups for transaction coordination, if necessary.
>
> The Spring Framework also supports the Dependency Injection ([JSR 330](https://www.jcp.org/en/jsr/detail?id=330)) and Common Annotations ([JSR 250](https://jcp.org/en/jsr/detail?id=250)) specifications, which application developers may choose to use instead of the Spring-specific mechanisms provided by the Spring Framework.
>
> ...
>
> ## 3. Design Philosophy
>
> When you learn about a framework, it’s important to know not only what it does but what principles it follows. Here are the guiding principles of the Spring Framework:
>
> - <u>Provide choice at every level.</u> Spring lets you defer design decisions as late as possible. For example, you can switch persistence providers through configuration without changing your code. The same is true for many other infrastructure concerns and integration with third-party APIs.
> - <u>Accommodate diverse perspectives.</u> Spring embraces flexibility and is not opinionated about how things should be done. It supports a wide range of application needs with different perspectives.
> - <u>Maintain strong backward compatibility.</u> Spring’s evolution has been carefully managed to force few breaking changes between versions. Spring supports a carefully chosen range of JDK versions and third-party libraries to facilitate maintenance of applications and libraries that depend on Spring.
> - <u>Care about API design.</u> The Spring team puts a lot of thought and time into making APIs that are intuitive and that hold up across many versions and many years.
> - <u>Set high standards for code quality.</u> The Spring Framework puts a strong emphasis on meaningful, current, and accurate javadoc. It is one of very few projects that can claim clean code structure with no circular dependencies between packages.
>
> ## 4. Feedback and Contributions
>
> <u>For how-to questions or diagnosing or debugging issues, we suggest using Stack Overflow.</u> Click [here](https://stackoverflow.com/questions/tagged/spring+or+spring-mvc+or+spring-aop+or+spring-jdbc+or+spring-r2dbc+or+spring-transactions+or+spring-annotations+or+spring-jms+or+spring-el+or+spring-test+or+spring+or+spring-remoting+or+spring-orm+or+spring-jmx+or+spring-cache+or+spring-webflux+or+spring-rsocket?tab=Newest) for a list of the suggested tags to use on Stack Overflow. If you’re fairly certain that there is a problem in the Spring Framework or would like to suggest a feature, please use the [GitHub Issues](https://github.com/spring-projects/spring-framework/issues).
>
> ...
>
> ## 5. Getting Started
>
> If you are just getting started with Spring, you may want to begin using the Spring Framework by creating a [Spring Boot](https://projects.spring.io/spring-boot/)-based application. Spring Boot provides a quick (and opinionated) way to create a production-ready Spring-based application. It is based on the Spring Framework, favors convention over configuration, and is designed to get you up and running as quickly as possible.
>
> <u>You can use [start.spring.io](https://start.spring.io/) to generate a basic project</u> or follow one of the ["Getting Started" guides](https://spring.io/guides), such as [Getting Started Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/). As well as being easier to digest, these guides are very task focused, and most of them are based on Spring Boot. They also cover other projects from the Spring portfolio that you might want to consider when solving a particular problem.

### 1.2 spring的优点------

* 开源、免费
* 轻量级、非侵入式
* 控制反转、面向切面
* 支持事务的处理
* 支持几乎所有Java框架的整合（“spring是个大杂烩”）

## 二. 2

### 2.1 导入相关依赖

```xml
<!-- springframework的一个比较顶层的模块 -->
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.15</version>
</dependency>
<!-- spring和mybatis整合要用到 -->
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.15</version>
</dependency>
```

<img src="Spring.2022.02.12-/spring-webmvc模块的依赖.png" alt="spring-webmvc模块的依赖" style="zoom: 67%;" />

<img src="Spring.2022.02.12-/spring-jdbc模块的依赖.png" alt="spring-jdbc模块的依赖" style="zoom:67%;" />

1

1

1



<img src="Spring.2022.02.12-/spring5模块图.png" alt="spring5模块图" style="zoom: 50%;" />