终审关注点：

* 三个坐标不用纠结定义
* 注意图片要改相对地址，否则放上博客会挂的。或者考虑图床？

# Maven学习笔记

## 0. 前言

### 0.1 基于教程

[（超详细）2021最新Maven教程-Maven基础篇之Maven实战入门-最新IDEA版maven【半天快速掌握,附全套视频资料】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Fz4y167p5?p=2&spm_id_from=pageDriver)（发布时间为2021-01-12）

### 0.2 参考教程

[Maven 教程 | 菜鸟教程](https://www.runoob.com/maven/maven-tutorial.html)

* <a id="超级POM">[Maven POM | 菜鸟教程](https://www.runoob.com/maven/maven-pom.html)（POM标签大全详解可以在这里看）</a>

[Maven入门教程 - 静默虚空 - 博客园](https://www.cnblogs.com/jingmoxukong/p/5591368.html)

[Maven简介 | Maven教程网](http://mvnbook.com/index.html)

### 0.3 文档

官方文档：[Maven – Maven Documentation](https://maven.apache.org/guides/index.html)

中文翻译文档：[Maven中文手册](https://www.dba.cn/book/maven/)（非常有用，非常重要的学习资料）

### 0.4 环境版本

* Maven 3.8.4（老师用的是3.6.2）
* IDEA 2021.1.3
* JDK 11

## 1. Maven概述OK

### 1.1 Maven简介

**Maven是基于项目对象模型(Project Object Model, POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具。**

这给纯新手的参考资料，帮助理解。

> [如何给小白说明maven是什么？ - Martin Wang的回答 - 知乎](https://www.zhihu.com/question/32240102/answer/340029398)

这给所有人的参考资料，质量超高，以致于我想整篇Copy过来......下面将摘抄一段原文。

> 原文：[Maven简介 | Maven教程网](http://mvnbook.com/index.html)
>
> ## Maven读音
>
> Maven 的正确发音是\[ˈmeɪvn\](美文)，而不是"马文"。Maven 在美国是一个口语化的词语，代表专家、内行的意思，约等于北京话中的老炮儿。
>
> ## Maven的组成
>
> Maven是一个项目管理工具，它包含了：项目对象模型 (POM，Project Object Model)，项目生命周期(Project Lifecycle)，依赖管理系统(Dependency Management System)和各种插件。插件主要用来实现生命周期各个阶段(phase)的目标(goal)。Maven的组成如下所示：
>
> <img src="http://mvnbook.com/static/image/maven-core.png" alt="img" style="zoom:67%;" />
>
> 
>
> ***不过，上述介绍对于完全没有 Maven 实践经验的人来说，看了等于没看，并没有用处。只有当读者通读本站内容之后，反过头再看，才能豁然开朗。***
>
> ## 什么是项目构建？
>
> 构建是什么呢？简单地说，构建就是软件项目生产的整个过程，这个过程应该包括：
>
> （1）文档和代码的生成（有些项目会使用代码自动生成工具，比如数据库访问代码的逆向工程）
>
> （2）代码的编译、测试和打包
>
> （3）打包好的代码进行分发或者部署
>
> 由此可见，项目的构建可绝不仅仅是编译软件这件事情。除了写代码，在项目层面做的大部分工作，都包含在构建的过程中。
>
> 有了 Maven 这个开源利器，构建中的这些过程都能够进行良好的定义，而且 Maven 能够帮我们串起来形成一个自动构建过程，这样比我们手动执行要高效得多。
>
> ## 项目依赖管理的噩梦
>
> Java 最大的一个优势就是有非常强大的生态，整个生态中有无数的框架和API供人使用。我们在创建实际的项目过程中不可避免地需要用到这些框架和API，而它们通常都是以 Jar 包的形式提供。 相信很多人都经历过 Jar Hell 的问题吧。事实上，让一个项目所依赖的外部 Jar 包保持正确的版本和最新的状态，是件非常苦逼的事情。我们编译项目的时候，需要在 ClassPath 上存放依赖的 Jar 包，而这些 Jar 包还会有其他依赖。你一定经历过递归地一个个去下载所有外部依赖的痛苦过程吧，并且还要确保下载的版本都是正确的，当项目越来越复杂的时候，这是件极其麻烦的事情。
>
> Maven 的出现让我们获得了解脱，Maven 可以自动帮我们做依赖管理，我们需要做的就是在 POM 文件里指定依赖 Jar 包的名称、版本号，Maven 会自动下载，并递归地去下载依赖的进一步依赖。
>
> 另外，Maven 还提供一个非常方便的功能---快照依赖。快照依赖指的是那些还在开发中的内部依赖包。与其你经常地更新版本号来获取最新版本，不如直接依赖项目的快照版本。快照版本的每一个 Build 版本都会被下载到本地仓库，即使该快照版本已经在本地仓库了。使用快照依赖可以确保本地仓库中的每一个 Build 版本都是最新的，这对我们快速迭代开发是一个非常酷的特性。
>
> ## Maven学习要求
>
> Maven 是 Java 项目构建工具，可以用于管理 Java 依赖，还可以用于编译、打包以及发布 Java 项目，类似于 JavaScript 生态系统中的 NPM。因此，每一位高级工程师或软件架构师，都应该至少具备以下两项 Maven 技能：
>
> （1）熟练使用 Maven 构建项目；
>
> （2）排查并调解项目依赖冲突；

### 1.2 Java世界的项目构建工具OK

**Ant**

最早的构建工具。大概是2000年出的，是当时最流行的Java项目构建工具。它的缺点是其XML脚本编写格式使得XML文件特别大。

Ant比较老，现在一般是一些传统的软件企业公司在使用。

**Maven**

填补了Ant的缺点，但仍然采用XML作为配置文件格式。Maven第一次支持了从网络上下载的功能。

Maven使用纯Java编写，是当下大多数互联网公司使用的一个构建工具，中文文档也比较齐全。

**Gradle**

吸收了以上两个的优点，比如Ant的灵活和Maven的生命周期管理。被google作为android的御用管理工具。Gradle不用XML，而是采用DSL作为配置文件格式，使得脚本更加简洁。

Gradle使用groovy编写，是目前比较新型的构建工具。

> 扩展阅读：<a id="1.2扩展阅读">[项目自动构建工具对比(Maven、Gradle、Ant) - 灰色飘零 - 博客园](https://www.cnblogs.com/renhui/p/6855934.html)</a>

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

在Java世界中，可以用groupId、artifactId、version组成的坐标唯一标识一个依赖，任何基于Maven构建的项目自身也必须定义这三个属性。

#### 1.3.2 多模块构建

dao、service、controller三层分离，将一个项目分解为多个模块，已经是很常用的一种方式。

在Maven中需要定义一个parent POM，在该POM中可以使用<modules\>标签来定义一组子模块。parent POM不会有也没法有实际构建的产出，但parent POM中的build配置以及dependencies配置都会自动继承给子module。

#### 1.3.3 一致的项目结构

Ant时代大家创建Java项目目录时比较随意，并通过Ant配置指定哪些属于source，哪些属于testSource等。

而Maven在设计之初的理念就是“约定大于配置”(Conversion over configuration)。Maven制定了一套项目目录结构作为标准的Java项目结构，解决了不同IDE带来的文件目录不一致的问题。

> 所谓的"约定优于配置"，在maven中并不是完全不可以修改的，他们只是一些配置的默认值而已。但是除非必要，并不需要去修改那些约定内容。maven默认的文件存放结构如下：
>
> 每一个阶段的任务都知道怎么正确完成自己的工作，比如compile任务就知道从src/main/java下编译所有的java文件，并把它的输出class文件存放到target/classes中。
>
> 对maven来说，采用"约定优于配置"的策略可以减少修改配置的工作量，也可以降低学习成本，更重要的是，给项目引入了统一的规范。

#### 1.3.4 一致的构建模型和插件机制

参考<a href="#1.2扩展阅读">1.2扩展阅读</a>里的“一致的构建模型”和“插件机制”部分。

### 1.4 Maven的生命周期



> maven把项目的构建划分为不同的生命周期(lifecycle)。粗略一点的话，它这个过程(phase)包括：编译、测试、打包、集成测试、验证、部署。maven中所有的执行动作(goal)都需要指明自己在这个过程中的执行位置，然后maven执行的时候，就依照过程的发展依次调用这些goal进行各种处理。
>
> 这个也是maven的一个基本调度机制。一般来说，位置稍后的过程都会依赖于之前的过程。当然，maven同样提供了配置文件，可以依照用户要求，跳过某些阶段。

## 2. Maven环境配置

### 2.1 下载Maven

下载地址：[Maven – Download Apache Maven](https://maven.apache.org/download.cgi)

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

### <a id="4.1">4.1</a> 修改settings.xml

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

### 5.3 命令参数-D和-P

很多命令都可以携带参数，以完成更精准的任务。例如`mvn package -Dmaven.test.skip=true`，表示打包项目并跳过单元测试。同理`mvn deploy -Dmaven.test.skip=true`表示部署项目并跳过单元测试。

*注意：这一部分在第一次学习时搞不懂是正常的，毕竟没有真正用过。只需要了解大概的概念，保证后面遇到的时候不陌生即可。*

对不起，我要直接抄这篇文档了，写得实在太好了。

原文：[Maven参数配置 | Maven教程网](http://mvnbook.com/maven-properties.html)

> Maven 命令参数 中的 -D 表示 Properties属性，而 -P 表示 Profiles配置文件。
>
> ## Maven 命令参数-D和-P的区别
>
> -D 表示设置 Properties属性，使用命令行设置属性 -D 的正确方法是：
>
> ```
> mvn -DpropertyName=propertyValue clean package
> ```
>
> 如果 propertyName 不存在于 pom.xml 文件中，它将被设置。如果 propertyName 已经存在 pom.xml 文件中，其值将被作为参数传递的值覆盖。要设置多个变量，请使用多个空格分隔符加-D：
>
> ```
> mvn -DpropA=valueA -DpropB=valueB -DpropC=valueC clean package
> ```
>
> 例如，现有 pom.xml 文件如下所示：
>
> ```
> <properties>
>     <theme>myDefaultTheme</theme>
> </properties>
> ```
>
> 那么在执行过程中 mvn -Dtheme=newValue clean package 会覆盖 theme 的值，具有如下效果：
>
> ```
> <properties>
>     <theme>newValue</theme>
> </properties>
> ```
>
> -P 代表 Profiles 配置文件的属性，也就是说在 <profiles> 指定的 <id> 中，可以通过-P进行传递或者赋值。
>
> 例如，现有 pom.xml 文件如下所示：
>
> ```
> <profiles>
>       <profile>
>           <id>test</id>
>           ...
>       </profile>
> </profiles>
> ```
>
> 执行 mvn test -Ptest 为触发配置文件。或者如下所示：
>
> ```
> <profile>
>    <id>test</id>
>    
>    <activation>
>       <property>
>          <name>env</name>
>          <value>test</value>
>       </property>
>    </activation>
>    ...
> </profile>
> ```
>
> 执行mvn test -Penv=test 为触发配置文件。

### 5.4 Maven命令深度理解



原文：[Maven 命令深度理解 | Maven教程网](http://mvnbook.com/maven-command.html)

> Maven 命令看起来简单，一学即会 。其实，Maven 命令底层是插件的执行过程。
>
> 了解插件和插件目标才有助于深刻的理解 Maven命令。
>
> ## 插件与命令的关系
>
> Maven本质上是一个插件框架，它的核心并不执行任何具体的构建任务，所有这些任务都交给插件来完成。
>
> ![Maven插件](http://mvnbook.com/static/image/maven-core.png)
>
> Maven 实际上是一个依赖插件执行的框架，每个任务实际上是由插件完成。所以，Maven 命令都是由插件来执行的。
>
> ## Maven 命令的分类
>
> Maven 提供了两种类型的命令。一种必须在项目中运行，视为项目命令；另外一种则不需要，视为全局命令。
>
> ## Maven 命令格式解读
>
> Maven 命令都是由插件来执行的，其语法格式如下所示：
>
> ```
> mvn [plugin-name]:[goal-name]
> ```
>
> 例如，下面的命令：
>
> ```
> mvn help:effective-pom
> ```
>
> 这个命令采用了缩写的形式，其全称是这样的：
>
> ```
> org.apache.maven.plugins:maven-help-plugin:2.2:effective-pom
> ```
>
> 此命令以分号为分隔符，包含了 groupId，artifactId，version，goal 四部分。若 groupId 为 org.apache.maven.plugins 则可以使用上述的简写形式，也就是说和下面的命令是等价的：
>
> ```
> mvn help:effective-pom
> 
> mvn org.apache.maven.plugins:maven-help-plugin:2.2:effective-pom
> ```
>
> 都是执行了 maven-help-plugin 这个 plugin 中的 effective-pom 这个 goal。每次都要敲这么长一串命令是很繁琐的，因此才有了上述的简写的形式。
>
> > **提醒：**Maven 命名有要求，Maven 团队维护官方插件的保留命名方式是 maven-<myplugin>-plugin。

## 6. IDEA集成Maven环境OK

### 6.1 更改IDEA全局设置

对于2019及2019之前版本的IDEA，是打开File -> Other Settings -> Settins For New Projects

对于2021.1.3版本的IDEA（本机实测），是打开File -> New Projects Setting -> Settins For New Projects

> 为什么不改File -> Settings？因为它可能只对当前项目有效，下一次新建项目又要重新设置。

找到Build, Execution, Deployment -> Build Tools > Maven，或直接搜索"Maven"

把Maven home path改成我们自己安装的Maven的MAVEN_HOME（默认设置的是IDEA自带的那个MAVEN），然后把User settins file和Local repository都改一下（选中下面两个Override才能改），如图所示。

<img src="C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\idea设置maven.png" alt="idea设置maven" style="zoom: 67%;" />

### 6.2 使用Maven创建Java项目

#### 6.2.1 新建Project并选择Maven项目模板，Next

选择Maven的最普通最基础的Java项目模板**maven-archetype-quickstart**进行创建即可，如图。

> 参考资料：
>
> [Maven 三种archetype说明_我是程序媛，我骄傲-CSDN博客_maven-archetype-quickstart](https://blog.csdn.net/cx1110162/article/details/78297654)
>
> [Maven 的41种骨架功能介绍 - _zao123 - 博客园](https://www.cnblogs.com/iusmile/archive/2012/11/14/2770118.html)

<img src="C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\idea创建maven.png" alt="idea创建maven" style="zoom:67%;" />

#### 6.2.2 设定项目的GroupId和ArtifactId，Next

<a id="设定项目的GroupId和ArtifactId"><img src="C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\idea创建maven-名称.png" alt="idea创建maven-名称" style="zoom:67%;" /></a>

#### 6.2.3 检查Maven环境，Finished

<img src="C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\idea创建maven-完成.png" alt="idea创建maven-完成"  />

#### 6.2.4 手动创建资源文件夹

根据quickstart模板创建的src目录中是没有resources的，所以我们需要手动创建resources目录，并把它mark为资源文件。

![idea手动设置资源目录](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\idea手动设置资源目录.png)

也可以Ctrl+Alt+Shift+S一套组合拳调出Project Structure设置，在那里操作。

![idea手动设置资源目录2](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\idea手动设置资源目录2.png)

#### 6.2.5 设置命令

以设置编译命令为例，直接上图：

![编译命令1](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\编译命令1.png)

![编译命令2](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\编译命令2.png)

![编译命令3](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\编译命令3.png)

![编译命令4](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\编译命令4.png)

**总结一下：**

你可以在在运行按钮旁边的Add Configuration或Edit Configuration那里自定义一个或多个命令（要点击“+”，否则你只是修改了原来的命令，而不是新增了新命令）。

对于常见的基本Maven命令，也可以在右侧栏点开"Maven"，直接点击执行你想要的命令。

**什么原理？**

假设，你新建了一个命令，Name叫做"just run"，Command Line设为"exec:java -Dexec.mainClass="xyz.wuhang.App"。保存好设置后，你运行这条命令的效果，等同于你在项目目录下使用命令行输入"mvn exec:java -Dexec.mainClass="xyz.wuhang.App"。

说白了，和在命令行输入没有任何区别，只不过IDEA帮你省去了手动输入那一步。

### 6.3 使用Maven创建Web项目

#### 6.3.1 创建

选择**maven-archetype-webapp**模板即可，其他步骤与创建Java项目没什么区别。

#### 6.3.2 配置

还要在pom.xml里修改一下配置信息

* 在properties标签块里修改一下JDK版本

* 在build标签块里删除pluginManagement标签块

  > 为什么要删？
  >
  > 因为一般pluginManagement是用在父pom中的，有点Java抽象类的意思，只是声明，实际上Maven并不会为当前项目加载。
  >
  > 参考资料：
  >
  > [Maven中plugins和pluginManagement的区别 - hxwang - 博客园](https://www.cnblogs.com/whx7762/p/8072755.html)
  >
  > [java - What is pluginManagement in Maven's pom.xml? - Stack Overflow](https://stackoverflow.com/questions/10483180/what-is-pluginmanagement-in-mavens-pom-xml)

* 在build标签块的plugins标签块里添加插件声明：

  ```xml
  ......
  <build>
      ......
      <!-- 加上 -->
      <plugins>
          <!-- jetty插件 -->
          <plugin>
              <groupId>org.eclipse.jetty</groupId>
              <artifactId>jetty-maven-plugin</artifactId>
              <version>9.4.11.v20180605</version>
  			<!-- configuration部分可以不写，那样会采用默认设置 -->
              <configuration>
                  <!-- 热部署，每10秒扫描一次 -->
                  <scanIntervalSeconds>10</scanIntervalSeconds>
                  <webAppConfig>
                      <!-- 此处为项目的访问路径(contextPath上下文路径) -->
                      <contextPath>/test111</contextPath>
                  </webAppConfig>
              </configuration>
          </plugin>
          
          <!-- tomcat插件 -->
          <plugin>
              <groupId>org.apache.tomcat.maven</groupId>
              <artifactId>tomcat7-maven-plugin</artifactId>
              <version>2.2</version>
  			<!-- 同理，configuration部分也可以不写，那样会采用默认设置 -->
              <configuration>
                  <!--此处配置了访问的端口号 -->
                  <port>8081</port>
                  <path>/test222</path>
                  <!-- 默认为ISO-8859-1 -->
                  <uriEncoding>UTF-8</uriEncoding>
              </configuration>
  		</plugin>   
      </plugins>
  </build>
  ......
  ```

然后IDEA会自动扫描并下载项目需要的插件，这个过程可以在底部栏的Build里看到，如图：

![自动扫描并下载项目需要的插件](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\自动扫描并下载项目需要的插件.png)

![自动扫描并下载项目需要的插件2](C:\Me\杂\笔记\【学习笔记】\JavaEE\Maven2022.01.17-.assets\自动扫描并下载项目需要的插件2.png)

不过好像只有刚打开IDEA的时候才会自动扫描、下载。如果不想反复重启IDEA的话，也可以执行一下Maven的`install`命令。

#### 6.3.3 启动

**设置命令`jetty:run`，表示以jetty方式运行项目。**

5.1命令格式没忘吧？jetty是plugin-name，run是goal-name。

6.2.5设置命令的原理没忘吧？本质上只是在命令行输入这条命令并回车而已。

**也可以设置命令`jetty:run -Djetty.port=9090`。**

5.3的-D命令参数没忘吧？这条命令相当于执行jetty:run命令时把jetty.port的值设置为9090，但设置生效的时间仅限于这条命令执行期间——下次运行的port值还是默认8080（如果不带上-D的话）。

* 可能的报错1

  按照原视频教学用的jetty插件配置（如下，这是原来的），在我的本机环境下会报500错误，说无法编译jsp文件。在排除掉JDK不匹配的问题后仍然报错。经评论区大佬@80388400963_bili提醒，改成比较新的版本就好了（上面配置写的就是改过的，能用的）
  
  > ~~<!-- 以下配置不推荐使用！！ -->~~
  >
  > ~~<plugin>~~
  >     ~~<groupId>org.mortbay.jetty</groupId>~~
  >     ~~<artifactId>maven-jetty-plugin</artifactId>~~
  >     ~~<version>6.1.25</version>~~
  >
  > ​	~~<!-- configuration部分也可以不写，那样会采用默认设置 -->~~
  > ​    ~~<configuration>~~
  > ​        ~~<!-- 热部署，每10秒扫描一次 -->~~
  > ​        ~~<scanIntervalSeconds>10</scanIntervalSeconds>~~
  > ​        ~~<!-- 此处为项目的访问路径(contextPath上下文路径) -->~~
  > ​        ~~<contextPath>/test111</contextPath>~~
  > ​        ~~<connectors>~~
  > ​            ~~<connector implementation="org.mortbay.jetty.nio.SelectChannelConnector">~~
  > ​                ~~<!--此处配置了访问的端口号 -->~~
  > ​                ~~<port>9090</port>~~
  > ​            ~~</connector>~~
  > ​        ~~</connectors>~~
  > ​    ~~</configuration>~~
  > ~~</plugin>~~

#### 6.3.4 如何获取插件信息

Q: 虽然不用自己下载，但我们怎么找需要的插件版本和名称呢？

A: 我们可以在这里找：[Maven – Available Plugins](https://maven.apache.org/plugins/index.html)，也可以去对应软件（比如Tomcat）的官网，找到"Maven Plugin"栏目就可以了。

Q: 找到之后呢？

A: 只需要**复制一段配置代码**放到自己项目的pom.xml就可以了。官网一般会有指引，并给出各个配置选项的详细说明。

## 7. Maven仓库的基本概念OK

#### 7.1 本地仓库与远程仓库

对于Maven来说， 仓库只分为两类： **本地仓库**和**远程仓库**。

当Maven根据坐标寻找构件的时候，它首先会查看本地仓库，如果本地仓库存在则直接使用；如果不存在就去远程仓库找，找到之后先下载到本地仓库，然后再从本地仓库取用。（ 当然，如果本地仓库和远程仓库都没有，Maven就会报错）

> 什么是构件？
>
> 菜鸟教程如是说：“在 Maven 中，任何一个依赖、插件或者项目构建的输出，都可以称之为构件。”
>
> 简单来说，就是你这个项目的所需要用到的各种外部的依赖、插件、项目等等，比如写JDBC时需要用到的mysql-connector-java-8.0.26.jar，使用Druid时需要用到的druid-1.2.8.jar。

默认情况下，不管Linux还是Windows，每个用户在自己的用户目录下都有一个路径名为.m2/repository/的本地仓库目录。当然你可以通过settings.xml文件修改本地仓库的位置，详见<a href="#4.1">4.1 修改settings.xml</a>。

远程仓库又分为三种： **中央仓库，私服， 第三方公共库**。

#### 7.2 中央仓库

由于原始的本地仓库是空的，Maven必须知道至少一个可用的远程仓库，才能在执行Maven命令的时候下载到需要的构件。中央仓库就是这样一个默认的远程仓库。

Maven中央仓库是由Maven社区提供的仓库，其中包含了大量常用的库。一般来说，简单的Java项目依赖的构件都可以在这里下载到。

但是对于国人来说，由于国外服务器的访问速度较慢，通常不建议直接从中央仓库下载。

#### 7.3 私服

为了节省带宽和时间，可以在局域网内架设一个私有的仓库服务器，用其代理所有外部的远程仓库，这就是私服（仓库）。

私服是一种特殊的远程仓库，它是架设在局域网内的仓库服务，供局域网内的Maven用户使用，相当于这个局域网的“本地仓库”。当Maven需要下载构件时，它会先去私服中找，如果找不到， 则从外部远程仓库下载并缓存在私服上，再提供给Maven。

此外，一些无法从外部仓库下载，或者说没有被上传到外部仓库的构件，也能从本地上传到私服，提供给局域网中的其他Maven用户，比如公司内部开发使用的项目等等。

#### 7.4 第三方公共库

比如常见的阿里云公共仓库，用它作为下载源，速度更快更稳定。

有两种配置方法，一种是像<a href="#4.1">4.1 修改settings.xml</a>，照着视频教程去配置，另一种是照着[阿里云公共仓库的官网](https://developer.aliyun.com/mvn/guide)的使用指南去配置。

内容稍有不同，我也不打算深究，能用就行。

#### 7.5 如何获取构件信息

假设我知道自己需要一个tomcat插件，但是我不知道它具体的坐标（groupId、artifactId、version），咋办？

直接到下面这个网站查就好，它会直接给出一段配置代码让你复制，简直是把饭喂到嘴边了，伸手党的福音。

[Maven Repository: Search/Browse/Explore](https://mvnrepository.com/)





---

##### 1.3.1.1 三个重要属性的含义

其实IDEA在创建Maven项目时早已一语道破天机，看图：<a href="#设定项目的GroupId和ArtifactId">设定项目的GroupId和ArtifactId</a>

**groupId**

定义了当前项目属于哪个“组”，这个“组”的名称往往与项目隶属的组织或公司名称有关，并且其命名通常是该组织或公司的域名反写。必须定义。

**artifactId**

定义了当前项目在“组”中唯一的Id。当前项目可能是“大项目”中的一个“小项目”或模块（由于Maven中模块的概念，一个实际项目往往会被分成很多模块），推荐使用“大项目”的名称作为artifactId的前缀。必须定义。

**version**

当前项目所处的版本。必须定义。

> 其格式通常为：x.x.x-里程碑（了解即可，不必深究）。
>
> * 第一个x：大版本，有重大变革
> * 第二个x：小版本，修复bug或增加功能
> * 第三个x：小修改或小更新
> * 里程碑：
>   * SNAPSHOT：快照，开发版
>   * alpha：内部测试
>   * beta：公开测试
>   * Release | RC：发布版
>   * GA：正常版本