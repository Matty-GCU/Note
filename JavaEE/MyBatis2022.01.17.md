# MyBatis学习笔记

## 0. 前言

### 基于

教程：

[【狂神说Java】Mybatis最新完整教程IDEA版通俗易懂_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1NE411Q7Nx?spm_id_from=333.999.0.0)

> 教程使用环境：
>
> * **MyBatis-9.28???**
>
> * JDK 1.8
> * MySql 5.7(or 8.0)
> * maven 3.6.1(or 3.6.0)（3.6.2可能有小问题）
> * IDEA
>
> 知识背景
>
> * JDBC
> * MySQL
> * Java基础
> * **Maven???**
> * **Junit???**

### 参考资料

官方中文文档：[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)

沛雄的学习笔记（基于同一教程）：[MyBatis | broken's blog](https://www.lxzforever.top/2021/10/19/MyBatis/)

## 1. 简介

### 1.1 什么是MyBatis

![mybatis-logo](https://mybatis.org/images/mybatis-logo.png)





* MyBatis 是一款优秀的持久层框架，

* 它支持自定义 SQL、存储过程以及高级映射。
* MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
* MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
* MyBatis本是apache的一个开源项目iBatis，2010年这个项目由apache software foundation迁移到了google code，并且改名为MyBatis。2013年11月迁移到Github。