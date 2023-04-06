---
title: Java设计模式
date: 2023-04-06 14:37:00
categories: 工作
tags:
- Java 
- 设计模式
---
## 设计模式
### 常用设计模式

#### 单例模式

优点：系统内存中该类只存在一个对象，节省了系统资源，对于一些需要频繁创建销毁的对象，使用单例模式可以提高系统性能。

缺点：当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是使用new，可能会给其他开发人员造成困扰，特别是看不到源码的时候。

使用场景：

• 需要频繁的进行创建和销毁的对象；
• 创建对象时耗时过多或耗费资源过多，但又经常用到的对象；
• 工具类对象；
• 频繁访问数据库或文件的对象。

```
/**
 * 饿汉式（静态常量）【可用】
 * 优点：这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同步问题。
 * 缺点：在类装载的时候就完成实例化，
 * 没有达到Lazy Loading的效果。如果从始至终从未使用过这个实例，则会造成内存的浪费。
 */
private final static Singleton INSTANCE = new Singleton();

private Singleton() {
}

public static Singleton getInstance() {
    return INSTANCE;
}

/**
 * 饿汉式（静态代码块）【可用】
 * 这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块中，
 * 也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。优缺点和上面是一样的。
 */

private static Singleton INSTANCE_TWO;

static {
    INSTANCE_TWO = new Singleton();
}

private Singleton() { }

public static Singleton getInstanceTwo() {
    return INSTANCE_TWO;
}

/**
 * 懒汉式（线程不安全）【不可用】
 * 这种写法起到了Lazy Loading的效果，但是只能在单线程下使用。如果在多线程下，
 * 一个线程进入了if (singleton == null)判断语句块，还未来得及往下执行，另一个线程也通过了这个判断语句，
 * 这时便会产生多个实例。所以在多线程环境下不可使用这种方式。
 */
private static Singleton instanceTree;

private Singleton() { }

public static Singleton getInstanceTree() {
    if (instanceTree == null) {
        instanceTree = new Singleton();

    }
    return instanceTree;
}

/**
 * 懒汉式（线程安全，同步方法）【不推荐用】
 * 解决上面第三种实现方式的线程不安全问题，做个线程同步就可以了，于是就对getInstance()方法进行了线程同步。
 * 缺点：效率太低了，每个线程在想获得类的实例时候，执行getInstance()方法都要进行同步。而其实这个方法只执行一次实例化代码就够了，
 * 后面的想获得该类实例，直接return就行了。方法进行同步效率太低要改进。
 */
private static Singleton instanceTour;

private Singleton() { }

public static synchronized Singleton getInstanceTour() {
    if (instanceTour == null) {
        instanceTour = new Singleton();

    }
    return instanceTour;
}

/**
 * 懒汉式（线程安全，同步代码块）【不可用】
 * 由于第四种实现方式同步效率太低，所以摒弃同步方法，改为同步产生实例化的的代码块。
 * 但是这种同步并不能起到线程同步的作用。跟第3种实现方式遇到的情形一致，假如一个线程进入了if (singleton == null)判断语句块，
 * 还未来得及往下执行，另一个线程也通过了这个判断语句，这时便会产生多个实例
 */
private static Singleton instanceFive;

private Singleton() { }

public static synchronized Singleton getInstanceFive() {
    if (instanceFive == null) {
        synchronized (Singleton.class) {
            instanceFive = new Singleton();
        }
    }
    return instanceFive;
}

/**
 * 双重检查【推荐使用】
 * Double-Check概念对于多线程开发者来说不会陌生，如代码中所示，我们进行了两次if (singleton == null)检查，
 * 这样就可以保证线程安全了。这样，实例化代码只用执行一次，后面再次访问时，判断if (singleton == null)，直接return实例化对象。
 * 优点：线程安全；延迟加载；效率较高。
 */

private static Singleton instanceSix;

private Singleton() { }

public static synchronized  getInstanceSix() {
    if (instanceSix == null) {
        synchronized (Singleton.class) {
            if (instanceSix == null) {
                instanceSix = new Singleton();
            }

        }
    }
    return instanceSix;
}

/**
 * 静态内部类【推荐使用】
 * 这种方式跟饿汉式方式采用的机制类似，但又有不同。两者都是采用了类装载的机制来保证初始化实例时只有一个线程。
 * 不同的地方在饿汉式方式是只要Singleton类被装载就会实例化，没有Lazy-Loading的作用，而静态内部类方式在Singleton类被装载时并不会立即实例化，
 * 而是在需要实例化时，调用getInstance方法，才会装载SingletonInstance类，从而完成Singleton的实例化。
 * 类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮助我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的。
 * 优点：避免了线程不安全，延迟加载，效率高
 */
private Singleton() { }

private static class SingletonInstance {
    private static final Singleton instanceSeven = new Singleton();
}

public static Singleton getInstanceSeven() {
    return SingletonInstance.instanceSeven;
}

/**
 * 枚举【推荐使用】
 * 借助JDK1.5中添加的枚举来实现单例模式。不仅能避免多线程同步问题，
 * 而且还能防止反序列化重新创建新的对象
 */
public enum SingletonTwo {
    INSTANCE;
    public void whateverMethod() {

    }

}
```

#### 策略模式

优点：算法可以自由切换，避免使用多重条件判断，扩展性良好。

缺点：策略类会增多，所有策略类都需要对外暴露。

使用场景：

• 如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为；
• 一个系统需要动态地在几种算法中选择一种；
• 如果一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重的条件选择语句来实现；