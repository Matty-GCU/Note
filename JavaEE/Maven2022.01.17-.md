# Maven学习笔记

## 0. 前言

### 0.1 基于教程

[（超详细）2021最新Maven教程-Maven基础篇之Maven实战入门-最新IDEA版maven【半天快速掌握,附全套视频资料】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Fz4y167p5?p=2&spm_id_from=pageDriver)（发布时间为2021-01-12）

### 0.2 参考资料

[maven 是什么？ - ZeroGirl - 博客园](https://www.cnblogs.com/chcha1/p/12426420.html)

[如何给小白说明maven是什么？ - Martin Wang的回答 - 知乎](https://www.zhihu.com/question/32240102/answer/340029398)

[Maven学习七：坐标三元素_纪争光-CSDN博客_maven坐标三要素](https://blog.csdn.net/lfsfxy9/article/details/17045369)

[MVC的dao层、service层和controller层_勤于总结的菜鸟-CSDN博客_dao层和service层和control](https://blog.csdn.net/qq_26411021/article/details/79493340)

POM标签大全详解可以在这里看：<a id="超级POM">[Maven POM | 菜鸟教程](https://www.runoob.com/maven/maven-pom.html)</a>

## 1. Maven概述

### 1.1 简介

maven  /'mevən/  n.内行，专家（单词来源于意第绪语，意为“知识的积累”）

作为Apache组织中的一个颇为成功的开源项目，Maven主要服务于基于Java平台的项目构建，依赖管理和项目信息管理。

**Maven是基于项目对象模型(Project Object Model, POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具。**

### 1.2 Java项目构建工具

**Ant**

最早的构建工具，基于IDE。大概是2000年出的，是当时最流行的Java项目构建工具。它的缺点是其XML脚本编写格式使得XML文件特别大。

Ant比较老，一般是一些传统的软件企业公司在使用。

**Maven**

填补了Ant的缺点，但仍然采用XML作为配置文件格式。Maven第一次支持了从网络上下载的功能。

Maven使用纯Java编写，是当下大多数互联网公司使用的一个构建工具，中文文档也比较齐全。

**Gradle**

吸收了以上两个的优点，比如Ant的灵活和Maven的生命周期管理。被google作为android的御用管理工具。Gradle不用XML，而是采用DSL作为配置文件格式，使得脚本更加简洁。

Gradle使用groovy编写，是目前比较新型的构建工具。

### 1.3 Maven的四大特性

#### 1.3.1 依赖管理系统

先来看几个典型的依赖引用：

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.1.0</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.6</version>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.20</version>
</dependency>
```

Maven为Java世界引入了一个新的依赖管理系统。jar包升级时，只需修改配置文件即可。

在Java世界中，可以用groupId、artifactId、version组成的坐标(Coordination)唯一标识一个依赖。任何基于Maven构建的项目自身也必须定义这三个属性。

##### 1.3.1.1 三个重要属性的含义

**groupId**

定义了当前项目属于哪个“组”，这个“组”的名称往往与项目隶属的组织或公司名称有关，并且其命名通常是该组织或公司的域名反写。

必须定义。

**artifactId**

定义了当前项目在“组”中唯一的Id。当前项目可能是“大项目”中的一个“小项目”或模块（由于Maven中模块的概念，一个实际项目往往会被分成很多模块），推荐使用“大项目”的名称作为artifactId的前缀。

必须定义。

**version**

当前项目所处的版本。格式通常为：x.x.x-里程碑（了解即可，不必深究）

* 第一个x：大版本，有重大变革
* 第二个x：小版本，修复bug或增加功能
* 第三个x：小修改或小更新
* 里程碑：
  * SNAPSHOT：快照，开发版
  * alpha：内部测试
  * beta：公开测试
  * Release | RC：发布版
  * GA：正常版本

必须定义。

#### 1.3.2 多模块构建

dao、service、controller三层分离，将一个项目分解为多个模块，已经是很常用的一种方式。

在Maven中需要定义一个parent POM作为一组module的聚合POM。在该POM中可以使用<modules\>标签来定义一组子模块。parent POM不会有什么实际构建的产出，但parent POM中的build配置以及依赖配置都会自动继承给子module。

#### 1.3.3 一致的项目结构

Ant时代大家创建Java项目目录时比较随意，并通过Ant配置指定哪些属于source，哪些属于testSource等。

而Maven在设计之初的理念就是“约定大于配置”(Conversion over configuration)。Maven制定了一套项目目录结构作为标准的Java项目结构，解决了不同IDE带来的文件目录不一致的问题。

#### 1.3.4 一致的构建模型和插件机制

略

## 2. Maven环境配置

### 2.1 下载Maven

下载地址：[Maven – Download Apache Maven](https://maven.apache.org/download.cgi)

我下载的最新版是3.8.4，老师使用的是3.6.2。

### 2.2 配置环境变量

下载.zip后解压到任意目录，然后配一个环境变量MAVEN_HOME指向Maven的安装目录（也可以直接指向bin目录，不过这样不太规范就是了，所谓"HOME"应该是指安装目录的，即bin的上一级），最后再把**%MAVEN_HOME%\bin**加进Path环境变量中。

在命令行运行`mvn -v`检查是否配置成功。

如果出现这种情况：

```bash
Matty's PC@DESKTOP-E8M9L15 MINGW64 ~/Desktop
$ java -version		#给Java配的环境变量能用
java version "11.0.13" 2021-10-19 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.13+10-LTS-370)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.13+10-LTS-370, mixed mode)

Matty's PC@DESKTOP-E8M9L15 MINGW64 ~/Desktop
$ mvn -v			#给Maven配的环境变量却用不了，说JAVA_HOME配错了
The JAVA_HOME environment variable is not defined correctly,
this environment variable is needed to run this program.
```

说明你把JAVA_HOME环境变量指向了jdk的bin目录里了。

* 解决方法一：把JAVA_HOME指向bin目录的上一级，再改一下Path
* 解决方法二：删掉JAVA_HOME，直接在Path里指向jdk的bin目录

## 3. Maven目录结构

### 3.1 认识Maven项目目录结构

在所有的IDE中创建的Maven项目的目录结构都是一模一样的，不存在目录不兼容问题。

| 目录                          | 存放                            |
| ----------------------------- | ------------------------------- |
| ${basedir}                    | pom.xml和所有子目录             |
| ${basedir}/src/main/java      | 项目的java源码                  |
| ${basedir}/src/main/resources | 项目的资源文件，如.property文件 |
| ${basedir}/src/test/java      | 测试使用的java源码              |
| ${basedir}/src/test/resources | 测试使用的资源文件              |

* pom.xml是最为核心的配置文件，记录该Maven项目所需要的所有jar包的依赖，和所有插件的依赖。
* 所有目录的命名和结构都是固定的，注意大小写和单复数不要出错。

### 3.2 手动创建Maven项目

*注意：“手动”两字已经说明这一小节仅仅只是为了加深理解所做的小任务，实际工作中根本不需要我们手动创建目录，也不用手动编写整个pom.xml文件（更不用背/默写），只要能够看懂、会用就好了。*

先手动创建相对应的目录，再在根目录手动写一个测试用的pom.xml文件，内容如下：

（以下注释大部分出自菜鸟教程的<a href="#超级POM">POM标签大全详解</a>）

```xml
<!--这部分内容(2-6行)都是固定要这么写的，不用管-->
<?xml version="1.0" encoding="utf-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <!--Maven3和Maven2都必须用4.0.0的modelVersion，也是固定的，不用管-->
    <modelVersion>4.0.0</modelVersion>
    <!--项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 如com.mycompany.app生成的相对路径为：/com/mycompany/app -->
    <groupId>xyz.wuhang</groupId>
    <!-- 构件的标识符，它和group ID一起唯一标识一个构件。换句话说，你不能有两个不同的项目拥有同样的artifact ID和groupID；在某个特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven为项目产生的构件包括：JARs，源码，二进制发布和WARs等。 -->
    <artifactId>AMarvenProject</artifactId>
    <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号 -->
    <version>0.0.1-SNAPSHOT</version>
	<!--项目产生的构件类型，例如jar、war、ear、pom。插件可以创建他们自己的构件类型，所以前面列的不是全部构件类型 -->
    <packaging>jar</packaging>
    
    <!--项目的名称, Maven产生的文档用 -->
    <name>mavenPrj01</name>
    <!--项目主页的URL, Maven产生的文档用 -->
    <url>http://maven.apache.org</url>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    
    <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。 -->
    <dependencies>
        <dependency>
            <!--依赖的group ID -->
            <groupId>junit</groupId>
            <!--依赖的artifact ID -->
            <artifactId>junit</artifactId>
            <!--依赖的版本号。 -->
            <version>3.8.1</version>
            <!--依赖范围。在项目发布过程中，帮助决定哪些构件被包括进来。 -->
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

再手动创建包目录，然后手写一个简单的.java文件

```java
package xyz.wuhang.demo;

public class HelloWorld {
	public static void main(String[] args) {
		System.out.println("Hello Maven!");
	}
}
```

### 3.3 Maven项目的编译与启动