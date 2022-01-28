# MyBatis学习笔记

## 前言

**基于教程**

[【狂神说Java】Mybatis最新完整教程IDEA版通俗易懂_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1NE411Q7Nx?spm_id_from=333.999.0.0)

> 发布时间：2019-10-02
>
> 视频演示环境：
>
> * MyBatis 9.28
> * JDK 1.8
> * MySql 5.7(or 8.0)
> * Maven 3.6.1(or 3.6.0)
> * IDEA

**本机环境**

* **MyBatis ????**
* JDK 11
* MySql 8.0
* Maven 3.8.4
* IDEA

**知识背景**

> 以下内容达到能够简单使用的程度就可以学习MyBatis

* Java基础
* JDBC

* MySQL
* Maven
* Junit

**参考资料**

官方中文文档：[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)（可切换回英文原版）

> 任何老师讲MyBatis，都不可能比官方文档更详细。

broken的笔记：[MyBatis | broken's blog](https://guopeixiong.github.io/2021/10/19/MyBatis/)

## 一. 简介

### 1.1 什么是MyBatis

![mybatis-logo](https://mybatis.org/images/mybatis-logo.png)





* MyBatis 是一款优秀的持久层框架，

* 它支持自定义 SQL、存储过程以及高级映射。
* MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
* MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
* MyBatis本是apache的一个开源项目iBatis，2010年这个项目由apache software foundation迁移到了google code，并且改名为MyBatis。2013年11月迁移到Github。