---
title: Spring
categories: Spring
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



## Spring 事务

spring事务的传播行为 

### spring 事务特性（四）

1. 原子性：事务不可分割。
2. 一致性：事务的执行的前后数据的完整性保持一致。
3. 隔离性：一个事务执行的过程中，不该收到其他事务干扰。
4. 持久性：事务一旦结束，数据就持久到数据库。

### spring 事务的隔离级别（五）

1. 这是个 PlatfromTransactionManager 默认的隔离级别（使用数据库设置），使用数据库默认的事务隔离级别。

2. 读未提交，允许事务在执行过程中，读取其他事务未提交的数据。

3. 读已提交，允许事务在执行过程中，读取其他事务已经提交的数据。

4. 可重复读，在同一个事务内，任意时刻的查询结果都是一致的。

5. ：串行化，所有事务逐个依次执行。

### spring 事务传播机制（七）

1. Propagation.REQUIRED：默认的事务传播级别，它表示如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
2. Propagation.SUPPORTS：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
3. Propagation.MANDATORY：（mandatory：强制性）如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。
4. Propagation.REQUIRES_NEW：表示创建一个新的事务，如果当前存在事务，则把当前事务挂起。也就是说不管外部方法是否开启事务，Propagation.REQUIRES_NEW 修饰的内部方法会新开启自己的事务，且开启的事务相互独立，互不干扰。
5. Propagation.NOT_SUPPORTED：以非事务方式运行，如果当前存在事务，则把当前事务挂起。
6. Propagation.NEVER：以非事务方式运行，如果当前存在事务，则抛出异常。
7. Propagation.NESTED：如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于 PROPAGATION_REQUIRED。

- 普通型

Propagation.REQUIRED（需要有房子）：有房子了咱们一起住，没房子了咱们一起赚钱买房子。
Propagation.SUPPORTS（可以有房子）：有房子了就一起住，没房子了咱们就一起租房子。
Propagation.MANDATORY（强制有房子）：有房子了就一起住，没房子了就分手。

* 强势型

- Propagation.REQUIRES_NEW：不要你的房子，必须一起赚钱买房子。
- Propagation.NOT_SUPPORTED：不要你的房子，必须一起租房子。
- Propagation.NEVER：必须一起租房子，你有房子就分手。

- 懂事型

如果有房子了就以房子为基础做点小生意，卖个花生、水果啥的，如果买卖成了，那就继续发展；如果失败了，至少还有房子；如果没房子也没关系，一起赚钱买房子。

# Spring 答疑

### 1.  Spring如何解决循环依赖问题

使用 @Lazy 注解 设置某一个 service 延迟加载就可以了.

>@Component
>public class Subscriber implements MessageListener {
>	@Lazy
>	@Autowired
>	private VerificationSchemeService schemeService;

### 2. Spring 框架中都用到了哪些设计模式？

（1）工厂模式：Spring使用工厂模式，通过BeanFactory和ApplicationContext来创建对象

（2）单例模式：Bean默认为单例模式

（3）策略模式：例如Resource的实现类，针对不同的资源文件，实现了不同方式的资源获取策略

（4）代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术

（5）模板方法：可以将相同部分的代码放在父类中，而将不同的代码放入不同的子类中，用来解决代码重复的问题。比如RestTemplate, JmsTemplate, JpaTemplate

（6）适配器模式：Spring AOP的增强或通知（Advice）使用到了适配器模式，Spring MVC中也是用到了适配器模式适配Controller

（7）观察者模式：Spring事件驱动模型就是观察者模式的一个经典应用。

（8）桥接模式：可以根据客户的需求能够动态切换不同的数据源。比如我们的项目需要连接多个数据库，客户在每次访问中根据需要会去访问不同的数据库











