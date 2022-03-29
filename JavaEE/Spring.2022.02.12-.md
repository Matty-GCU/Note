# Spring5学习笔记

## 零. 前言

### 基于教程

[【狂神说Java】Spring5最新完整教程IDEA版通俗易懂_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1WE411d7Dv?spm_id_from=333.999.0.0)（发布时间：2019-10-13）

### 本机环境版本

* *Spring Framework 5.3.15*

* JDK 11.0.3

### 参考资料

官方：

[Spring Framework](https://spring.io/projects/spring-framework#learn)

* [Spring Framework Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/)
  * [Core Technologies](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core)

[Newest 'spring+or+spring-mvc+or+spring-aop' Questions - Stack Overflow](https://stackoverflow.com/questions/tagged/spring+or+spring-mvc+or+spring-aop)

[Spring Framework 中文文档 - Spring Framework 5.1.3.RELEASE Reference | Docs4dev](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/)

非官方：

[Spring | broken's blog](https://guopeixiong.github.io/2021/10/21/Spring/)（基于同一教程的学习笔记，值得参考）

[Spring 5.X系列教程:满足你对Spring5的一切想象-持续更新 - flydean - 博客园](https://www.cnblogs.com/flydean/p/spring5.html)

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

### 1.2 spring的优点

* 开源、免费
* 轻量级、非侵入式
* 控制反转、面向切面
* 支持事务的处理
* 支持几乎所有Java框架的整合（“spring是个大杂烩”）

### 1.3 Maven导入Spring相关依赖

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

---

2022.02.12正式开始，看了P1

应该就这一次

### ---

2022.03.24重新开始

此时我已经跟着学校课程快“学完”Spring了，所以接下来的基础/概念部分笔记可能会比较简略。

---

### 1.4 spring七大模块

详细介绍可看：[Spring七大模块详解_压到我腿毛了的博客-CSDN博客_spring框架的七大模块](https://blog.csdn.net/Huang1178387848/article/details/82697242)

我就只摘里面的一张图：

![Spring七大模块图](Spring.2022.02.12-/Spring七大模块图（来源文章）.png)

### 1.5 现代化的Java开发

现代化的Java开发，其实就是Spring开发！

* Spring Boot

  * 快速开发的脚手架

  * 基于Spring Boot可以快速开发单个微服务

  * 约定大于配置

* Spring Cloud

  * 基于Spring Boot实现

<img src="Spring.2022.02.12-/the source of modern java（来源视频）.png" alt="the source of modern java（来源视频）" style="zoom:67%;" />

## 二. 控制反转

* 视频P3-P6

### 2.1 概念

Spring官方文档如是说：

> This chapter covers the Spring Framework implementation of the **Inversion of Control (IoC) principle**. IoC is also known as **dependency injection (DI)**.

也就是说，IoC和DI的概念几乎是等同的。

**在Spring中实现控制反转的是IoC容器(The IoC Container)，其实现方式是依赖注入。**

扩展阅读：

[Spring IoC有什么好处呢？ - Mingqi的回答 - 知乎](https://www.zhihu.com/question/23277575/answer/169698662)

[浅谈控制反转与依赖注入 - 胡小国的文章 - 知乎](https://zhuanlan.zhihu.com/p/33492169)

### 2.2 实践

新建p6包

```java
package p6AndP7;

public class User {
    private String name;
    
    /**
     * 如果在配置文件里没有配置bean的constructor-arg，则必须要有无参构造方法
     */
    public User() {
        System.out.println("调用User的无参构造方法");
    }
    
    /**
     * 如果在配置文件里配置了bean的property，则必须要有对应成员变量的setter方法
     */
    public void setName(String name) {
        this.name = name;
    }
    
    public void showName() {
        System.out.println(name);
    }
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myUser" class="p6.User">
        <property name="name" value="小吴"/>
    </bean>

</beans>
```

```java
package p6AndP7;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestP6 {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("p6AndP7/configP6.xml");
        System.out.println("---在加载配置文件时，IoC容器就已经帮我们new好了Bean（实例化Spring管理的对象）---");
        User user = (User) applicationContext.getBean("myUser");
        user.showName();
    }
}
```

运行结果：

```
调用User的无参构造方法
---在加载配置文件时，IoC容器就已经帮我们new好了Bean（实例化Spring管理的对象）---
小吴

Process finished with exit code 0
```

## 三. Spring配置

* 视频P7

怎么配置Bean以及bean标签怎么用就不说了，很简单。

### 3.1 别名

可以通过两种方式配置Bean的一个或多个别名，通过别名获得的Bean对象引用与通过id获得的完全相同（singleton作用域下）。

```xml
<!-- bean标签的name属性中可以配置多个别名，别名之间可以用空格/逗号/分号分隔 -->
<bean id="myUser" class="User" name="alias3,alias4;alias5 alias6">
    <property name="name" value="小吴"/>
</bean>

<!-- 一个alias标签只能配置一个别名 -->
<alias name="myUser" alias="userAlias1"/>
<alias name="myUser" alias="userAlias2"/>
...
```

### 3.2 import标签

多用于团队开发。可以将多个配置文件导入并合并为一个配置文件。

```xml
<import resource="another-config1.xml"/>
<import resource="another-config2.xml"/>
...
```

## 四. 依赖注入

* 视频P8-P10

### 4.1 搭建测试环境

```java
package p8ToP10;

public class Student {
    //各种不同类型的成员变量（Student对象的“依赖”），如何通过配置文件进行“注入”？
    String name;
    String[] namesOfFriends;
    Teacher favoriteTeacher;
    Teacher[] allTeachers;
    List<String> notebooks;
    Set<String> textbooks;
    Map<String, String> grades;
    String anEmptyString;
    String aNullValue;
    
    //getter setter
    //toString
}
```

```java
package p8ToP10;

public class Teacher {
    String teacherName;
    long teacherId;
        
    //getter setter
    //toString
}
```

### 4.2 实践Setter方式注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="teacherWu" class="p8ToP10.Teacher">
        <property name="teacherName" value="吴老师"/>
        <property name="teacherId" value="136"/>
    </bean>

    <bean id="teacherLin" class="p8ToP10.Teacher">
        <property name="teacherName" value="林老师"/>
        <property name="teacherId" value="134"/>
    </bean>

    <bean id="aStudent" class="p8ToP10.Student">
        <!-- 值类型注入 -->
<!--        <property name="name" value="小吴同学"/>-->
        <property name="name">
            <value>小吴同学</value>
        </property>

        <!-- 值类型的数组注入 -->
        <property name="namesOfFriends">
            <array>
                <value>朋友名字1</value>
                <value>朋友名字2</value>
                <value>朋友名字3</value>
            </array>
        </property>

        <!-- 引用类型注入 -->
<!--        <property name="favoriteTeacher" ref="teacherWu"/>-->
        <property name="favoriteTeacher">
            <ref bean="teacherWu"/>
        </property>

        <!-- 引用类型的数组注入 -->
        <property name="allTeachers">
            <array>
                <ref bean="teacherWu"/>
                <ref bean="teacherLin"/>
            </array>
        </property>

        <!-- List注入（仅演示值类型） -->
        <property name="notebooks">
            <list>
                <value>笔记本111</value>
                <value>笔记本111</value>
                <value>笔记本222</value>
            </list>
        </property>

        <!-- Set注入（仅演示值类型） -->
        <property name="textbooks">
            <set>
                <value>语文课本</value>
                <value>数学课本</value>
                <value>英语课本</value>
            </set>
        </property>

        <!-- Map注入（仅演示值类型） -->
        <property name="grades">
            <map>
                <entry key="语文" value="85"/>
                <entry key="数学" value="80"/>
                <entry key="英语" value="90"/>
            </map>
        </property>

        <!-- 空字符串注入 -->
        <property name="anEmptyString" value=""/>

        <!-- null值注入 -->
        <property name="aNullValue">
            <null/>
        </property>
        
    </bean>
</beans>
```

```java
package p8ToP10;

public class TestP8ToP10 {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("p8ToP10/config.xml");
        //小技巧：可以传一个requiredType参数，这样就不用强转了
        Student aStudent = applicationContext.getBean("aStudent", Student.class);
        System.out.println(aStudent);
    }
}
```

### 4.3 构造器方式注入

就是用在bean标签里用constructor-arg子标签，我已经掌握了，课本里也有，所以这里略过。

**值得注意的是，setter方式注入本质上是调用Bean类的setter方法，所以该类一定要有setter方法。**

**而构造器方式注入，同理，也需要有有参构造方法才能注入成功。**

### 4.4 其他注入方式

使用p（parameter）命名空间或者c（contructor）命名空间进行注入。

用起来很简单的。

> 注意：不能直接使用，需要导入XML约束：
>
> ```xml
> xmlns:p="http://www.springframework.org/schema/p"
> xmlns:c="http://www.springframework.org/schema/c"
> ```

参考官网：

![p-namespace](Spring.2022.02.12-/p-namespace.png)

![c-namespace](Spring.2022.02.12-/c-namespace.png)

## ---

P11-15暂时跳过

---



