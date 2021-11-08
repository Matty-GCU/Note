# Java8 Lambda表达式

基于：

视频版（同一作者）：[Java8 Lambda表达式视频教程(无废话版)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ci4y1g7qD?p=1)

文字版（同一作者）：[一文搞懂Java8 Lambda表达式(附视频教程)](https://mp.weixin.qq.com/s/QsKs7wriZH8f1jk__DbW4Q)

《Java 8 高级应用与开发》（清华大学出版社，2016）第8章8.1-8.4

参考：

[在Java代码中写Lambda表达式是种怎样的体验？ - Java3y的回答 - 知乎](https://www.zhihu.com/question/37872003/answer/1009015660)

## 1. Lambda表达式介绍

Lambda 表达式是Java 8引入的重要新特性。

Lambda 表达式简化了匿名内部类的形式，并且可以达到同样的效果（当然Lambda 要优雅得多）。虽然最终达到的效果是一样的，但其底层实现原理却并不相同。匿名内部类在编译之后会创建一个新的匿名内部类出来，而 Lambda 是调用 JVM invokedynamic指令实现的，并不会产生新类。

Lambda表达式返回的是**接口对象实例**。

其基本语法如下：

```java
(参数列表) -> {
方法体
};
```

`->`是Lambda运算符，英文名是`goes to`

## 2. Lambda表达式语法

总共6种情况，接口方法**无返回值**和**有返回值**分2种，其中**无参数**、**单个参数**和**多个参数**又分3种情况。

来看示例代码：

```java
public class Test {
    public static void main(String[] args) {
        I01 i01 = () -> {
            System.out.println("无返回值、无参数");
        };
        I02 i02 = (int a) -> {
            System.out.println("无返回值，单个参数。a=" + a);
        };
        I03 i03 = (int a, int b) -> {
            System.out.println("无返回值，多个参数。a=" + a + ",b=" + b);
        };
        I04 i04 = () -> {
            System.out.println("有返回值、无参数");
            return 4;
        };
        I05 i05 = (int a) -> {
            System.out.println("有返回值，单个参数。a=" + a);
            return 5;
        };
        I06 i06 = (int a, int b) -> {
            System.out.println("有返回值，多个参数。a=" + a + ",b=" + b);
            return 6;
        };
        i01.method();
        i02.method(5);
        i03.method(5,10);
        System.out.println(i04.method());
        System.out.println(i05.method(5));
        System.out.println(i06.method(5, 10));
    }
}

interface I01 {
    void method();
}

interface I02 {
    void method(int a);
}

interface I03 {
    void method(int a, int b);
}

interface I04 {
    int method();
}

interface I05 {
    int method(int a);
}

interface I06 {
    int method(int a, int b);
}
```

输出：

```
无返回值、无参数
无返回值，单个参数。a=5
无返回值，多个参数。a=5,b=10
有返回值、无参数
4
有返回值，单个参数。a=5
5
有返回值，多个参数。a=5,b=10
6
```

看完以上代码已经能了解个大概了。

不过细心的你已经发现，要是这些接口里面定义的方法不止一个呢？那岂不是无法确定Lambda表达式究竟是要实现哪一个接口方法？这里就要提到函数式接口的概念——

## 3. 函数式接口(Functional Interface)

Java 8为了使现有的函数更加友好地支持Lambda表达式，引入了函数式接口的概念。

函数式接口本质上是一个仅有一个抽象方法的普通接口，所以又叫SAM接口(Single Abstract Method Interface)。

函数式接口在实际使用过程中很容易出错，比如某人在接口定义中又增加了另一个方法，则该接口不再是函数式接口，此时将该接口转换为Lambda表达式会报错。为了克服函数式接口的脆弱性，并且能够明确声明接口是作为函数式接口的意图，Java 8增加了**@FunctionalInterface**注解来标注函数式接口。

使用@FunctionalInterface注解标注的接口必须是函数式接口，也就是说该接口中只能声明一个抽象方法，如果声明多个抽象方法就会报错。但是默认方法和静态方法不属于抽象方法，因此在函数式接口中也可以定义默认方法和静态方法。

比如这样声明一个函数式接口是被允许的：

```java
@FunctionalInterface
interface InterfaceDemo {
    void method(int a);
    static void staticMethod() {
        ...
    }
    default void defaultMethod() {
        ...
    }
}
```

@FunctionalInterface注解不是必须的，如果一个接口符合"函数式接口"的定义，那么加不加该注解都没有影响。当然加上该注解能够更好地让编译器进行检查，也能提高代码的可读性。

## 4. Lambda表达式精简语法

1. 参数类型可以省略

   比如`I02 i02 = (int a) -> {System.out.println(...);};`可以写成`I02 i02 = (a) -> {System.out.println(...);};`

2. 假如只有一个参数，那么`()`括号可以省略

   比如`I02 i02 = (a) -> {System.out.println(...);};`可以写成`I02 i02 = a -> {System.out.println(...);};`

3. 假如方法体只有一条语句，那么语句后的`;`分号和方法体的`{}`大括号可以一起省略

   比如`I02 i02 = a -> {System.out.println(...);};`可以写成`I02 i02 = a -> System.out.println(...);`

4. 如果方法体中唯一的语句是return返回语句，那么在省略第3种情况的同时，`return`也必须一起省略

   比如`I05 i05 = a -> {return 1;};`可以写成``I05 i05 = a -> 1;`

## 5. 方法引用(Method Reference)

在Java 8中可以用方法引用来进一步简化Lambda表达式。（虽然两者在底层实现原理上略有不同，但在实际使用中完全可以视为等价）

有时候多个Lambda表达式的实现函数是一样的，我们可以封装成一个通用方法，再通过方法引用来实现接口。

#### 5.1 方法引用语法

* 如果是实例方法：`对象名::实例方法名`
* 如果是静态方法：`类名::实例方法名`

* 如果是构造方法：`类名::new`

#### 5.1实例方法引用

来看示例代码：

```java
public class Test {

    public void eat(int a) {
        System.out.println("吃东西。" + "a=" + a);
    }

    public static void main(String[] args) {
        //Lambda表达式写法：
        Dog dog1 = (a) -> System.out.println("吃东西。" + "a=" + a);
        Cat cat1 = (a) -> System.out.println("吃东西。" + "a=" + a);
        dog1.doSomething(5);
        cat1.doSomething(5);
        //方法引用写法：
        Test test = new Test();
        Dog dog2 = test::eat;
        Cat cat2 = test::eat;
        dog2.doSomething(10);
        cat2.doSomething(10);
    }
}

@FunctionalInterface
interface Dog {
    void doSomething(int a);
}

@FunctionalInterface
interface Cat {
    void doSomething(int a);
}
```

输出结果：

```
吃东西。a=5
吃东西。a=5
吃东西。a=10
吃东西。a=10
```

#### 5.2 构造方法引用

如果函数式接口的实现恰好可以通过调用一个类的构造方法来实现（比如说接口方法与这个构造方法的参数个数、参数类型和返回值都对的上），那么就可以使用构造方法引用。

上代码！

```java
public class Test {

    public void eat(int a) {
        System.out.println("吃东西。" + "a=" + a);
    }

    public static void main(String[] args) {
		//Lambda表达式写法：
        DogService dogService1 = (name, age) -> new Dog(name, age);
        System.out.println(dogService1.getDog("大狗", 5));
        //方法引用写法：
        DogService dogService2 = Dog::new;
        System.out.println(dogService2.getDog("二狗", 3));
    }
}

@FunctionalInterface
interface DogService {
    Dog getDog(String name, int age);
}

class Dog {

    String name;
    int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

看结果：

```
Dog{name='大狗', age=5}Dog{name='二狗', age=3}
```

## 6. 内置函数式接口

java.util.function包下有一系列内置函数式接口，也是Java 8伴随Lambda表达式一起推出的。

* 关于内置函数式接口可以看看这篇文章的第二部分：[JAVA8新特性-Lambda表达式、函数式接口以及方法引用_sout-CSDN博客](https://blog.csdn.net/m2606707610/article/details/83754507)

## 7. 写在最后

其实有了上面的基础之后，看源码就变得非常简单了，毕竟一个函数式接口里面也就一个抽象方法，有问题多看源码即可。

---

2021.11.08