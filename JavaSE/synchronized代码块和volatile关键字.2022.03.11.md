# Java单例模式实现中的synchronized代码块和volatile关键字

## [用不用 synchronized 的区别？](https://mp.weixin.qq.com/s/-fv3uDIf9zADtU0Vue8SsA)

> 作者小结：
>
> 1. 当 5 个线程中只有一个对象时，5 个线程之间的执行顺序是串行的，互不影响，线程0进入方法后，其他线程就无法再进入方法了，得等待线程0执行完后，其他线程才能进入，并且一次只能有1个线程进入。
> 2. 每个线程一个对象时，就不会出现排队的场景，各个线程互不影响，相当于每个线程都有各自的资源，没有互相竞争的关系。
>
> 由此，我们可以得出什么呢？**「synchronized」** 关键字就是**「悲观锁」**的具体实现，在多线程并发竞争同一资源时，实现同一时刻只有一个线程能操作资源。

synchronized入门。

**多个线程无法同时调用同一个实例的任意synchronized实例方法。**

事实上，每个线程争夺的资源（synchronized锁住的）不是这个实例方法，而是这个实例本身。

* 假如线程A正在调用某个实例的`synchronized实例方法1`，那么即使线程B想调用的是同一实例的`synchronized实例方法2`，也不行，要等。
* 如果给每个线程都分配一个实例，那各个线程之间就不存在需要竞争的资源了。

## [synchronized 作为悲观锁，锁住了什么？](https://mp.weixin.qq.com/s/VlciD2XnHtsG3BC-z6K7ZA)

> 作者小结：
>
> 这一篇我们讲了 **「synchronized」** 修饰方法时的 2 种锁机制：锁实例对象和锁类的 Class 对象。从锁的**「粗粒度」**来对比，锁类 Class 对象的粒度大于锁实例对象。

**synchronized修饰实例方法，锁的是实例对象；修饰类方法（静态方法）时，锁的是Class对象。**

所以前面那句加粗的话，把“实例”改成“类”也同样成立：**多个线程无法同时调用同一个类（同一Class对象）的任意synchronized类方法（静态方法）。**

作者提到粗粒度的问题，我猜想并且验证过，确实，多个线程也无法同时调用同一个实例的任意synchronized类方法——虽然这样不规范，应该直接通过类来调用——所以锁类Class对象的粒度确实更大。

## [synchronized 代码块怎么用](https://mp.weixin.qq.com/s/pdw74vR-SSlH_1gFzpIdMg)

> 作者小结：
>
> 这篇介绍了**「synchronizd 代码块」**的 3 种使用方式，并详细介绍了各自的使用方式和区别。简单的列个表。
>
> |   类型    |            使用方式             |         锁作用范围         |
> | :-------: | :-----------------------------: | :------------------------: |
> |   this    |      synchronized(this){}       |     锁住当前的实例对象     |
> |  object   | synchronized(anotherInstance){} | 锁住其他实例对象，比较灵活 |
> | xxx.class |    synchronized(xxx.class){}    |      锁住 Class 对象       |

使用synchronized代码块，可以更加灵活地选择同步代码，再也不用非得同步整个方法了，还有就是不仅可以选择锁当前实例对象还是当前类（Class对象），还可以选择锁其他实例对象和其他类（Class对象）。

在懒汉式单例类中，我们使用的双重检查锁定，就锁了该单例类的Class对象。

```java
package design.pattern.singleton;

public class LazySingleton {

    private volatile static LazySingleton instance;
    
    private LazySingleton() {}

    public static LazySingleton getInstance3() {
        //Double-Check Locking
        if (instance == null) {
            synchronized (LazySingleton.class) {
                if (instance == null) {
                    instance = new LazySingleton();
                }
            }
        }
        return instance;
    }
    
}
```

## [Java中Volatile关键字详解](https://www.cnblogs.com/zhengbin/p/5654805.html)

> ...
>
> **可见性，是指线程之间的可见性，一个线程修改的状态对另一个线程是可见的。**也就是一个线程修改的结果。另一个线程马上就能看到。比如：用volatile修饰的变量，就会具有可见性。volatile修饰的变量不允许线程内部缓存和重排序，即直接修改内存。所以对其他线程是可见的。
>
> ...
>
> ### 当一个变量定义为 volatile 之后，将具备两种特性：
>
> 　　1.保证此变量对所有的线程的可见性，这里的“可见性”，如本文开头所述，当一个线程修改了这个变量的值，volatile 保证了新值能立即同步到主内存，以及每次使用前立即从主内存刷新。但普通变量做不到这点，普通变量的值在线程间传递均需要通过主内存（详见：[Java内存模型](http://www.cnblogs.com/zhengbin/p/6407137.html)）来完成。
>
> 　　2.禁止指令重排序优化。有volatile修饰的变量，赋值后多执行了一个“load addl $0x0, (%esp)”操作，这个操作相当于一个**内存屏障**（指令重排序时不能把后面的指令重排序到内存屏障之前的位置），只有一个CPU访问内存时，并不需要内存屏障；（什么是指令重排序：是指CPU采用了允许将多条指令不按程序规定的顺序分开发送给各相应电路单元处理）。
>
> ### volatile 性能：
>
> 　　volatile 的读性能消耗与普通变量几乎相同，但是写操作稍慢，因为它需要在本地代码中插入许多内存屏障指令来保证处理器不发生乱序执行。

我的简单理解是：

**使用volatile修饰的变量，在被多个线程使用的时候，总能更新到最新值。**

因为它不会被拷贝到每个线程内部的缓存中，而是一直放在内存。无论哪个线程，每次使用这个变量都是从内存拿，修改也是直接修改内存中的值。

至于“禁止指令重排序”这点，涉及很多JVM底层的东西，暂时先不管。

2022.03.11