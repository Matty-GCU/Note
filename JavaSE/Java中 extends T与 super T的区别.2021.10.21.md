# Java中<? extends T>与<? super T>的区别

参考资料：[Java 泛型 <? super T> 中 super 怎么 理解？与 extends 有何不同？ - 胖君的回答 - 知乎](https://www.zhihu.com/question/20400700/answer/117464182)

问题引入：

```java
// compile error >>> required type: capture of ? extends Fruit
// List <? extends Fruit> plate1 = new ArrayList();
// plate1.add(new Fruit());
// plate1.add(new Apple());
// plate1.add(new RedApple());

List <? super Fruit> plate2 = new ArrayList();
plate2.add(new Fruit());
plate2.add(new Apple());
plate2.add(new RedApple());
```

### <? extends T>

**<? extends T>是 “上界通配符（Upper Bounds Wildcards）”**，确定了上界，也就是说不存在下界，即plate1的元素类型有可能是任意一种T的子类或T本身。最关键的地方在于，哪怕把上面的第2行代码

```java
//实际上new ArrayList()里的元素类型是无法确定的（任意的）
List <? extends Fruit> plate1 = new ArrayList();
```

改成

```java
//实际上new ArrayList<Fruit>()里的元素类型确定是Fruit
List <? extends Fruit> plate1 = new ArrayList<Fruit>();
```

根据声明**(Q1)**，Java编译器也只知道plate1里放的是Fruit的子类或Fruit本身的对象，但具体是什么类型不知道，可能是Fruit？可能是Apple？可能是RedApple？显然，

```java
Apple apple = new Fruit();			//compile error
RedApple redApple = new Apple();	//compile error
```

既然如此，那就不可能往plate1里继续添加**(Q2)**任何类型的对象——要知道，plate1的元素类型是“没有下限”的，万一里面已有的元素类型比你要添加的元素类型更“低级”怎么办？！

那可以从plate1中取出元素吗？可以，但是取出来的元素只能赋值给Fruit的父类或Fruit本身（的对象），就是说，有上界无下界的类型（的对象）只能赋值给超过或等于其上界的类型（的对象）。

##### Q1:为什么一开始的第2行代码改了也会报错？既然`new ArrayList<Fruit>()`的元素类型十分确定，那Java编译器为什么不把plate的元素类型也确定下来？

A1:因为编译器无法确定plate会不会被赋予别的值。比如：

```java
List <? extends Fruit> plate1;
if(...) {
	plate1 = new ArrayList<Fruit>();
}
else {
	plate1 = new ArrayList<RedApple>();
}
//编译时期，编译器无法确定程序会进入哪个循环（运行时期才知道），因此只能根据plate的声明来保证plate的类型安全。
plate1.add(新元素);
```

##### Q2:为什么要强调不能“继续”添加？

A2:上面的示例只是示例，在实际使用中，我们可能先声明一个ArrayList\<Fruit\>对象并**添加**很多个元素，然后再声明一个List <? extends Fruit>对象，并把前者赋值给后者，这是完全可以的。

### <? super T>

<? super T>是 **“下界通配符（Lower Bounds Wildcards）”**，也就是说确定了下界，即plate2的元素类型有可能是T的父类或T本身。显然，

```java
Fruit fruit = new Apple();		//comoile OK
Food food = new Apple();		//comoile OK
```

所以往plate2里添加T的子类或T本身（的对象）都是被允许的。

前面说确定了下界，那是不是不存在上界？不是，因为Java中有Object类作为所有类的父类，所以一定存在上界，即Object类。因此，从plate2中取出来的元素只能赋值给Object类（的对象）——要知道，plate2的元素类型下可至Fruit，上可至Object，万一里面已有的元素类型是Object怎么办？！

### 作用

以前看源码的时候有印象，但当时看不太懂。。。下次遇到再回来写

### 小结

总结一下，就是说 `List<? extends/super Fruit> plate`声明了plate这个变量可以指向哪种类型的集合，但不是一种确定的类型，而是一个范围，具体是啥类型，在编译时期没法确定。

Java的设计者为了保证类型安全，设计了这些规则。只要理解了通配符的原理，自然会掌握它的用法。

* 上界<? extends T>不能往里存任何类型，可以往外取，但只能放在T的父类或T本身里
* 下界<? super T>可以往里存T的子类或T本身，也可以往外取，但只能放在Object里

* 口诀：上界不能存，取出必须放上界之上；下界可以存下界之下，取出必须放Object。

最后看一下**PECS（Producer Extends Consumer Super）原则：**

1. 频繁往外读取内容的，适合用上界Extends。

2. 经常往里插入的，适合用下界Super。

---

2021.10.21