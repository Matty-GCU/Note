

[^疑惑1]: 事实上我没有理解这句话，但还是记录下来。我认为就算要这么说，也应该是Lambda表达式能够被隐式地转换。

# Java8 Lambda表达式

基于：

[Java8 Lambda表达式视频教程(无废话版)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ci4y1g7qD?p=1)

## Lambda表达式介绍

Java 8引入的新特性。使用它可以使代码变得更简洁。

通过Lambda表达式，可以代替我们以前经常写的匿名内部类来实现接口。

Lambda表达式的本质是一个匿名函数。

```java
(参数列表) -> {
方法体
};
```

`->`是Lambda运算符，英文名是`goes to`

## Lambda表达式语法

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

看以上代码已经能了解个大概了。

不过细心的你已经发现，

`I01 i01 = () -> {System.out.println(...);};`

的形式，不就和**匿名类实现接口**如出一辙吗？

等等，好像哪里不对......Lambda表达式的本质不是匿名函数吗？怎么就把匿名函数当成匿名类用了？这就要提到函数式接口。

## 函数式接口(Functional Interface)

Java 8为了使现有的函数更加友好地支持Lambda表达式，引入了函数式接口的概念。

函数式接口本质上是一个仅有一个抽象方法的普通接口。

函数式接口能够被隐式地转换为Lambda表达式。[^疑惑1]

函数式接口在实际使用过程中很容易出错，比如某人在接口定义中又增加了另一个方法，则该接口不再是函数式接口，此时将该接口转换为Lambda表达式

## Lambda表达式精简语法

* 参数类型可以省略

  比如`(int a, int b) -> {System.out.println(...);};`可以写成`(int a, int b) -> {System.out.println(...);};`