# Java注解与反射

基于教程：

[【狂神说Java】注解和反射_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1p4411P7V3?from=search&seid=9700813218872012494&spm_id_from=333.337.0.0)

参考资料：

[怎样理解 Java 注解和运用注解编程？_tzllxya的博客-CSDN博客](https://blog.csdn.net/tzllxya/article/details/90269712)

[JAVA 注解的基本原理 - Single_Yam - 博客园](https://www.cnblogs.com/yangming1996/p/9295168.html)

《Java 8 高级应用与开发》（清华大学出版社，2016）第7章7.1-7.4

[通俗易懂的双亲委派机制_吴延宝-CSDN博客_双亲委派机制](https://blog.csdn.net/codeyanbao/article/details/82875064)

前言：

注解和反射的知识虽然属于JavaSE，但是非常重要，因为**注解是所有框架的底层！**比如Spring、SpringMVC、SpringBoot等框架中就充满了注解。

## Part 1  注解(Annotation)

### 1.1 什么是注解？

注解(Annotaion)也叫元数据(Metadata)，是jdk5.0引进的技术。

当然，在Java的后续版本中，不仅对原有的注解进行了改进，也增加了一些新的注解。比如Java 7新增了内置注解@SafeVarargs，Java 8新增了内置注解@FunctionalInterface，Java 8新增了元注解@Repeatable，Java 9改进了元注解@Deprecated，...

> 学习注解**一定一定要看源码！**Java的内置注解和元注解就那么几个，代码量极少。学习完自定义注解后（看得懂源码的语法），看源码就可以搞清楚很多问题！
>
> 给新手的温馨提醒：在IDE中按住Ctrl+鼠标单击注解，可以查看该注解的源码

##### 注解的作用

如果说注释是写给人看的，那么注解就是写给程序看的。它更像一个标签，贴在一个类、一个方法或者字段上。

它的目的是为当前读取该注解的程序提供判断依据，比如程序只要读到加了@Test的方法，就知道该方法是待测试方法。

##### 注解的级别

注解和类、接口、枚举是同一级别的。

### 1.2 内置注解

内置注解都位于java.lang包中，比如：

**@Override 重写**

**@Deprecated 废弃的，不建议使用的**

**@SuppressWarning 抑制警告**

**...**

### 1.3 元注解

元注解(Meta-Annotation)用于注解其他注解。

元注解都位于java.lang.annotation包中。

#### @Documented

对于被@Documented元注解标记的注解，javadoc工具会将此注解的注解信息包含在生成javadoc中。

应用详解：[@Document元注解的使用 - 春晨 - 博客园](https://www.cnblogs.com/springmorning/p/10261472.html)

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Documented {
}
```

#### @inherited

1. 没有被@Inherited元注解标记的注解，就不具有继承性，在子类中获取不到父类的该注解；
2. @Inherited的继承概念跟我们理解的面向对象继承概念不一样，它只作用于【子类与父类之间的继承】，而对于【接口之间的继承】和【类与接口之间的实现】这两种继承关系不起作用。
3. 被@Inherited元注解标记的注解在使用时，如果父类和子类都使用该注解（赋予不同参数值），那么子类的注解（的参数值）会覆盖父类的注解（的参数值）。

应用详解：[@Inherited元注解的使用 - 春晨 - 博客园](https://www.cnblogs.com/springmorning/p/10268727.html)

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Inherited {
}
```

#### @Target

描述注解的使用范围（请参照枚举类ElementType）

* ElementType.TYPE：允许被标记的注解作用在类、接口（包括注解）和枚举上
* ElementType.METHOD：允许被标记的注解作用在方法上
* ...

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
    ElementType[] value();
}
```

#### @Retention

描述注解的生命周期（请参照枚举类RetentionPolicy）

* RetentionPolicy.SOURCE：注解只保留在源文件，当.java文件编译成.class文件的时候，注解被遗弃。
* RetentionPolicy.CLASS：注解被保留到.class文件，当JVM加载.class文件时被遗弃，这是默认的生命周期。
* RetentionPolicy.RUNTIME：注解不仅被保存到.class文件中，在JVM加载.class文件之后，注解仍然存在，**可以通过反射获取**。

![](https://pic4.zhimg.com/80/v2-d8c81ca3bcf6b35370d45e0035da1f69_hd.jpg)

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
    RetentionPolicy value();
}
```

### 1.4 自定义注解

#### 如何声明注解

* @interface用来声明一个注解
* 其中的每一个“方法”其实是声明了一个配置参数，“方法”的名称就是参数的名称，“返回类型”就是参数的类型
* 参数类型只能是
  * 基本数据类型
  * String
  * Class
  * 枚举类型
  * 注解类型
  * 以上类型的一维数组
* 可以通过default关键字来声明参数的默认值

#### 如何使用注解

* 有默认值的参数在使用时可以不赋值
* 如果只有一个参数，并且叫value，那么使用该注解时，可以不指定参数名，因为默认就是给value参数赋值
* 如果有多个参数，无论是否叫value，都必须写明参数值与参数名的对应关系
* 如果数组的元素只有一个，可以省略花括号{}

MyAnnotaion.java

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
public @interface MyAnnotation {
    String myName() default "Matty";
    int myAge();
}
```

Test.java

```java
public class Test {
    @MyAnnotaion(myName = "Wu Hang", myAge = 19)
    public void method1() {}
}
```

### 1.5 注解的本质

#### 线索

* @interface和interface很相似

* 接口java.lang.annotatio.Annotation的注释中有一句话，用来描述注解：

  > **The common interface extended by all annotation types.** Note that an interface that manually extends this one does not define an annotation type. Also note that this interface does not itself define an annotation type.
  >
  > 所有的注解类型都继承自这个接口。注意，一个手动继承这个接口的接口并不会定义一个注解类型。也要注意，这个接口本身并不定义一个注解类型。

* 枚举类常量ElementType.TYPE的注释是这样写的：

  > Class, **interface (including annotation type)**, or enum declaration
  >
  > 类，接口（包括注解类型），或枚举类型

#### 猜想

注解的本质与接口有关。

#### 验证[^疑惑1]

先看测试代码：

```java
Class c1 = Comparable.class;
Class c2 = Override.class;
System.out.println(c1);			//输出结果：interface java.lang.Comparable
System.out.println(c2);			//输出结果：interface java.lang.Override
```

输出结果：

```
interface java.lang.Comparable
interface java.lang.Override
```

我们再看一下Class类的源码：

```java
public String toString() {
    return (isInterface() ? "interface " : (isPrimitive() ? "" : "class "))
        + getName();
}
```

再回到测试代码加上：

```java
System.out.println(c2.isInterface());
```

输出结果：

```
true
```

#### 结论

注解的本质是一种特殊的接口，而它的参数类型和参数名实际上是接口的方法返回类型和方法名。（当然注解使用起来和接口还是有很大区别的

## Part 2 反射(Reflection)
### 2.1 反射概述

#### 动态语言vs静态语言

动态语言，就是在运行时可以改变其结构的语言。例如增加新的函数、对象，甚至引进外部的代码，删除或修改已有的函数。比如：在游戏程序运行过程中，启动外挂程序，改变游戏程序的代码结构。

主要动态语言：Object-C、C#、JavaScript、PHP、Python

与动态语言相对的，在运行时结构不可变的语言就是静态语言。

主要静态语言：Java、C、C++

#### 反射机制

Java虽然是静态语言，但却拥有一定的动态性，因为我们可以利用反射机制获得类似动态语言的特性。

反射机制允许程序在执行期借助Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性和方法。

* 优点：可以实现动态创建对象和编译，体现出很大的灵活性。

* 缺点：对性能有影响。使用反射基本上是一种解释的操作，我们可以告诉JVM，我们希望做什么，JVM会满足我们的要求。这类操作总是慢于直接执行相应的操作。

#### Java反射机制提供的功能

* 在运行时判断任意一个对象所属的类

* 在运行时构造任意一个类的对象

* 在运行时判断任意一个类所具有的成员变量和方法

* 在运行时调用任意一个对象的的成员变量和方法

* 在运行时获取泛型信息

* 在运行时处理注解

* 生成**动态代理**

  > AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期间**动态代理**实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容
  >
  > 参考资料：[AOP（面向切面编程）_百度百科](https://baike.baidu.com/item/AOP/1332219?fr=aladdin)

* ...

### 2.2 获得反射对象

#### 反射相关的主要API

* java.lang.Class

* java.lang.reflect.Method

* java.lang.reflect.Field

* java.lang.reflect.Constructor
* ...

#### Class类

Class类是反射的根源，对于任何你想动态加载、运行的类，必须获得相应的Class对象。

Class类没有公共构造方法，其对象是JVM在加载类时通过调用类加载器中的`defineClass()`方法创建的，因此不能显式地实例化一个Class对象。

每个类被加载之后，JVM都会为该类生成一个对应的Class对象，类的整个结构都会被封装到Class对象里，通过它就可以访问到JVM中该类的信息。一旦类被载入JVM，同一个类将不会再次被载入。

一个Class对象对应的是一个加载到JVM中的.class字节码文件。

#### 如何获取Class对象

1. 已知某个类，通过类的Class属性获取。该方法最为安全可靠，程序性能最好，推荐使用。

   ```java
   Class c = String.class;
   ```

2. 已知某个实例，通过该实例的getClass()方法获取。

   ```java
   String str = new String();
   Class c = str.getClass();
   ```

3. 已知全类名，通过Class类的静态方法forName()获取，可能抛出ClassNotFoundException。

   ```java
   Class c = Class.forName("java.lang.String");
   ```

4. 基本数据类型可以直接用包装类的类名.Type（注意String不是基本类型，它的底层由char[]实现）

   ```java
   Class c = Integer.Type;
   ```

5. 还可以通过ClassLoader获取，这个之后再讲

#### 哪些类型可以有Class对象

* 类：外部类，成员类（成员内部类，静态内部类），局部内部类，匿名内部类
* 接口
* 注解
* 枚举
* 任意类型、任意维度的数组
* 基本数据类型
* void

```java
Class c1 = Object.class;			//输出结果：class java.lang.Object
Class c2 = Comparable.class;		//输出结果：interface java.lang.Comparable
Class c3 = Override.class;			//输出结果：interface java.lang.Override
Class c4 = int[].class;				//输出结果：class [I
Class c5 = String[].class;			//输出结果：class [Ljava.lang.String;
Class c6 = String[][].class;		//输出结果：class [[Ljava.lang.String;
Class c7 = ElementType.class;		//输出结果：class java.lang.annotation.ElementType
Class c8 = int.class;				//输出结果：int
Class c9 = void.class;				//输出结果：void
```

### 2.3 类加载机制

当程序主动使用某个类，而该类还未被加载到内存中时，则系统会通过以下三个步骤来对该类进行初始化。如果没有意外出现，JVM会连续完成这三个步骤。

> 这一小节涉及到很多JVM的知识，因此只作简单解释，不必深究。如果想了解更深的话，需要专门学习JVM这一块的内容。
>
> 在开发过程中无须过分关心类加载机制，只需对其有所了解即可。

#### 1. 类的加载(Loading)

**类加载器**将类的.class文件读入内存，并为之创建一个Class对象。

*JVM提供的类加载器被称为系统类加载器，除此之外，我们可以通过继承ClassLoader类来创建自己的类加载器。*

#### 2. 类的链接(Linking)

将类的二进制数据合并到JRE中。类的链接又分三个阶段：

1）验证(Verification)阶段——确保加载的类信息符合JVM规范，没有安全方面的问题。

2）准备(Preparation)阶段——为类变量（static）分配内存，并设置默认初始值。

> 类变量(class variable)、静态字段(static field)、静态变量(staic variable)指的都是同一个东西，只是叫法不同。
>
> 详见：[Java 中 field 和 variable 区别及相关术语解释](https://www.ituring.com.cn/article/491755)

3）解析(Resolution)阶段——将JVM常量池中的符号引用（常量名）替换成直接引用（地址）。

#### 3. 类的初始化(Initialization)

对类变量进行初始化。类的初始化又分三个步骤：

1）如果类没有被加载和链接，则程序先加载并链接该类。

2）初始化一个类时，如果该类的直接父类未被初始化，则先初始化其直接父类。JVM会保证该类及其所有祖先类都被初始化。

3）如果类中有初始化语句，则直接执行初始化语句。[^疑惑2]

**什么时候会发生类的初始化？**

以下情况属于**类的主动引用：【一定会发生类的初始化】**

* 当JVM启动时，先初始化main方法所在的类
* new一个类
* 调用类变量（static）。但如果调用的是类常量（static final），则要看这个常量的值是否能在编译期确定，详见文章：[类初始化阶段详解_拿笔小星的博客-CSDN博客](https://blog.csdn.net/u013096088/article/details/79439482)
* 调用静态方法（static）[^疑惑3]
* 使用java.lang.reflection包中的方法对类进行反射调用
* 初始化一个类时，如果该类的直接父类未被初始化，则先初始化其直接父类。

以下情况属于**类的被动引用：【不会发生类的初始化】**

* 通过子类访问父类的类变量时，该子类不会被初始化，只有该父类会被初始化，即只有真正声明这个类变量的类才会被初始化。
* 声明对象数组，不会触发对应类的初始化。

* 访问类常量，不会触发对应类的初始化。

### 2.4 类加载器(ClassLoader)

讲类加载器，其实也就是在讲类加载机制中类的加载(Loading)。

#### 类加载器的作用

类加载器负责将磁盘或网络上的.class文件读入内存中，并为之创建一个Class对象。

#### 类加载器有哪些

来看下面这段：

> A Java Classloader is of **three types**:
>
> 1. **BootStrap ClassLoader:** A Bootstrap Classloader is a Machine code which kickstarts the operation when the JVM calls it. <u>It is not a java class.</u> Its job is to load the first pure Java ClassLoader. Bootstrap ClassLoader loads classes from the location ***rt.jar***. Bootstrap ClassLoader doesn’t have any parent ClassLoaders. It is also called as the **Primodial ClassLoader**.
>
> 2. **Extension ClassLoader:** The Extension ClassLoader is a child of Bootstrap ClassLoader and loads the extensions of core java classes from the respective JDK Extension library. It loads files from ***jre/lib/ext*** directory or any other directory pointed by the system property ***java.ext.dirs***.
>
> 3. **System ClassLoader:** An Application ClassLoader is also known as a System ClassLoader. It loads the Application type classes found in the environment variable ***CLASSPATH, -classpath or -cp command line option***. The Application ClassLoader is a child class of Extension ClassLoader.
>
> 摘自：[ClassLoader in Java - GeeksforGeeks](https://www.geeksforgeeks.org/classloader-in-java/#:~:text=BootStrap%20ClassLoader%3A%20A%20Bootstrap%20Classloader,when%20the%20JVM%20calls%20it.&text=Its%20job%20is%20to%20load,jar.)

直接上代码：

```java
Class c1 = Class.forName("java.lang.Object");
ClassLoader l1 = c1.getClassLoader();					//BootStrap ClassLoader，
Class c2 = Class.forName("com.mysql.cj.jdbc.Driver");
ClassLoader l2 = c2.getClassLoader();					//Extension ClassLoader，对应类ExtClassLoader
Class c3 = Class.forName("chapter04.stack.Stack");
ClassLoader l3 = c3.getClassLoader();					//System ClassLoader，对应类AppClassLoader
System.out.println(l1);
System.out.println(l2);
System.out.println(l3);
```

输出结果：

```
null
sun.misc.Launcher$ExtClassLoader@610455d6
sun.misc.Launcher$AppClassLoader@18b4aac2
```

注意一下两个地方：

* 启动类加载器是JVM自带的类加载器，它不是一个Java类，无法直接获取，因此getClassLoader()方法返回null。

* Launcher$AppClassLoader的意思是，Laucher类的内部类AppClassLoader。

### 2.5 双亲委派机制

#### 双亲委派的原理

如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委托给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中。
只有当父类加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需要加载的类）时，子加载器才会尝试自己去加载。这种从下往上委托，再从上向下加载的过程叫作双亲委派机制。

> 为什么是“双”亲委派？简单来说这就是个翻译问题。如果换成“父辈代理机制”或是“上溯委托机制”就比较准确了。

<img src="https://upload-images.jianshu.io/upload_images/7634245-7b7882e1f4ea5d7d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" title="双亲委派机制的流程图" style="zoom:80%;" />

### 2.6



[^疑惑1]: 网上很多资料的验证方式是这样的：`public @interface Annotation`经过编译-->反编译后会变成`public interface Annotation extends annotation`，`String value()`经过编译-->反编译后会变成`public abstract String value()`。但是实事求是地说，我自己无论用jd-gui (JD-core ver1.1.3) 还是XJad (Jad ver1.5.8e2) ，反编译得到的结果都和一开始的源文件没有区别，根本无法复现这种实验。
[^疑惑2]:实际上我没看懂这句话，查了资料也还是没搞懂。
[^疑惑3]:我们知道，通过类名调用静态方法时，类的构造方法并不会被执行，那么此时该类是否已被初始化？如果是，那么是否可以理解为：初始化一个类不一定要调用其构造方法，但是调用一个类的构造方法一定导致该类的初始化？