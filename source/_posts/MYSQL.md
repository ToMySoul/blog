---
title: Mysql
categories: 工作
tags: 
- 数据库
- Mysql
---

mysql 5.1 之前，默认myisam ，从mysql5.5 之后版本默认搜索引擎为innoDB

## MyISAM存储引擎

### 存储引擎InnoDB与Myisam的六大区别

1. 事务：

* InnoDB支持回滚、支持事务、外检、行锁、多版本事务管理mvcc 
* myisam 不支持事务、不支持外检、表级锁

2. 存储结构

* InnoDB 两个文件.ifm （表结构）、.ibd（数据和索引）
* myisam 三个文件.ifm（表结构）、.myd(数据文件)、.myi(索引文件)

3. 存储空间

* Innodb 占用空间大，需要更多的内存和储存，他需要有缓存池用于高速缓存数据和索引
* myisam 占用空间小，可被压缩，数据以文件的方式存储. 支持三种不同的存储格式;  静态表（默认，但是注意数据末尾不能有空格，会被去掉）、动态表、压缩表。

4. 索引

* Innodb 可以使用sphinx 插件支持全文索引，主键索引保存的是数据，其他索引保存的是主键索引地址。
* myisam 支持全文索引fulltext， 索引保存的是行地址。

5. 缓存

* myisam仅仅缓存索引，不缓存数据（myisam数据文件和索引文件分开）
* innodb 缓存索引和真实数据,所以对内存有更高的要求。

6. 性能

* select: myisam 性能高

* update、insert :innodb性能高

清空整个表时，innodb 是一行一行的删除，效率非常慢。myisam则会重建表

### 为什么myisam会在innodb 的查询速度快？

Innodb 在做select的时候，要维护的东西比myisam引擎多很多；

1. 数据块，innodb 要缓存，mysiam 只缓存索引快，这中间还有换进换出的减少；
2. innodb寻址要映射到块，再到行，myisam记录的直接是文件的offset,定位比innodb要快
3. innodb还需要维护mvcc一致；虽然你的场景，但他还是需要去检查和维护。

如果执行大量的select .myisam 是更好的选择。

如果你的数据执行大量的insert或update，出于性能方面的考虑，应该使用innoDB表。

## InniDB 的索引模型

在InnoDB 中，表都是根据主键顺序以索引的形式存放的，这种存储方式的表称为索引组织表InnoDB使用了B+ 树索引模型，每个索引都对应一个B+树。

* 主键索引的叶子节点存的是整行的数据。在InnoDB中，主键索引也被称为聚簇索引。
* 非主键索引的叶子结点内容是主键的值。在InnoDB中，非主键索引也被称为二级索引。

### 主键索引和普通索引的查询区别

* 主键查询方式 select * from T where 主键=500，则只需要搜索ID这颗B+树。
* 普通索引查询方式 select * from T where 普通索引=500，则先搜索普通索引树，得到主键的值，然后到ID索引树搜索一次。这个过程称为回表。

### 覆盖索引

​	如果执行的语句是select ID from T where k between 3 and 5，这时只需要查ID的值，而ID的值已经在k索引树上了，因此可以直接提供查询结果，不需要回表。也就是说，在这个查询里面，索引k已经覆盖了我们的查询需求，我们称为覆盖索引。由于覆盖索引可以减少树的搜索次数，显著提高性能，所以覆盖索引是一个常用的性能优化手段。

​	基于上面覆盖索引的说明，我们来讨论一个问题：在一个市民信息表上，是否有必要将身份证号和姓名建立联合索引。假设市民表的定义如下：

​	如果现在有一个高频请求，要根据市民的身份证号查询他的姓名，这个**联合索引**就有意义了。它可以在这个高频请求上用到覆盖索引，不再需要回表查整行记录，减少语句的执行时间。当然，索引字段的维护总是有代价的。因此，在建立冗余索引来支持覆盖索引时就需要做一下权衡。

### 最左匹配原则

​	索引项是按照索引定义里面出现的字段顺序排序的。按照最左匹配原则，当你的逻辑需求是查到所有名字是“张三”的人或者是查到所有名字第一个字是“张”的人，都可以用上这个索引。这个最左匹配既可以是联合索引的最左N个字段，也可以是字符串索引的最左M个字符。那在建立联合索引的时候，如何安排索引内的字段顺序呢？

* 第一原则是，如果通过调整顺序，可以少维护一个索引，那么这个顺序往往就是需要优先考虑的。

* 第二要考虑的就是空间原则。比如我们的市民表中，name字段是比age字段大的，因此创建一个(name,age)的联合索引和一个(age)的单字段索引，要优于(age,name)的联合索引和一个(name)的单字段索引。

### 索引下推

前面我们说到，最左前缀匹配可以在索引中定位记录。这时，那些不符合最左前缀的部分，会怎么样呢？

还是以市民表的联合索引（name,age）为例，如果现在有个请求是检索出表中，名字第一个字是张，而且年龄是10岁的所有男孩。那么SQL语句是这么写的：

mysql> select * from tuser where name like '张%' and age=10 and ismale=1;
此时，这个语句在搜索索引树的时候，只能用到“张”，找到第一个满足条件的记录ID3，然后是判断其他条件是否满足。这个判断过程在MySQL5.6之前和之后是完全不同的：

5.6之前，只能从ID3开始一个一个回表，到主键索引上找出数据行，在对比字段值。
5.6开始，引入了索引下推优化，可以在索引遍历过程中，对索引中包含的字段先做判断，直接过滤掉不满足条件的记录，减少回表次数。

在无索引下推执行过程图中，InnoDB并不会去看age的值，只是按顺序把name第一个字是“张”的记录一条条取出来回表。因此，需要回表4次。在索引下推执行过程图中，InnoDB在(name,age)索引内部就判断了age是否等于10，对于不等于10的记录，直接判断并跳过。此时只需要回表2次。

### 普通索引和唯一索引

 唯一索引相较于普通索引，能保证数据表中每一行数据的唯一性。

* 普通索引查找满足条件的第一条记录后，需要查找下一个记录，直到第一个不满足条件的记录。
* 对于唯一索引来说由于索引定义了唯一性，查找到第一个满足条件的记录后就会停止继续检索。


## MYSQL锁
{% asset_img Mysql锁.png Mysql锁 %}

锁是计算机协调多个进程或线程并发访问某一资源的机制。锁保证数据并发访问的一致性、有效性；锁冲突也是影响数据库并发访问性能的一个重要因素。锁是Mysql在服务器层和存储引擎层的的并发控制。

按粒度分三种：

* 表锁：开销小，加锁快；锁定力度大，发生锁冲突概率高，并发度最低;不会出现死锁。
* 行锁：开销大，加锁慢；会出现死锁；锁定粒度小，发生锁冲突的概率低，并发度高。
* 页锁：开销和加锁速度介于表锁和行锁之间；会出现死锁；锁定粒度介于表锁和行锁之间，并发度一般。

兼容性分两种：

- 共享锁（S Lock）,也叫读锁（read lock），相互不阻塞。
- 排他锁（X Lock），也叫写锁（write lock），排它锁是阻塞的，在一定时间内，只有一个请求能执行写入，并阻止其它锁读取正在写入的数据

### InnoDB行锁

* **Record Lock 记录锁**

记录锁就是直接锁定某行记录。当我们使用唯一性的索引（唯一索引，聚集索引）进行等值查询且精准匹配到一条记录时，此时就会直接将这条记录锁定。

- **Gap Lock 间隙锁**

隙锁(Gap Locks) 的间隙指的是两个记录之间逻辑上尚未填入数据的部分,是一个**左开右开空间**。

间隙锁就是锁定某些间隙区间的。当我们使用用等值查询或者范围查询，并且没有命中任何一个record，此时就会将对应的间隙区间锁定。例如select * from t where id =3 for update;或者select * from t where id > 1 and id < 6 for update;就会将(1,6)区间锁定。

- **Next-key Lock 临键锁**

临键指的是间隙加上它右边的记录组成的**左开右闭区间**。比如上述的(1,6]、(6,8]等

临键锁就是记录锁(Record Locks)和间隙锁(Gap Locks)的结合，即除了锁住记录本身，还要再锁住索引之间的间隙。当我们使用范围查询，并且命中了部分record记录，此时锁住的就是临键区间。注意，临键锁锁住的区间会包含最后一个record的右边的临键区间。例如select * from t where id > 5 and id <= 7 for update;会锁住(4,7]、(7,+∞)。mysql默认行锁类型就是临键锁(Next-Key Locks)。当使用唯一性索引，等值查询匹配到一条记录的时候，临键锁(Next-Key Locks)会退化成记录锁；没有匹配到任何记录的时候，退化成间隙锁。

### MySql的乐观锁和悲观锁

- **悲观锁**（Pessimistic Concurrency Control）

悲观锁认为被它保护的数据是极其不安全的，每时每刻都有可能被改动，一个事务拿到悲观锁后，其他任何事务都不能对该数据进行修改，只能等待锁被释放才可以执行

- **乐观锁（Optimistic Concurrency Control）**

乐观锁通常是通过在表中增加一个版本(version)或时间戳(timestamp)来实现，其中，版本最为常用。

事务在从数据库中取数据时，会将该数据的版本也取出来(v1)，当事务对数据变动完毕想要将其更新到表中时，会将之前取出的版本v1与数据中最新的版本v2相对比，如果v1=v2，那么说明在数据变动期间，没有其他事务对数据进行修改，此时，就允许事务对表中的数据进行修改，并且修改时version会加1，以此来表明数据已被变动。

如果，v1不等于v2，那么说明数据变动期间，数据被其他事务改动了，此时不允许数据更新到表中，一般的处理办法是通知用户让其重新操作。不同于悲观锁，乐观锁通常是由开发者实现的。
### MySQL 遇到过死锁问题吗，你是如何解决的

### MySQL事务

### MySQL隔离级别

#### 隔离级别

1. 读取未提交内容

在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。读取未提交的数据，也被称之为脏读（Dirty Read）

2. 读取提交内容

这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这种隔离级别 也支持所谓的不可重复读（Nonrepeatable Read），因为同一事务的其他实例在该实例处理其间可能会有新的commit，所以同一select可能返回不同结果

3. 可重读

这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上，这会导致另一个棘手的问题：幻读 （Phantom Read）。简单的说，幻读指当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的“幻影” 行。**InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题**。

4. 可串行化

这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。

#### 不同隔离级别带来的数据问题

- 脏读

事务读到另一个事务未提交的数据

- 不可重复读

事务读到另一个事务已提交修改的数据 

- 幻读

事务读到另一个事务已提交的新增或修改的数据

#### 隔离级别

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
| -------- | ---- | ---------- | ---- |
| 未提交读 | √    | √          | √    |
| 已提交读 | ×    | √          | √    |
| 可重复读 | ×    | ×          | √    |
| 串行化   | ×    | ×          | ×    |

大多事数据库的隔离级别默认为已提交读；mysql 的默认隔离级别为不可重复读。

#### Mysql-幻读处理



### 锁问题总结

### 什么是锁？MySQL 中提供了几类锁？

### 什么是死锁？

死锁是指两个或者多个事务在同一资源上互相占用，并请求锁定对方占用的资源，从而导致恶行循环的现象，当多个事务试图以不同的顺序锁定资源时，就可能会产生死锁，多个事务同时锁定同一个资源时，也会产生死锁。

### 常见的死锁案例有哪些？

### 如何处理死锁？

* 应急手动杀掉进程

1. 在mysql中查看当前所有进程列表（show processlist）
2. 查看事务列表(select * from information_schema.INNODB_TRX;)
3. 手动杀掉进程 kill  [processid]

* 设置参数

设置MYSQL 锁等待超时 innodb_lock_wait_timeout=50 ，autocommit=on

* MySQL有两种死锁处理方式：

  1. 等待，直到超时（innodb_lock_wait_timeout=50s）。
  2. 发起死锁检测，主动回滚一条事务，让其他事务继续执行（innodb_deadlock_detect=on）。

  由于性能原因，一般都是使用死锁检测来进行处理死锁。

  **死锁检测**

  死锁检测的原理是构建一个以事务为顶点、锁为边的有向图，判断有向图是否存在环，存在即有死锁。

  **回滚**

  检测到死锁之后，选择插入更新或者删除的行数最少的事务回滚，基于 INFORMATION_SCHEMA.INNODB_TRX 表中的 trx_weight 字段来判断。

* 分析

当前有哪些事务在等待锁？ 这些锁需要锁哪些表，锁哪些索引，锁哪些记录和值 ？
 处于等待状态的相关SQL是什么？
 在等待哪些事务完成 ？
 拥有当前锁的SQL是什么？

在mysql 5.5中，information_schema 库中增加了三个关于锁的表（MEMORY引擎）；
 innodb_trx ## 当前运行的所有事务
 innodb_locks ## 当前出现的锁
 innodb_lock_waits ## 锁等待的对应关系

查看是三个表的表结构

**innodb_locks**

```ruby
> desc innodb_locks;
+-------------+---------------------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------------+---------------------+------+-----+---------+-------+
| lock_id | varchar(81) | NO | | | |#锁ID
| lock_trx_id | varchar(18) | NO | | | |#拥有锁的事务ID
| lock_mode | varchar(32) | NO | | | |#锁模式
| lock_type | varchar(32) | NO | | | |#锁类型
| lock_table | varchar(1024) | NO | | | |#被锁的表
| lock_index | varchar(1024) | YES | | NULL | |#被锁的索引
| lock_space | bigint(21) unsigned | YES | | NULL | |#被锁的表空间号
| lock_page | bigint(21) unsigned | YES | | NULL | |#被锁的页号
| lock_rec | bigint(21) unsigned | YES | | NULL | |#被锁的记录号
| lock_data | varchar(8192) | YES | | NULL | |#被锁的数据
+-------------+---------------------+------+-----+---------+-------+
```

**innodb_lock_waits**

```ruby
> desc innodb_lock_waits;
+-------------------+-------------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------------------+-------------+------+-----+---------+-------+
| requesting_trx_id | varchar(18) | NO | | | |#请求锁的事务ID
| requested_lock_id | varchar(81) | NO | | | |#请求锁的锁ID
| blocking_trx_id | varchar(18) | NO | | | |#当前拥有锁的事务ID
| blocking_lock_id | varchar(81) | NO | | | |#当前拥有锁的锁ID
+-------------------+-------------+------+-----+---------+-------+
```

**innodb_trx**

```ruby
> desc innodb_trx ;
+----------------------------+---------------------+------+-----+---------------------+-------+
| Field | Type | Null | Key | Default | Extra |
+----------------------------+---------------------+------+-----+---------------------+-------+
| trx_id | varchar(18) | NO | | | |#事务ID
| trx_state | varchar(13) | NO | | | |#事务状态：
| trx_started | datetime | NO | | 0000-00-00 00:00:00 | |#事务开始时间；
| trx_requested_lock_id | varchar(81) | YES | | NULL | |#innodb_locks.lock_id
| trx_wait_started | datetime | YES | | NULL | |#事务开始等待的时间
| trx_weight | bigint(21) unsigned | NO | | 0 | |#
| trx_mysql_thread_id | bigint(21) unsigned | NO | | 0 | |#事务线程ID
| trx_query | varchar(1024) | YES | | NULL | |#具体SQL语句
| trx_operation_state | varchar(64) | YES | | NULL | |#事务当前操作状态
| trx_tables_in_use | bigint(21) unsigned | NO | | 0 | |#事务中有多少个表被使用
| trx_tables_locked | bigint(21) unsigned | NO | | 0 | |#事务拥有多少个锁
| trx_lock_structs | bigint(21) unsigned | NO | | 0 | |#
| trx_lock_memory_bytes | bigint(21) unsigned | NO | | 0 | |#事务锁住的内存大小（B）
| trx_rows_locked | bigint(21) unsigned | NO | | 0 | |#事务锁住的行数
| trx_rows_modified | bigint(21) unsigned | NO | | 0 | |#事务更改的行数
| trx_concurrency_tickets | bigint(21) unsigned | NO | | 0 | |#事务并发票数
| trx_isolation_level | varchar(16) | NO | | | |#事务隔离级别
| trx_unique_checks | int(1) | NO | | 0 | |#是否唯一性检查
| trx_foreign_key_checks | int(1) | NO | | 0 | |#是否外键检查
| trx_last_foreign_key_error | varchar(256) | YES | | NULL | |#最后的外键错误
| trx_adaptive_hash_latched | int(1) | NO | | 0 | |#
| trx_adaptive_hash_timeout | bigint(21) unsigned | NO | | 0 | |#
+----------------------------+---------------------+------+-----+---------------------+-------+
```

### 如何查看死锁？

1. 查询是否锁表（show OPEN TABLES where In_use > 0;）

2. 查询进程（show processlist）
3. 杀死进程id（kill [processid]）

查看当前的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;

查看当前锁定的事务

SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;

查看当前等锁的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;



### 如何避免死锁？

**收集死锁信息：**

1. 利用命令 SHOW ENGINE INNODB STATUS查看死锁原因。
2. 调试阶段开启 innodb_print_all_deadlocks，收集所有死锁日志。

**减少死锁：**

1. 使用事务，不使用 lock tables 。
2. 保证没有长事务。
3. 操作完之后立即提交事务，特别是在交互式命令行中。
4. 如果在用 (SELECT ... FOR UPDATE or SELECT ... LOCK IN SHARE MODE)，尝试降低隔离级别。
5. 修改多个表或者多个行的时候，将修改的顺序保持一致。
6. 创建索引，可以使创建的锁更少。
7. 最好不要用 (SELECT ... FOR UPDATE or SELECT ... LOCK IN SHARE MODE)。
8. 如果上述都无法解决问题，那么尝试使用 lock tables t1, t2, t3 锁多张表

### InnoDB 默认是如何对待死锁的？



### 如何开启死锁检测？

### 什么是全局锁？它的应用场景有哪些？

### 什么是共享锁？

### 什么是排它锁？

### 使用全局锁会导致什么问题？

### 如何处理逻辑备份时，整个数据库不能插入的情况？

### [For update ](https://blog.csdn.net/qq_40891009/article/details/106007658)

select … for update 语句是我们经常使用手工加锁语句。

for update 是一种行级锁，又叫排它锁，一旦用户对表某个记录施加了行级加锁，则该用户可以查询也可以更新被加锁的数据行,其他用户只能查询但不能更新被加锁的数据行，如果其他用户想更新该表中的数据行，则也必须对该表施加行级锁，即使多个用户对一个表均使用了共享更新，但也不允许两个事务同时对一个表进行更新，真正对表进行更新时，是以独占方式锁表，一直到提交或复原该事务为之。行锁永远时独占方式锁。

简单地讲for update就是将原来我们在代码里面加锁的情况转移到在数据库操作时进行加锁,通过它,我们不需要在代码逻辑中手动进行加锁,只需要在需要操作的记录时使用select 结合for update即可锁定数据,但是两个事务不能同时操作被锁定的数据,只有等一个事务提交完后另一个事务才只能操作,从而保证了线程安全问题。

特点：

1. Mysql中For update仅适用于Innodb, 且必须在事务处理模块(BEGIN/COMMIT)中才能生效 。Postgresql的话可以直接使用(下面会使用案例说明)
2. For Update虽然是行级锁,但是并不所有的情况下都只锁行,某些情况下也会将整个表锁住


