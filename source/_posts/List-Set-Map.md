---
title: List-Set-Map
categories: 工作
tags: 
- Java
- 数组
- 数据结构
---
### List-Set-Map

* List  有序，允许存储重复元素，允许存取Null 值

ArryList : 基于数组的数据结构快速实现；查询快，增删慢；线程不安全，建议单线程使用

LinkedList : 基于链表的数据结构实现；查询慢，增删快；线程不安全

Vector ： 线程安全，API方法都带有synchronize 关键字；效率低

CopyOnWriterArrayList : 线程安全；renntrantLock 可重入锁 ；多线程并发访问下，效率高

* Set 存取无序，不允许添加重复元素；线程不安全；允许存放null 值

HashSet :

1. 1.8 之前 数组+链表，1.8之后 数组+链表+红黑树；存取无序；
2. 不允许添加重复元素，依据equals()和hashcode();线程不安全；
3. 允许存放Null 值

TreeSet : 

1. 基于红黑树数据结构实现；
2. 添加进来的元素会进行排序，元素自身具备比较性  implements Comparable 重写compareTo(o1)

，容器具备比较性  new Comparator  重写compare(o1,o2)

3. 不允许添加重复元素，根据compareTo(o1)或compare(o1,o2) 的返回值；
4. 线程不安全；
5. 不允许存放Null 值

LinkedHashSet:

1. 存取有序，依靠链表实现存取有序
2. 线程不安全
3. 允许存放Null 值
4. 不允许添加重复元素,依据equals()和hashcode()

copyOnWriteArraySet

1. 线程安全。利用可重入锁renntrantLock 











