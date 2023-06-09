---
title: Redis
categories: 工作
tags: 
- redis
- 数据库
---

### 什么是Redis

​	Redis 本质上是Key-Value 类型的内存数据库，整个数据库加载在内存当中操作。

* 纯内存操作性能快。
* 支持保存多种数据结构，单个Value的最大限制1GB。
* List 类型可以实现轻量级高性能的消息队列服务。
* Set 可以做高性能的Tag 系统。
* Redis 可以设置Key-Value 过期时间。

​	Redis 支持的数据类型:String,Lisr,Set,Hash,Sorted Set

一个字符串类型的值能存储最大容量：512M

### Redis 有哪几种数据淘汰策略

1. **noeviction** (默认策略): 对于写请求不在提供服务，直接返回错误（DEL 请求和部分特殊请求除外）

2. **allkeys-lru**：从所有Key 中使用LRU算法进行淘汰（LRU 算法：最近最少使用算法）
3. **volatile-lru**：从设置了过期时间的key中使用LRU算法进行淘汰
4. **allkeys-random**：从所有key中随机淘汰数据
5. **volatile-random**：从设置了过期时间的key中随机淘汰

当使用volatile-[lru](https://so.csdn.net/so/search?q=lru&spm=1001.2101.3001.7020)、volatile-random、volatile-ttl这三种策略时，如果没有key可以被淘汰，则和noeviction一样返回错误

### 为什么Redis需要把所有的数据放在内存中？

Redis 为了达到最快的读写速度将数据都读到内存中，并通过异步的方式将数据写入磁盘。所以Redis具有快速和数据库持久化的特征。

如果设置了最大使用的内存，则数据已有记录数达到内存限值后不能那个继续插入新值。

### Redis 集群的三种方式

主从复制；哨兵模式，Cluster集群

1. 主从复制

* 基本原理



2. 哨兵模式
3. 集群

一主二从 

## Redis 分布式锁

所有服务都到一个统一的地方来取锁，只有取到锁才能继续执行下去。

为实现分布式锁，在Redis 中存在 命令。意为set if  not exists(如果不存在该ksy，才去set)

> setnx key value  

如果进行了加锁，但因为宕机或者出现异常未释放锁，就造成了死锁。

可以设置过期时间，单位秒，

> setex key seconds value 

为指定key 设置过期时间，但存在另一个问题，还没设置过期时间，redis 就宕机了，所以要保证原子性。加锁的同时设置过期时间。

> set key value ex seconds nx 
>
> 例： set order_lock  123 ex 100 nx 

这样还存在一种问题，redis 一个业务执行时间很长，锁已经过期，被人已经设置了新的锁，但是业务执行完之后直接释放锁，有可能删除了别人加的锁。

所以在加锁的时候，要设一个随机值，在删除锁时进行对比，如果是自己的锁，才删除。

**加锁和释放锁的原子性可以用lua脚本来保证，那锁的自动续期改如何实现呢？**

## Redisson 实现

redisson 顾名思义，redis的儿子，本质上还是redis 加锁，不过是对redis做了很多封装。他不仅提供了一系列分布式的java 常用对象，还提供了许多分布式服务。

Redisson 优势

* 锁的自动续期（默认都是30s ），如果业务超长，运行期间会自动给锁续上新的30s ，不用担心业务执行时间超长而锁被自动删掉。

* 加锁的业务只要运行完成。就不会给当前续期，即便不手动解锁，锁默认在30s 后删除不会造成死锁问题，

### 持久化（默认RDB）

#### **RDB** （快照）

按照一定的时间将内存的数据以快照的形式保存到硬盘中，对应产生的数据库文件为dump.rdb

##### 触发快照的时机

* 客户端方式：BGSAVE和SAVE指令
* 服务端方式：服务器配置自动触发和shutdown

1. **符合自定义配置的快照规则（redis.conf）** 

save：这里是用来配置触发 Redis的 RDB 持久化条件，也就是什么时候将内存中的数据保存到硬盘。比如"save m n"。表示m秒内数据集存在n次修改时，自动触发bgsave。

默认如下配置： save 900 1 ：表示900秒钟内至少1个键被更改则进行快照。 save 300 10 ：表示300秒内至少10个键被更改则进行快照。 save 60 10000 ：表示60秒内至少10000个键被更改则进行快照。

如果不需要持久化，那么你可以注释掉所有的 save 行来停用保存功能或设置为save "" 。

2. **执行save或者bgsave命令**

SAVE:

* 执行该命令会阻塞当前Redis 服务器，执行save 命令期间，Redis不能处理其他命令，直到RDB过程完成为止。

* 执行完成的时候，如果存在老的RDB 文件，就把新的替换掉旧的，我们的客户端都有几万或者几十万，这种方式不可取。

BGSAVE：

* 执行该命令时，Redis会在后台异步进行快照操作，快照的同时还可以响应客户端请求。

* 具体操作是Redis进程执行fork操作创建子进程，RDB持久化过程由子进程负责，完成后自动结束。阻塞只发生在fork阶段，一般时间很短。基本上 Redis 内部所有的RDB操作都是采用 bgsave 命令。

2. **执行ushall命令**

2. **执行主从复制操作（第一次）** 

##### 注意事项

Redis在进行快照的过程中不会修改RDB文件，只有快照结束后才会将旧的文件替换成新的，也就是说任何时候RDB文件都是完整的；

这就使得我们可以通过定时备份RDB文件来实现Redis数据库的备份，RDB文件是经过压缩的二进制文件，占用的空间会小于内存中的数据，更加利于传输。

##### RDB优缺点

1. 优点
   * RDB可以最大化Redis的性能
   * RDB在恢复大数据集时，速度比AOF的恢复速度要快。
2. 缺点    
   * RDB快照是一次全量备份，储存的是内存数据的二进制序列化形式，储存上非常紧凑。当进行快照时，会开启一个子进程专门负责快照持久化，子进程会拥有父进程的内存数据，父进程修改内存子进程不会反应过来。所以在快照持久化期间修改的数据不会被保存，可能丢失数据，一旦Redis异常退出，就会丢失最后一次快照以后更改的所有数据。
   * 这个时候我们就需要根据具体的应用场景，通过组合设置自动快照条件的方式来将可能发生的数据损失控制在能够接受范围。
   * 如果数据相对来说比较重要，希望将损失降到最小，则可以使用AOF方式进行持久化。

#### **AOF**（追加日志文件）

redis会将每一个收到的写命令都通过write函数追到文件中。

开启AOF持久化后，每执行一条更改redis 中的数据的命令，redis就会将该命令写入硬盘中的AOF文件，这一个过程会降低redis性能，使用较快的硬盘可以提高AOF的性能。

```
# 可以通过修改redis.conf配置文件中的appendonly参数开启
appendonly yes
# AOF文件的保存位置和RDB文件的位置相同，都是通过dir参数设置的。
dir ./
# 默认的文件名是appendonly.aof，可以通过appendfilename参数修改
appendfilename appendonly.aof
```

* 原理

  1. Redis将所有对数据库进行过写入的命令及其参数记录到AOF文件，以此达到记录数据库状态的目的。（同步）

  2. 当一个Redis客户端需要执行命令时，通过网络连接。将协议文本发送给Redis服务器，AOF文件通过网络通讯协议的格式来保存这些命令。

  3. Redis客户端使用RESP（Redis的序列化协议）协议与Redsi的服务器端进行通信。

* AOF保存模式

​	Redis目前支持三种AOF保存模式：不保存，每秒钟保存一次，每执行一个命令保存一次

AOF_FSYNC_NO（不保存）：在这种模式下，每次调用ushAppendOnlyFile函数，WRITE都会被执行，但**SAVE会被略过**。在这种模式下，SAVE只会在以下情况中被执行： 1.Redis被关闭； 2.AOF功能被关闭； 3.系统的写缓存被刷新（可能是缓存已经被写满，或者定期保存操作被执行）。 这三种情况下的SAVE操作都会引起Redis主进程阻塞。

AOF_FSYNC_EVERYSEC（每一秒钟保存一次）：在这种模式中，SAVE原则上每隔一秒钟就会执行一次，因为SAVE操作是由后台**子线程**调用的，所以它不会引起服务器主进程阻塞。

AOF_FSYNC_ALWAYS（每执行一个命令保存一次（不推荐））：在这种模式下，每次执行完一个命令之后，WRITE和SAVE都会被执行。另外，因为SAVE是由Redis**主进程**执行的，所以在SAVE执行期间，主进程会被阻塞，不能接受命令请求。

### 

### Redis 缓存穿透、击穿、雪崩

https://baijiahao.baidu.com/s?id=1708708550003841683&wfr=spider&for=pc

#### **缓存穿透**  (redis里不存在这个缓存key)  

​    redis 缓存和数据库中没有相关数据，redis中没有这样的数据，无法进行拦截，直接被穿透到数据库，导致数据库压力过大宕机。

**解决方案：**

* 对不存在的数据缓存到redis中，设置Key,value值为Null（不管数据为Null还是系统Bug 问题），并设置一个短期过期时间段，避免时间过长影响正常用户使用。
* 拉黑该ip 

* 对参数进行校验，不合法参数进行拦截

* 布隆过滤器，将所有可能存在的数据哈希到一个足够打的bitmap（位图中），一个一定不存在的数据会被这个bitmao 拦截掉，从而避免了对底层储存系统的查询压力。

#### **缓存击穿** （高并发量的同时key失效，导致请求直接到达数据库）

​    某一个热点key，在不停地扛着高并发，当这个热点key在时效的一瞬间，持续的高并发访问就击破缓存直接访问数据库，导致数据库宕机。

**解决方案：**

* 设置热点数据“永不过期”
* 加上互斥锁：上面现象是多个线程同时去查询数据库的这条数据局，那么我们可以在第一个查询数据的请求上使用一个互斥锁来锁住它，其他线程走到这一步拿不到锁就等着，等第一个线程查询到了数据，然后将数据放到redis 缓存起来。后面线程进来发现有缓存，就直接走缓存。



#### **雪崩** （大面积的key缓存时效）

* 限流
* 设置不同Key 值的失效时间不同，热门数据缓存时间设置长一些，反之短一些
* 一些特别热门的数据，可以设置永不过期

### Redis 命令

卸载服务：redis-server --service-uninstall

开启服务：redis-server --service-start

停止服务：redis-server --service-stop

