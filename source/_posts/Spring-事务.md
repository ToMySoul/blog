---
title: Spring 事务
date: 2023-04-06 14:47:54
categories: 工作
tags: 
- Spring
- 事务
---
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
