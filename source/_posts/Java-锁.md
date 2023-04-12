---
title: Java-锁
date: 2023-04-11 11:25:33
categories: Java
tags:
- Java 
- 锁
---

## volatile
volatile 是 JVM 提供的轻量级的同步机制。volatile 关键字可以保证并发编程三大特征（原子性、可见性、有序性）中的可见性和有序性，不能保证原子性。
1. 可见性
> 保证此变量对所有线程的可见性，即当一个线程修改了这个变量的值，新值对于其它线程来说是可以立即得知的。
> 禁止指令重排序

### volatile实现原理
volatile可见性借助了CPU的lock指令，lock指令在多核处理器下，可以将当前处理器的缓存行的数据写回到系统内存，
其他CPU里缓存了该内存地址的数据置为无效。 通过在写volatile的机器指令前加上lock前缀，使写volatile具有以下原则。
>写volatile时处理器将缓存写回到主机内存。
>一个处理器的缓存写回到内存，会导致其他处理器的缓存失效。

```
/**
 * @program: souk-demo
 * @description:
 * @author: ye jun jun
 * @create: 2023-04-11 13:39
 */
public class VolatileTest {
//    volatile static int number = 0;
    static int number = 0;
    public static void add(){
        number=10;
    }

    /**
     * 可见性
     */
    public static void main(String[] args) {

        new Thread(()->{
            System.out.println(Thread.currentThread().getName()+"开始执行时，number="+VolatileTest.number);

            try {
                Thread.sleep(3000);
                VolatileTest.add();
                System.out.println(Thread.currentThread().getName()+"执行add()方法之后，number="+VolatileTest.number);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }).start();
        while (number==0){

        }
         System.out.println(Thread.currentThread().getName()+"程序结束");

    }


}
```
运行结果:陷入死循环
```
Thread-0开始执行时，number=0
Thread-0执行add()方法之后，number=10
```
加入  volatile static int number = 0; 关键字 volatile
运行结果：跳出循环

```
Thread-0开始执行时，number=0
Thread-0执行add()方法之后，number=10
main程序结束
```

---

2. 有序性（禁止指令重排序）
计算机在执行程序时，为了提高计算性能，编译器和处理器常常会对指令进行重排序，一般分为三种
> 源代码->编译器优化的重排->指令并行的重排->内存系统的重排->最终执行的命令

有序性实现原理
volatile有序性