---
title: Java 多线程
date: 2023-04-06 14:40:49
categories: 工作
tags:
- Java
- 多线程
---

## 多线程

### 创建线程的三种方式

```
    /**
     * 创建线程三种方式
     */
    @Test
    public void threadDemoTest() {
//        一、继承Thread，重写run方法，调用start即可。
        //如果thread 中Runnable 值不为空，则调用runnable 中 run 方法 ，为空
        Thread thread = new Thread("线程二");

        thread.start();
        System.out.println(thread.getName());

//        二、实现runnable接口，重写run方法，调用start。

        MyRunnable r1 = new MyRunnable();
        r1.setName("线程一");

//  三、实现Callable接口类，并重写call方法，使用FutureTask类来包装Callable实现类的对象，并且使用FutureTask对象来作为Thread对象的target来创建线程。
        Mycallable mycallable = new Mycallable();
        FutureTask ft = new FutureTask(mycallable);
        Thread thread1 = new Thread(ft);
        thread1.start();
    }


```

### 实例1：如何让n个线程顺序遍历含有n个元素的List集合

     /**
     * 数据，线程数
     *
     * @param data
     * @param threadNum
     */
    public synchronized void handleList(List<String> data, int threadNum) {
        int size = data.size();
        int limitNum = size % threadNum == 0 ? size / threadNum : (size / threadNum + 1);
        boolean falg = size % threadNum != 0;
    
        for (int i = 0; i < threadNum; i++) {
            int startIndex = i * limitNum;
            int endIndex = (i + 1) * limitNum;
    
            if (falg && i == threadNum - 1) {
                endIndex = size;
            }
    
            HandleThread thread = new HandleThread("线程[" + (i + 1) + "] ", data, startIndex, endIndex);
            thread.start();
        }
    }
    
    /**
     * 实例1：如何让n个线程顺序遍历含有n个元素的List集合
     */
    @Test
    public void multiThreadedSequentialTraversalList() {
    
        // 准备数据
        List<String> data = new ArrayList<String>();
        for (int i = 1; i < 15; i++) {
            data.add("item" + i);
        }
        handleList(data, 4);
    
    }
### 实例2：List多线程并发读取读取现有的list对象

    /**
     * 实例2：List多线程并发读取读取现有的list对象
     */
    @Test
    public void multiThreadedReadListObject() {
        List<String> list = new ArrayList<String>(10);
        Map<Long, Integer> map = new HashMap<>(16);
        for (int i = 0; i < 100; i++) {
            list.add("" + i);
        }
        //获取虚拟机的cpu 处理数量（方法询问jvm，jvm去问操作系统，操作系统去问硬件）
        int pcount = Runtime.getRuntime().availableProcessors();
        long start = System.currentTimeMillis();
        for (int i = 0; i < pcount; i++) {
            Thread t = new MyThread1(list, map);
            map.put(t.getId(), Integer.valueOf(i));
            t.start();
            try {
                t.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(list.get(i));
        }
        System.out.println("----" + (System.currentTimeMillis() - start));
    }
### 详解多线程及Runable 和Thread的区别

```
public static void main(String[] args) {
    MyThreadTest mt1=new MyThreadTest("窗口1");
    MyThreadTest mt2=new MyThreadTest("窗口1");
    MyThreadTest mt3=new MyThreadTest("窗口1");
    mt1.start();
    mt2.start();
    mt3.start();


    MyRunnableTest mt = new MyRunnableTest();
    Thread mrt1 = new Thread(mt,"窗口1");
    Thread mrt2 = new Thread(mt,"窗口2");
    Thread mrt3 = new Thread(mt,"窗口3");
    mrt1.start();
    mrt2.start();
    mrt3.start();
}

/**
 * 一共创建了3个MyThreadTest对象，而这3个对象的资源不是共享的，
 * 即各自定义的ticket=5是不会共享的，因此3个线程都执行了5次循环操作。
 *
 */
static class  MyThreadTest extends  Thread{
    private  int ticket=5;
    private  String name;
    public  MyThreadTest(String name){
        this.name=name;
    }
    public  void run(){
        while (true){
            if (ticket<1){
                break;
            }
            System.out.println(name+'='+ticket--);
        }
    }
}

/**
 * 只创建了一个MyRunnableTest对象，而3个Thread线程都以同一个MyRunnableTest来启动，所以他们的资源是共享的。
 */
static class  MyRunnableTest implements   Runnable{
    private  int ticket=5;

    public  void run(){
        while (true){
            if (ticket<1){
                break;
            }
            System.out.println(Thread.currentThread().getName()+'='+ticket--);
        }
    }
}
```