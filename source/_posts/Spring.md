---
title: Spring
categories: 工作
tags: 
- spring
---


# [Spring](https://blog.csdn.net/a745233700/article/details/80959716/) 

## Spring IOC 

1. 什么是ioc 





## Spring AOP

Aop 一般称为面向切面，作为面向对象的补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为切面。减少系统中的重复代码，降低了模块间的耦合度，提高系统的可维护性。可用于权限认证、日志、事务处理。

### spring aop 里面几个名词概念

1. 连接点：指程序运行过程中所执行的方法。在spring aop 中一个连接点总代表一个方法的执行。

2. 切面：被抽取来的公共模块，可以用来会横切多哥对象。Aspect 切面可以看成切点和通知的结合，一个切面可以由多个切点和通知组成。

   > 在Spring AOP中，切面可以在类上使用 @AspectJ 注解来实现。

3. 切点：切点用于定义  要对那些Join point 进行拦截

> 切点分为execution方式和annotation方式。execution方式可以用路径表达式指定对哪些方法拦截，比如指定拦截add*、search*。annotation方式可以指定被哪些注解修饰的代码进行拦截。

。。。。。


## Spring MVC

