---
title: Java-问题集
date: 2023-04-11 13:47:13
categories: Java
tags:
- Java
-  问题集
---
### 关键字this为什么不能出现在static方法中

```
   public static void add(){
        this.number=10; //this 错误
    }
```

>当Static方法在类加载时就已经存在了（JAVA虚拟机初始化时），但是对象是在创建时才在内存中生成。也就是说this还无法指向对象引用，静态方法就已经存在了，在运行静态方法时，无法获取>到当前对象引用，从而导致出现异常。所以在静态方法中无法使用this关键字


