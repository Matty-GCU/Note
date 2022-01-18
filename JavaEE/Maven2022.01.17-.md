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

[Maven入门教程 - 静默虚空 - 博客园](https://www.cnblogs.com/jingmoxukong/p/5591368.html)

### 0.3 文档

官方文档：[Maven – Maven Documentation](https://maven.apache.org/guides/index.html)

中文翻译文档：[Maven中文手册](https://www.dba.cn/book/maven/)（非常有用，非常重要的学习资料）

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

> 所谓的"约定优于配置"，在maven中并不是完全不可以修改的，他们只是一些配置的默认值而已。但是除非必要，并不需要去修改那些约定内容。maven默认的文件存放结构如下：
>
> 每一个阶段的任务都知道怎么正确完成自己的工作，比如compile任务就知道从src/main/java下编译所有的java文件，并把它的输出class文件存放到target/classes中。
>
> 对maven来说，采用"约定优于配置"的策略可以减少修改配置的工作量，也可以降低学习成本，更重要的是，给项目引入了统一的规范。

#### 1.3.4 一致的构建模型和插件机制

略

### 1.4 Maven的生命周期

> maven把项目的构建划分为不同的生命周期(lifecycle)。粗略一点的话，它这个过程(phase)包括：编译、测试、打包、集成测试、验证、部署。maven中所有的执行动作(goal)都需要指明自己在这个过程中的执行位置，然后maven执行的时候，就依照过程的发展依次调用这些goal进行各种处理。
>
> 这个也是maven的一个基本调度机制。一般来说，位置稍后的过程都会依赖于之前的过程。当然，maven同样提供了配置文件，可以依照用户要求，跳过某些阶段。

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

| 目录                          | 存放                                                 |
| ----------------------------- | ---------------------------------------------------- |
| ${basedir}                    | pom.xml和所有子目录                                  |
| ${basedir}/src/main/java      | 项目的java源码                                       |
| ${basedir}/src/main/resources | 项目的资源文件，如.property文件、图片等              |
| ${basedir}/src/test/java      | 测试使用的java源码                                   |
| ${basedir}/src/test/resources | 测试使用的资源文件                                   |
| ${basedir}/target             | 输出目录，所有的输出物都存放在这个目录下（自动生成） |
| ${basedir}/target/classes     | 编译后的class文件存放处（自动生成）                  |

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

再在src/main/java里手动创建对应的包目录，然后手写一个简单的.java文件

```java
package xyz.wuhang.demo;

public class HelloWorld {
	public static void main(String[] args) {
		System.out.println("Hello Maven!");
	}
}
```

## 4. Maven项目的编译与启动

### 4.1 修改settings.xml文件

第一次下载前要修改conf目录下的配置文件settings.xml。

可以在\<localRepository\>里看到默认下载位置：

```xml
<!-- localRepository
| The path to the local repository maven will use to store artifacts.
|
| Default: ${user.home}/.m2/repository
<localRepository>/path/to/local/repo</localRepository>
-->

<!-- 加上这行，更改默认下载位置 -->
<localRepository>/C:/Java/.m2/repository</localRepository>
```

可以在\<mirrors\>里添加阿里巴巴的镜像源，提高下载速度：

```xml
<mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
    
    <!-- 加上这段，添加镜像源 -->
    <mirror>
      <id>nexus-aliyun</id>
      <mirrorOf>central</mirrorOf>
      <name>Nexus Aliyun</name>
      <url>https://maven.aliyun.com/nexus/content/groups/public/</url>
    </mirror>
</mirrors>
```

### 4.2 编译java文件

项目根目录下管理员cmd执行：

```shell
mvn compile
```

编译成功后会出现**BUILD SUCCESS**

* 可能的报错1

  ```xml
  C:\Users\Matty's PC\Desktop\AMavenProject>mvn compile
  [INFO] Scanning for projects...
  [ERROR] [ERROR] Some problems were encountered while processing the POMs:
  [FATAL] Non-parseable POM C:\Users\Matty's PC\Desktop\AMavenProject\pom.xml: processing instruction can not have PITarget with reserved xml name (position: START_DOCUMENT seen <!--\u8fd9\u90e8\u5206\u5185\u5bb9(2-6\u884c)\u90fd\u662f\u56fa\u5b9a\u8981\u8fd9\u4e48\u5199\u7684\uff0c\u4e0d\u7528\u7ba1-->\r\n<?xml ... @2:7)  @ line 2, column 7
  ...
  ```

  报错原因：pom.xml文件的第一行写了中文注释——这是不可以的（哪怕注释格式正确），因为xml文件的第一行必须是XML文档声明，也就是\<xml\>那堆东西。

  解决方法：删除第一行注释。其他地方怎么写注释都没事，但第一行必须是\<xml>开头。

* 可能的报错2

  ```xml
  ...
  [ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) on project AMarvenProject: Compilation failure: Compilation failure:
  [ERROR] 不再支持源选项 5。请使用 6 或更高版本。
  [ERROR] 不再支持目标选项 1.5。请使用 1.6 或更高版本。
  ...
  ```

  报错原因：没有指定java的版本。

  解决方法：在pom.xml中添加如下的属性。

  ```xml
  <properties>
      <!-- 填入你所使用的jdk版本（如1.8、11、14等等） -->
      <maven.compiler.target>11</maven.compiler.target>
      <maven.compiler.source>11</maven.compiler.source>
  </properties>
  ```

### 4.3 执行main方法

项目根目录下管理员cmd执行：

```shell
mvn exec:java -Dexec.mainClass="main方法所在主类的包路径，如xyz.wuhang.demo"
```

执行成功后会出现运行结果（比如打印出一句“Hello Maven!”），并且同样会出现**BUILD SUCCESS**

* 可能的报错1

  ```shell
  ...
  [WARNING]
  java.lang.ClassNotFoundException: xyz.wuhang.demo.HelloWorld
      at java.net.URLClassLoader.findClass (URLClassLoader.java:476)
      at java.lang.ClassLoader.loadClass (ClassLoader.java:588)
      at java.lang.ClassLoader.loadClass (ClassLoader.java:521)
      at org.codehaus.mojo.exec.ExecJavaMojo$1.run (ExecJavaMojo.java:246)
      at java.lang.Thread.run (Thread.java:834)
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  0.442 s
  [INFO] Finished at: 2022-01-18T17:00:04+08:00
  [INFO] ------------------------------------------------------------------------
  [ERROR] Failed to execute goal org.codehaus.mojo:exec-maven-plugin:3.0.0:java (default-cli) on project AMarvenProject: An exception occured while executing the Java class. xyz.wuhang.demo.HelloWorld -> [Help 1]
  ...
  ```

  报错原因（我的理解）：.java文件不能放在src/**test**/java里，那样虽然编译能通过，但运行会报java.lang.ClassNotFoundException。压根编译的时候压根就没编译到它，相当于啥都没编译，当然能通过。但执行时就找不到了，执行命令`mvn exec:java -Dexec.mainClass="xyz.wuhang.demo.HelloWorld"`时应该是去src/**main**/java里找的。

  解决方法：把.java文件移到src/main/java对应的包目录下。

## 5. Maven常用命令

虽然IDEA等工具为我们提供了图形界面化工具，但其底层还是依靠命令来驱动的，因此了解并熟练运用Maven的命令行操作是很有必要的。

### 5.1 Maven命令格式

```
mvn [plugin-name]:[goal-name]
```

例如，一个 Java 工程可以使用 maven-compiler-plugin 的 compile-goal 编译，使用`mvn compiler:compile`命令。当然这条命令在实际使用中可以省去plugin-name。

建议在项目目录（也就是pom.xml所在目录）下运行Maven命令，否则必须通过参数来指定项目目录。

### 5.2 常用命令表

| 命令                      | 作用                                                  |
| ------------------------- | ----------------------------------------------------- |
| mvn -version (或maven -v) | 打印Maven的版本信息                                   |
| mvn clean                 | 清理项目产生的临时文件（删除项目目录下的target目录）  |
| mvn compile               | 编译src/main/java目录下的源代码                       |
| mvn package               | 打包项目（会在target目录下生成jar/war包）             |
| mvn deploy                | 部署项目，将项目的jar/war包发布到远程供其他人下载使用 |
| mvn test                  | 执行src/test/java目录下junit的测试用例                |
| mvn install               | 将项目需要的jar/war包复制到本地仓库以供使用           |
| mvn site                  | 生成项目相关信息的网站                                |
| mvn eclipse:eclipse       | 将项目转化为Eclipse项目                               |
| mvn dependency:tree       | 以树结构打印出项目中的依赖                            |
| mvn archetype:generate    | 根据模板创建一个Java项目                              |
| mvn tomcat7:run           | 在tomcat容器中运行web应用                             |
| mvn jetty:run             | 在Jetty Servlet容器中运行web项目                      |

* Jetty是一个小型服务器，常用于开发端，因为它启动比较快，便于调试。
* 以上命令看得多、用得多，就记住了，不用纠结于全部记下来

### 5.3 命令参数

*注意：第一次学习时搞不懂是正常的，只需要了解大概的概念，保证后面遇到的时候不陌生即可。*

很多命令都可以携带参数，以完成更精准的任务。

#### 5.3.1 -D 传入属性

以**-D**开头的参数，例如`mvn package -Dmaven.test.skip=true`，表示打包项目并跳过单元测试。同理`mvn deploy -Dmaven.test.skip=true`表示部署项目并跳过单元测试。

#### 5.3.2 -P 使用指定的profile配置

一般项目开发需要有多个环境，比如开发、测试、预发、正式4个环境，在pom.xml中的配置如下：



profiles中定义了各个环境的变量id，filters中定义了变量配置文件的地址，其中地址中的环境变量就是上面profile中的值，resources中定义的是哪些目录下的文件会被配置文件中定义的变量替换。

通过maven可以实现按不同环境进行打包部署，例如

```shell
mvn package -Pdev -Dmaven.test.skip=true
```

表示打包本地环境并跳过单元测试。