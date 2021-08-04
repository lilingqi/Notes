# 1、Nosql概述

## 1.1 数据库发展的历史

==一、单机Mysql时代==

![image-20210728213637693](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728213637693.png)

90年代,一个网站的访问量一般不会太大，单个数据库完全够用。随着用户增多，网站出现以下问题

​      1. 据量增加到一定程度，单机数据库就放不下了

​      2. 数据的索引（B+ Tree）,一个机器内存也存放不下

​      3. 访问量变大后（读写混合），一台服务器承受不住。

==二、Memcached(缓存) + Mysql + 垂直拆分（读写分离）==

 网站80%的情况都是在读，每次都要去查询数据库的话就十分的麻烦！所以说我们希望减轻数据库的压力，我们可以使用缓存来保证效率！

![image-20210728220419936](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728220419936.png)

优化过程经历了以下几个过程：

1. 优化数据库的数据结构和索引(难度大)

2. 文件缓存，通过IO流获取比每次都访问数据库效率略高，但是流量爆炸式增长时候，IO流也承受不了

3. MemCache,当时最热门的技术，通过在数据库和数据库访问层之间加上一层缓存，第一次访问时查询数据库，将结果保存到缓存，后续的查询先检查缓存，若有直接拿去使用，效率显著提升。

==三、分库分表 + 水平拆分 + Mysql集群==

![image-20210728220944305](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728220944305.png)

==四、近些年采用的结构==

   如今信息量井喷式增长，各种各样的数据出现（用户定位数据，图片数据等），大数据的背景下关系型数据库（RDBMS）无法满足大量数据要求。Nosql数据库就能轻松解决这些问题。

![image-20210728221100451](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728221100451.png)

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长！
这时候我们就需要使用NoSQL数据库的，Nosql可以很好的处理以上的情况！

## 1.2 什么是Nosql

  ==NoSQL = Not Only SQL（不仅仅是SQL）==

  Not Only Structured Query Language

  关系型数据库:是指采用了[关系模型](https://baike.baidu.com/item/关系模型/3189329)来组织数据的数据库，其以行和列的形式存储数据，以便于用户理解，关系型数据库这一系列的行和列被称为表，一组表组成了[数据库](https://baike.baidu.com/item/数据库/103728)。用户通过查询来检索数据库中的数据，而查询是一个用于限定数据库中某些区域的执行代码。关系模型可以简单理解为二维表格模型，而一个关系型数据库就是由[二维表](https://baike.baidu.com/item/二维表/2863955)及其之间的关系组成的一个数据组织

  非关系型数据库：数据存储没有固定的格式，并且可以进行横向扩展。

   NoSQL泛指非关系型数据库，随着web2.0互联网的诞生，传统的关系型数据库很难对付web2.0时代！尤其是超大规模的高并发的社区，暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅速，Redis是发展最快的。


## 1.3 Nosql特点

1. 方便扩展（数据之间没有关系，很好扩展！）
2. 大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）
3. 数据类型是多样型的！（不需要事先设计数据库，随取随用）

## 1.4 RDBMS和Nosql比较

```java
传统的 RDBMS(关系型数据库)
- 结构化组织
- SQL
- 数据和关系都存在单独的表中 row col
- 操作，数据定义语言
- 严格的一致性
- 基础的事务
- ...
```

```java
Nosql
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储，列存储，文档存储，图形数据库（社交关系）
- 最终一致性
- CAP定理和BASE
- 高性能，高可用，高扩展
- ...
```

## 1.5 Nosql的四大分类

1. **KV键值对**

   ```bash
   新浪：Redis
   美团：Redis + Tair
   阿里、百度：Redis + Memcache
   ```

2. **文档型数据库（bson数据格式）：**

   ```bash
   MongoDB(掌握)
     基于分布式文件存储的数据库。C++编写，用于处理大量文档。
     MongoDB是RDBMS和NoSQL的中间产品。MongoDB是非关系型数据库中功能最丰富的，NoSQL中最像关系型数据库的数据库。
   ConthDB
   ```

3. **列存储数据库**

   ```bash
   HBase(大数据必学)
   分布式文件系统
   ```

4. **图关系数据库**

   ```bash
   用于广告推荐，社交网络
   Neo4j、InfoGrid
   ```

   

![image-20210728223914848](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728223914848.png)

# 2、redis入门

## 2.1 redis概述

一、redis是什么

  Redis（Remote Dictionary Server )，即远程字典服务。

  是一个开源的使用ANSI C语言编写、支持网络、可基于内存也可以持久化的日志型、Key-Value数据库，并提供多种语言的API。

  与memcached一样，为了保证效率，数据都是缓存在内存中。**区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。**

==官网介绍：==

![image-20210728225010664](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728225010664.png)

二、redis能够做什么

1. 内存存储、持久化，内存是断电即失的，所以需要持久化（RDB、AOF）
2. 高效率、用于高速缓冲
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器(eg：浏览量)
6. 。。。。。。。。。。

三、redis的特性

1. 多样的数据类型

2. 持久化

3. 集群

4. 事务

   …

## 2.2 redis的环境搭建

官网：https://redis.io/

推荐使用Linux服务器学习。

windows版本的Redis已经停更很久了…

一、 windows安装

这是我安装的位置：

![image-20210728225745009](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728225745009.png)

![image-20210728225805004](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728225805004.png)

1. 开启redis-server.exe

2. 启动redis-cli.exe测试

   ![image-20210728225957405](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728225957405.png)



二、linux下的安装



## 2.3 测试性能

**redis-benchmark：**Redis官方提供的性能测试工具，参数选项如下：

![img](https://img-blog.csdnimg.cn/20200513214125892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

简单测试：

```bash
# 测试：100个并发连接 100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

![image-20210728231252132](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210728231252132.png)

## 2.4 基础知识

==一、redis默认有16个数据库==

![img](https://img-blog.csdnimg.cn/20200820104357466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

​    默认使用的第0个;

​    16个数据库为：DB 0~DB 15
​    默认使用DB 0 ，可以使用`select n`切换到DB n，`dbsize`可以查看当前数据库的大小，与key数量相关。

```bash
127.0.0.1:6379> config get databases # 命令行查看数据库数量databases
1) "databases"
2) "16"

127.0.0.1:6379> select 8 # 切换数据库 DB 8
OK
127.0.0.1:6379[8]> dbsize # 查看数据库大小
(integer) 0

# 不同数据库之间 数据是不能互通的，并且dbsize 是根据库中key的个数。
127.0.0.1:6379> set name sakura 
OK
127.0.0.1:6379> SELECT 8
OK
127.0.0.1:6379[8]> get name # db8中并不能获取db0中的键值对。
(nil)
127.0.0.1:6379[8]> DBSIZE
(integer) 0
127.0.0.1:6379[8]> SELECT 0
OK
127.0.0.1:6379> keys *
1) "counter:__rand_int__"
2) "mylist"
3) "name"
4) "key:__rand_int__"
5) "myset:__rand_int__"
127.0.0.1:6379> DBSIZE # size和key个数相关
(integer) 5
```

```bash
keys * ：查看当前数据库中所有的key。

flushdb：清空当前数据库中的键值对。

flushall：清空所有数据库的键值对。
```

==二、**Redis是单线程的，Redis是基于内存操作的。**==

所以Redis的性能瓶颈不是CPU,而是机器内存和网络带宽。

那么为什么Redis的速度如此快呢，性能这么高呢？QPS达到10W+

**Redis为什么单线程还这么快？**

```bash
误区1：高性能的服务器一定是多线程的？
误区2：多线程（CPU上下文会切换！）一定比单线程效率高！
核心：Redis是将所有的数据放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作！），对于内存系统来说，如果没有上下文切换效率就是最高的，多次读写都是在一个CPU上的，在内存存储数据情况下，单线程就是最佳的方案。
```

# 3、五大数据类型

​    Redis是一个开源（BSD许可），内存存储的数据结构服务器，可用作数据库，高速缓存和消息队列代理。它支持字符串、哈希表、列表、集合、有序集合，位图，hyperloglogs等数据类型。内置复制、Lua脚本、LRU收回、事务以及不同级别磁盘持久化功能，同时通过Redis Sentinel提供高可用，通过Redis Cluster提供自动分区。

## 3.1 Redis-key

```bash
在redis中无论什么数据类型，在数据库中都是以key-value形式保存，通过进行对Redis-key的操作，来完成对数据库中数据的操作。
```

==**关于key的常规操作：**==

~~~bash
set key value 设置键值对
get key 获取value
exists key 判断键是否存在
del key 删除键值对
move key db 将键移动到指定数据库
expire key second：设置键值对的过期时间
type key 查看value的数据类型
RENAME key newkey 修改key的名称
RENAMENX key newkey 仅当newkey不存在的时候，将key改名为newkey
~~~

示例：

~~~bash
127.0.0.1:6379> keys * # 查看当前数据库所有key
(empty list or set)
127.0.0.1:6379> set name qinjiang # set key
OK
127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> move age 1 # 将键值对移动到指定数据库
(integer) 1
127.0.0.1:6379> EXISTS age # 判断键是否存在
(integer) 0 # 不存在
127.0.0.1:6379> EXISTS name
(integer) 1 # 存在
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> keys *
1) "age"
127.0.0.1:6379[1]> del age # 删除键值对
(integer) 1 # 删除个数


127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> EXPIRE age 15 # 设置键值对的过期时间

(integer) 1 # 设置成功 开始计数
127.0.0.1:6379> ttl age # 查看key的过期剩余时间
(integer) 13
127.0.0.1:6379> ttl age
(integer) 11
127.0.0.1:6379> ttl age
(integer) 9
127.0.0.1:6379> ttl age
(integer) -2 # -2 表示key过期，-1表示key未设置过期时间

127.0.0.1:6379> get age # 过期的key 会被自动delete
(nil)
127.0.0.1:6379> keys *
1) "name"

127.0.0.1:6379> type name # 查看value的数据类型
string
~~~

关于ttl命令

 redis的key，通过ttl命令来返回key的过期时间，一般有三种情况：

1. 如果没有设置过期时间，返回-1

2. 如果设置了过期时间而且已经过期，就会返回-2

3. 如果设置了过期时间，还没有过期就会返回剩余的时间

更多命令：https://www.redis.net.cn/order/

## 3.2 String（字符串）

![image-20210729093402370](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210729093402370.png)

String类似的使用场景：value除了是字符还可以是数字

- 计数器
- 统计多单位的数量：uid:123666：follow 0
- 粉丝数
- 对象存储缓存

## 3.3 List（列表）

  **Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）列表最多可存储$$ 2^{32}- 1$$ 元素 (4294967295, 每个列表可存储40多亿)。**

==**首先我们列表，可以经过规则定义将其变为队列、栈、双端队列等**==

![img](https://img-blog.csdnimg.cn/20200820104440398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

==**正如图Redis中List是可以进行双端操作的，所以命令也就分为了LXXX和RXXX两类，有时候L也表示List例如LLEN**==

![image-20210729095322771](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210729095322771.png)

~~~bash
---------------------------LPUSH---RPUSH---LRANGE--------------------------------

127.0.0.1:6379> LPUSH mylist k1 # LPUSH mylist=>{1}
(integer) 1
127.0.0.1:6379> LPUSH mylist k2 # LPUSH mylist=>{2,1}
(integer) 2
127.0.0.1:6379> RPUSH mylist k3 # RPUSH mylist=>{2,1,3}
(integer) 3
127.0.0.1:6379> get mylist # 普通的get是无法获取list值的
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> LRANGE mylist 0 4 # LRANGE 获取起止位置范围内的元素
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> LRANGE mylist 0 2
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> LRANGE mylist 0 1
1) "k2"
2) "k1"
127.0.0.1:6379> LRANGE mylist 0 -1 # 获取全部元素
1) "k2"
2) "k1"
3) "k3"

---------------------------LPUSHX---RPUSHX-----------------------------------

127.0.0.1:6379> LPUSHX list v1 # list不存在 LPUSHX失败
(integer) 0
127.0.0.1:6379> LPUSHX list v1 v2  
(integer) 0
127.0.0.1:6379> LPUSHX mylist k4 k5 # 向mylist中 左边 PUSH k4 k5
(integer) 5
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "k1"
5) "k3"

---------------------------LINSERT--LLEN--LINDEX--LSET----------------------------

127.0.0.1:6379> LINSERT mylist after k2 ins_key1 # 在k2元素后 插入ins_key1
(integer) 6
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "ins_key1"
5) "k1"
6) "k3"
127.0.0.1:6379> LLEN mylist # 查看mylist的长度
(integer) 6
127.0.0.1:6379> LINDEX mylist 3 # 获取下标为3的元素
"ins_key1"
127.0.0.1:6379> LINDEX mylist 0
"k5"
127.0.0.1:6379> LSET mylist 3 k6 # 将下标3的元素 set值为k6
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "k6"
5) "k1"
6) "k3"

---------------------------LPOP--RPOP--------------------------

127.0.0.1:6379> LPOP mylist # 左侧(头部)弹出
"k5"
127.0.0.1:6379> RPOP mylist # 右侧(尾部)弹出
"k3"

---------------------------RPOPLPUSH--------------------------

127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
3) "k6"
4) "k1"
127.0.0.1:6379> RPOPLPUSH mylist newlist # 将mylist的最后一个值(k1)弹出，加入到newlist的头部
"k1"
127.0.0.1:6379> LRANGE newlist 0 -1
1) "k1"
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
3) "k6"

---------------------------LTRIM--------------------------

127.0.0.1:6379> LTRIM mylist 0 1 # 截取mylist中的 0~1部分
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"

# 初始 mylist: k2,k2,k2,k2,k2,k2,k4,k2,k2,k2,k2
---------------------------LREM--------------------------

127.0.0.1:6379> LREM mylist 3 k2 # 从头部开始搜索 至多删除3个 k2
(integer) 3
# 删除后：mylist: k2,k2,k2,k4,k2,k2,k2,k2

127.0.0.1:6379> LREM mylist -2 k2 #从尾部开始搜索 至多删除2个 k2
(integer) 2
# 删除后：mylist: k2,k2,k2,k4,k2,k2


---------------------------BLPOP--BRPOP--------------------------

mylist: k2,k2,k2,k4,k2,k2
newlist: k1

127.0.0.1:6379> BLPOP newlist mylist 30 # 从newlist中弹出第一个值，mylist作为候选
1) "newlist" # 弹出
2) "k1"
127.0.0.1:6379> BLPOP newlist mylist 30
1) "mylist" # 由于newlist空了 从mylist中弹出
2) "k2"
127.0.0.1:6379> BLPOP newlist 30
(30.10s) # 超时了

127.0.0.1:6379> BLPOP newlist 30 # 我们连接另一个客户端向newlist中push了test, 阻塞被解决。
1) "newlist"
2) "test"
(12.54s)
~~~

==**链表小结：**==

~~~bash
list实际上是一个链表，before Node after , left, right 都可以插入值

如果key不存在，则创建新的链表

如果key存在，新增内容

如果移除了所有值，空链表，也代表不存在

在两边插入或者改动值，效率最高！修改中间元素，效率相对较低
~~~

**应用：**

**消息排队！消息队列（Lpush Rpop）,栈（Lpush Lpop）**

## 3.4 Set（集合）

  Redis的Set是string类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

  Redis 中 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

  集合中最大的成员数为 $$2^{32} - 1$$ (4294967295, 每个集合可存储40多亿个成员)。
![image-20210729102541877](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210729102541877.png)

~~~bash
---------------SADD--SCARD--SMEMBERS--SISMEMBER--------------------

127.0.0.1:6379> SADD myset m1 m2 m3 m4 # 向myset中增加成员 m1~m4
(integer) 4
127.0.0.1:6379> SCARD myset # 获取集合的成员数目
(integer) 4
127.0.0.1:6379> smembers myset # 获取集合中所有成员
1) "m4"
2) "m3"
3) "m2"
4) "m1"
127.0.0.1:6379> SISMEMBER myset m5 # 查询m5是否是myset的成员
(integer) 0 # 不是，返回0
127.0.0.1:6379> SISMEMBER myset m2
(integer) 1 # 是，返回1
127.0.0.1:6379> SISMEMBER myset m3
(integer) 1

---------------------SRANDMEMBER--SPOP----------------------------------

127.0.0.1:6379> SRANDMEMBER myset 3 # 随机返回3个成员
1) "m2"
2) "m3"
3) "m4"
127.0.0.1:6379> SRANDMEMBER myset # 随机返回1个成员
"m3"
127.0.0.1:6379> SPOP myset 2 # 随机移除并返回2个成员
1) "m1"
2) "m4"
# 将set还原到{m1,m2,m3,m4}

---------------------SMOVE--SREM----------------------------------------

127.0.0.1:6379> SMOVE myset newset m3 # 将myset中m3成员移动到newset集合
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "m4"
2) "m2"
3) "m1"
127.0.0.1:6379> SMEMBERS newset
1) "m3"
127.0.0.1:6379> SREM newset m3 # 从newset中移除m3元素
(integer) 1
127.0.0.1:6379> SMEMBERS newset
(empty list or set)

# 下面开始是多集合操作,多集合操作中若只有一个参数默认和自身进行运算
# setx=>{m1,m2,m4,m6}, sety=>{m2,m5,m6}, setz=>{m1,m3,m6}

-----------------------------SDIFF------------------------------------

127.0.0.1:6379> SDIFF setx sety setz # 等价于setx-sety-setz
1) "m4"
127.0.0.1:6379> SDIFF setx sety # setx - sety
1) "m4"
2) "m1"
127.0.0.1:6379> SDIFF sety setx # sety - setx
1) "m5"


-------------------------SINTER---------------------------------------
# 共同关注（交集）

127.0.0.1:6379> SINTER setx sety setz # 求 setx、sety、setx的交集
1) "m6"
127.0.0.1:6379> SINTER setx sety # 求setx sety的交集
1) "m2"
2) "m6"

-------------------------SUNION---------------------------------------

127.0.0.1:6379> SUNION setx sety setz # setx sety setz的并集
1) "m4"
2) "m6"
3) "m3"
4) "m2"
5) "m1"
6) "m5"
127.0.0.1:6379> SUNION setx sety # setx sety 并集
1) "m4"
2) "m6"
3) "m2"
4) "m1"
5) "m5"
~~~

## 3.5 Hash(哈希)

Redis hash 是一个string类型的field和value的映射表，==**hash特别适合用于存储对象。**==

Set就是一种简化的Hash,只变动key,而value使用默认值填充。可以将一个Hash表作为一个对象进行存储，表中存放对象的信息

![image-20210729103223279](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210729103223279.png)

~~~bash
------------------------HSET--HMSET--HSETNX----------------
127.0.0.1:6379> HSET studentx name sakura # 将studentx哈希表作为一个对象，设置name为sakura
(integer) 1
127.0.0.1:6379> HSET studentx name gyc # 重复设置field进行覆盖，并返回0
(integer) 0
127.0.0.1:6379> HSET studentx age 20 # 设置studentx的age为20
(integer) 1
127.0.0.1:6379> HMSET studentx sex 1 tel 15623667886 # 设置sex为1，tel为15623667886
OK
127.0.0.1:6379> HSETNX studentx name gyc # HSETNX 设置已存在的field
(integer) 0 # 失败
127.0.0.1:6379> HSETNX studentx email 12345@qq.com
(integer) 1 # 成功

----------------------HEXISTS--------------------------------
127.0.0.1:6379> HEXISTS studentx name # name字段在studentx中是否存在
(integer) 1 # 存在
127.0.0.1:6379> HEXISTS studentx addr
(integer) 0 # 不存在

-------------------HGET--HMGET--HGETALL-----------
127.0.0.1:6379> HGET studentx name # 获取studentx中name字段的value
"gyc"
127.0.0.1:6379> HMGET studentx name age tel # 获取studentx中name、age、tel字段的value
1) "gyc"
2) "20"
3) "15623667886"
127.0.0.1:6379> HGETALL studentx # 获取studentx中所有的field及其value
 1) "name"
 2) "gyc"
 3) "age"
 4) "20"
 5) "sex"
 6) "1"
 7) "tel"
 8) "15623667886"
 9) "email"
10) "12345@qq.com"


--------------------HKEYS--HLEN--HVALS--------------
127.0.0.1:6379> HKEYS studentx # 查看studentx中所有的field
1) "name"
2) "age"
3) "sex"
4) "tel"
5) "email"
127.0.0.1:6379> HLEN studentx # 查看studentx中的字段数量
(integer) 5
127.0.0.1:6379> HVALS studentx # 查看studentx中所有的value
1) "gyc"
2) "20"
3) "1"
4) "15623667886"
5) "12345@qq.com"

-------------------------HDEL--------------------------
127.0.0.1:6379> HDEL studentx sex tel # 删除studentx 中的sex、tel字段
(integer) 2
127.0.0.1:6379> HKEYS studentx
1) "name"
2) "age"
3) "email"

-------------HINCRBY--HINCRBYFLOAT------------------------
127.0.0.1:6379> HINCRBY studentx age 1 # studentx的age字段数值+1
(integer) 21
127.0.0.1:6379> HINCRBY studentx name 1 # 非整数字型字段不可用
(error) ERR hash value is not an integer
127.0.0.1:6379> HINCRBYFLOAT studentx weight 0.6 # weight字段增加0.6
"90.8"
####################
#hash变更的数据user name age，尤其是用户信息之类的，经常变动的信息！Hash更适合对象的存储，String更适合字符串的存储！
~~~

## 3.6 Zset（有序集合）

==**不同的是每个元素都会关联一个double类型的分数（score**）==。redis正是通过分数来为集合中的成员进行从小到大的排序。

score相同：按字典顺序排序

有序集合的成员是唯一的,但分数(score)却可以重复。

![image-20210729104220125](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210729104220125.png)

~~~bash
-------------------ZADD--ZCARD--ZCOUNT--------------
127.0.0.1:6379> ZADD myzset 1 m1 2 m2 3 m3 # 向有序集合myzset中添加成员m1 score=1 以及成员m2 score=2..
(integer) 2
127.0.0.1:6379> ZCARD myzset # 获取有序集合的成员数
(integer) 2
127.0.0.1:6379> ZCOUNT myzset 0 1 # 获取score在 [0,1]区间的成员数量
(integer) 1
127.0.0.1:6379> ZCOUNT myzset 0 2
(integer) 2

----------------ZINCRBY--ZSCORE--------------------------
127.0.0.1:6379> ZINCRBY myzset 5 m2 # 将成员m2的score +5
"7"
127.0.0.1:6379> ZSCORE myzset m1 # 获取成员m1的score
"1"
127.0.0.1:6379> ZSCORE myzset m2
"7"

--------------ZRANK--ZRANGE-----------------------------------
127.0.0.1:6379> ZRANK myzset m1 # 获取成员m1的索引，索引按照score排序，score相同索引值按字典顺序顺序增加
(integer) 0
127.0.0.1:6379> ZRANK myzset m2
(integer) 2
127.0.0.1:6379> ZRANGE myzset 0 1 # 获取索引在 0~1的成员
1) "m1"
2) "m3"
127.0.0.1:6379> ZRANGE myzset 0 -1 # 获取全部成员
1) "m1"
2) "m3"
3) "m2"

#testset=>{abc,add,amaze,apple,back,java,redis} score均为0
------------------ZRANGEBYLEX---------------------------------
127.0.0.1:6379> ZRANGEBYLEX testset - + # 返回所有成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
5) "back"
6) "java"
7) "redis"
127.0.0.1:6379> ZRANGEBYLEX testset - + LIMIT 0 3 # 分页 按索引显示查询结果的 0,1,2条记录
1) "abc"
2) "add"
3) "amaze"
127.0.0.1:6379> ZRANGEBYLEX testset - + LIMIT 3 3 # 显示 3,4,5条记录
1) "apple"
2) "back"
3) "java"
127.0.0.1:6379> ZRANGEBYLEX testset (- [apple # 显示 (-,apple] 区间内的成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
127.0.0.1:6379> ZRANGEBYLEX testset [apple [java # 显示 [apple,java]字典区间的成员
1) "apple"
2) "back"
3) "java"

-----------------------ZRANGEBYSCORE---------------------
127.0.0.1:6379> ZRANGEBYSCORE myzset 1 10 # 返回score在 [1,10]之间的的成员
1) "m1"
2) "m3"
3) "m2"
127.0.0.1:6379> ZRANGEBYSCORE myzset 1 5
1) "m1"
2) "m3"

--------------------ZLEXCOUNT-----------------------------
127.0.0.1:6379> ZLEXCOUNT testset - +
(integer) 7
127.0.0.1:6379> ZLEXCOUNT testset [apple [java
(integer) 3

------------------ZREM--ZREMRANGEBYLEX--ZREMRANGBYRANK--ZREMRANGEBYSCORE--------------------------------
127.0.0.1:6379> ZREM testset abc # 移除成员abc
(integer) 1
127.0.0.1:6379> ZREMRANGEBYLEX testset [apple [java # 移除字典区间[apple,java]中的所有成员
(integer) 3
127.0.0.1:6379> ZREMRANGEBYRANK testset 0 1 # 移除排名0~1的所有成员
(integer) 2
127.0.0.1:6379> ZREMRANGEBYSCORE myzset 0 3 # 移除score在 [0,3]的成员
(integer) 2


# testset=> {abc,add,apple,amaze,back,java,redis} score均为0
# myzset=> {(m1,1),(m2,2),(m3,3),(m4,4),(m7,7),(m9,9)}
----------------ZREVRANGE--ZREVRANGEBYSCORE--ZREVRANGEBYLEX-----------
127.0.0.1:6379> ZREVRANGE myzset 0 3 # 按score递减排序，然后按索引，返回结果的 0~3
1) "m9"
2) "m7"
3) "m4"
4) "m3"
127.0.0.1:6379> ZREVRANGE myzset 2 4 # 返回排序结果的 索引的2~4
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> ZREVRANGEBYSCORE myzset 6 2 # 按score递减顺序 返回集合中分数在[2,6]之间的成员
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> ZREVRANGEBYLEX testset [java (add # 按字典倒序 返回集合中(add,java]字典区间的成员
1) "java"
2) "back"
3) "apple"
4) "amaze"

-------------------------ZREVRANK------------------------------
127.0.0.1:6379> ZREVRANK myzset m7 # 按score递减顺序，返回成员m7索引
(integer) 1
127.0.0.1:6379> ZREVRANK myzset m2
(integer) 4


# mathscore=>{(xm,90),(xh,95),(xg,87)} 小明、小红、小刚的数学成绩
# enscore=>{(xm,70),(xh,93),(xg,90)} 小明、小红、小刚的英语成绩
-------------------ZINTERSTORE--ZUNIONSTORE-----------------------------------
127.0.0.1:6379> ZINTERSTORE sumscore 2 mathscore enscore # 将mathscore enscore进行合并 结果存放到sumscore
(integer) 3
127.0.0.1:6379> ZRANGE sumscore 0 -1 withscores # 合并后的score是之前集合中所有score的和
1) "xm"
2) "160"
3) "xg"
4) "177"
5) "xh"
6) "188"

127.0.0.1:6379> ZUNIONSTORE lowestscore 2 mathscore enscore AGGREGATE MIN # 取两个集合的成员score最小值作为结果的
(integer) 3
127.0.0.1:6379> ZRANGE lowestscore 0 -1 withscores
1) "xm"
2) "70"
3) "xg"
4) "87"
5) "xh"
6) "93"
~~~

==应用场景：==

~~~bash
set排序 存储班级成绩表 工资表排序！
普通消息，1.重要消息 2.带权重进行判断
排行榜应用实现，取Top N测试
~~~

# 4、三种特殊的数据类型

## 4.1 Geospatial（地理位置）

==使用经纬度定位地理坐标并用一个**有序集合zset保存**，所以zset命令也可以使用==

![image-20210729105640177](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210729105640177.png)

有效经纬度

```bash
有效的经度从-180度到180度。
有效的纬度从-85.05112878度到85.05112878度。
```

指定的单位参数unit必须是一下单位的其中一个

~~~bash
m 表示单位为米。

km 表示单位为千米。

mi 表示单位为英里。

ft 表示单位为英尺。
~~~

关于GEORADIUS的参数

~~~bash
通过georadius就可以完成 附近的人功能

withcoord:带上坐标

withdist:带上距离，单位与半径单位相同

COUNT n : 只显示前n个(按距离递增排序)
~~~

~~~bash
----------------georadius---------------------
127.0.0.1:6379> GEORADIUS china:city 120 30 500 km withcoord withdist # 查询经纬度(120,30)坐标500km半径内的成员
1) 1) "hangzhou"
   2) "29.4151"
   3) 1) "120.20000249147415"
      2) "30.199999888333501"
2) 1) "shanghai"
   2) "205.3611"
   3) 1) "121.40000134706497"
      2) "31.400000253193539"
     
------------geohash---------------------------
127.0.0.1:6379> geohash china:city yichang shanghai # 获取成员经纬坐标的geohash表示
1) "wmrjwbr5250"
2) "wtw6ds0y300"

~~~

## 4.2 Hyperloglog(基数统计)

​      Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。花费 12 KB 内存，就可以计算接近 $$2^{64}$$ 个不同元素的基数。==**因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身**==，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。其底层使用string数据类型。


**什么是基数：**数据集中不重复的元素的个数。



**应用场景：**网页的访问量。一个用户多次访问也只能算作一个人。

传统实现：每一次用户访问的时候，存储用户的id，然后每次有人访问的时候都进行id比较，但是用户变多的时候这种方式极其的浪费空间，而我们的目的只是计数，Hyperloglog就能帮助我们利用最小空间完成。

![image-20210730090931632](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730090931632.png)

```BASH
127.0.0.1:6379> pfadd test1 1 2 3 4 5 #添加元素
(integer) 1
127.0.0.1:6379> pfcount test1#估算test1的基数
(integer) 5
127.0.0.1:6379> pfadd test2 4 5 6 7 8
(integer) 1
127.0.0.1:6379> pfcount test2
(integer) 5
127.0.0.1:6379> pfcount test1 test2 #这个是将test1和test2合并后的基数计算出来了
(integer) 8
127.0.0.1:6379> pfmerge test3 test1 test2#将test1和test2合并成test3
OK
127.0.0.1:6379> pfcount test3#计算test3的基数
(integer) 8
127.0.0.1:6379> pfcount test1
(integer) 5
127.0.0.1:6379> pfcount test2
(integer) 5
127.0.0.1:6379> type test1#hyperloglog的底层采用string类型
string
127.0.0.1:6379> 
```

**如果允许容错，那么一定可以使用Hyperloglog !**

**如果不允许容错，就使用set或者自己的数据类型即可 ！**

## 4.3 BitMaps（位图）

  使用位存储，信息状态只有 0 和 1， Bitmap是一串连续的2进制数字（0或1），每一位所在的位置为偏移(offset)，在bitmap上可执行AND,OR,XOR,NOT以及其它位操作。

**应用场景**

签到统计、状态统计

![image-20210730093541863](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730093541863.png)

~~~bash
------------setbit--getbit--------------
127.0.0.1:6379> setbit sign 0 1 # 设置sign的第0位为 1 
(integer) 0
127.0.0.1:6379> setbit sign 2 1 # 设置sign的第2位为 1  不设置默认 是0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 5 1
(integer) 0
127.0.0.1:6379> type sign#底层存储是string
string

127.0.0.1:6379> getbit sign 2 # 获取第2位的数值
(integer) 1
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 4 # 未设置默认是0
(integer) 0

-----------bitcount----------------------------
127.0.0.1:6379> BITCOUNT sign # 统计sign中为1的位数
(integer) 4
~~~

# 5、事务

## 5.1 特性和本质

Redis的单条命令是保证原子性的，但是redis的事务不保证原子性,Redis事务没有隔离级别的概念。



```java
原子性是指事务是一个不可再分割的工作单元，事务中的操作要么都发生，要么都不发生。

可采用“A向B转账”这个例子来说明解释

RDBMS中，默认情况下一条SQL就是一个单独事务，事务是自动提交的。只有显式的使用start transaction开启一个事务，才能将一个代码块放在事务中执行。
```

**Redis事务的本质:**一组命令的集合

事务中每条命令都会被序列化，执行过程中按照顺序执行，不允许其他的命令进行干扰。

1. 一次性
2. 顺序性
3. 排他性



## 5.2 Redis事务的操作过程

1. 开启事务（multi）
2. 命令入队
3. 执行事务（exec）

所有命令在加入的时候都是没有执行的，必须在第三步exec命令执行之后才会一次性执行完。

```bash
127.0.0.1:6379> multi # 开启事务
OK
127.0.0.1:6379> set k1 v1 # 命令入队
QUEUED
127.0.0.1:6379> set k2 v2 # ..
QUEUED
127.0.0.1:6379> get k1
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> keys *
QUEUED
127.0.0.1:6379> exec # 事务执行
1) OK
2) OK
3) "v1"
4) OK
5) 1) "k3"
   2) "k2"
   3) "k1"
```

**取消事务（discard）**

~~~bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> DISCARD # 放弃事务
OK
127.0.0.1:6379> EXEC 
(error) ERR EXEC without MULTI # 当前未开启事务
127.0.0.1:6379> get k1 # 被放弃事务中命令并未执行
(nil)
~~~

## 5.3 事务错误

情况一：代码语法错误（编译时异常）所有命令都不执行

~~~bash
127.0.0.1:6379> multi #开启事务
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2 
QUEUED
127.0.0.1:6379(TX)> get k1
QUEUED
127.0.0.1:6379(TX)> error k1#一条错误指令
(error) ERR unknown command `error`, with args beginning with: `k1`, #虽然报错了，但是不影响后面的代码入队
127.0.0.1:6379(TX)> exec 
(error) EXECABORT Transaction discarded because of previous errors.#执行报错
127.0.0.1:6379> get k1#这些命令都没有被执行
(nil)
~~~

情况二：代码逻辑错误（运行时异常）其他的命令都可以正常执行----------这里就证明了redis的事务不保证原子性

~~~bash
127.0.0.1:6379> set k1 v1 #设置k1为字符型数据
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> incr k1 #对k1进行自增，这里因为k1里面是字符型数据，运行时候会报错
QUEUED
127.0.0.1:6379(TX)> get k1 #数据可以正常获取，说明运行时错误不影响后面的命令执行
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> exec
1) OK
2) OK
3) (error) ERR value is not an integer or out of range
4) "v1"
5) "v2"
# 虽然中间有一条命令报错了，但是后面的指令依旧正常执行成功了。
# 所以说Redis单条指令保证原子性，但是Redis事务不能保证原子性。
~~~

## 5.4 监控

**悲观锁：**

- 很悲观，认为什么时候都会出现问题，无论做什么都会加锁

**乐观锁：**

- 很乐观，认为什么时候都不会出现问题，所以不会上锁！更新数据的时候去判断一下，在此期间是否有人修改过这个数据
- 获取version
- 更新的时候比较version

使用==watch key==监控指定数据，相当于乐观锁加锁。

**正常执行：**模拟转账操作（没有外界修改数据）

~~~bash
127.0.0.1:6379> set llq 100
OK
127.0.0.1:6379> set hx 0
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> decrby llq 20
QUEUED
127.0.0.1:6379(TX)> incrby hx 20
QUEUED
127.0.0.1:6379(TX)> get llq
QUEUED
127.0.0.1:6379(TX)> get hx
QUEUED
127.0.0.1:6379(TX)> exec
1) (integer) 80
2) (integer) 20
3) "80"
4) "20"
~~~

**非正常情况：**测试多线程修改值，使用watch可以当做redis的乐观锁操作（相当于getversion）

~~~bash
-----------------------线程1--------------------------------------
127.0.0.1:6379> set llq 100
OK
127.0.0.1:6379> set hx 10
OK
127.0.0.1:6379> watch llq
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> decrby llq 20
QUEUED
127.0.0.1:6379(TX)> incrby hx 20
QUEUED
127.0.0.1:6379(TX)> get llq
QUEUED
127.0.0.1:6379(TX)> get hx #此时事务还没有执行
~~~

~~~bash
------------------------线程2-------------------------------------------
127.0.0.1:6379> incrby llq 10 #模拟线程2插队来改变llq的余额
(integer) 110
~~~

~~~bash
------------------------线程1---------------------------------------------
127.0.0.1:6379(TX)> exec #事务执行失败，队列中的命令都没有执行，因为watch检测到llq的余额发生了变化
(nil)  
127.0.0.1:6379> get llq
"110"
~~~

解锁获取最新值，然后再加锁进行事务。==unwatch==进行解锁。

==注意：每次提交执行exec后都会自动释放锁，不管是否成功==

# 6、jedis



#  7、SpringBoot整合redis

# 8、自定义Redis工具类

# 9、Redis.conf

**容量单位不区分大小写，G和GB有区别**

![img](https://img-blog.csdnimg.cn/2020051321485460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)



**可以使用 include 组合多个配置问题:**

![img](https://img-blog.csdnimg.cn/20200513214902552.png)



**网络配置:**

![img](https://img-blog.csdnimg.cn/20200513214912813.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)



**日志输出级别:**

![img](https://img-blog.csdnimg.cn/20200513214923678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)



**日志输出文件:**

![img](https://img-blog.csdnimg.cn/20200513214933713.png)



**持久化:**

由于Redis是基于内存的数据库，需要将数据由内存持久化到文件中

持久化方式：

- RDB
- AOF

**RDB相关部分：**

![image-20210730112223110](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730112223110.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214944964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20200513214955679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)



**AOF相关部分：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215037918.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215047999.png)



**主从复制：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215016371.png)



**Security模块中进行密码设置：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215026143.png)



**客户端连接相关：**

~~~bash
maxclients 10000  最大客户端数量
maxmemory <bytes> 最大内存限制
maxmemory-policy noeviction # 内存达到限制值的处理策略
~~~



**过期政策：**

redis 中的**默认**的过期策略是 **volatile-lru** 。

**设置方式**

~~~bash
config set maxmemory-policy volatile-lru 
~~~

maxmemory-policy 六种方式

~~~bash
volatile-lru：只对设置了过期时间的key进行LRU（默认值）

allkeys-lru ： 删除lru算法的key

volatile-random：随机删除即将过期key

allkeys-random：随机删除

volatile-ttl ：删除即将过期的

noeviction ： 永不过期，返回错误
~~~

# 10、持久化RDB

**RDB：Redis Databases**

## 10.1 什么是RDB

在指定时间间隔后，将数据集中的数据库**快照**写入数据库

**快照：**快照，顾名思义可以理解为拍照一样，把整个内存数据映射到硬盘中，保存一份到硬盘，因此恢复数据起来比较快，把数据映射回去即可，不像AOF，一条条的执行操作命令。具体参考https://www.cnblogs.com/zhenghongxin/p/8669913.html

## 10.2 工作原理和过程



![image-20210730141402559](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730141402559.png)

​    Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。我们默认的就是RDB，一般情况下不需要修改这个配置！

==**这种工作方式使得 Redis 可以从写时复制（copy-on-write）机制中获益(因为是使用子进程进行写操作，而父进程依然可以接收来自客户端的请求。)**==

![image-20210730141731695](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730141731695.png)

## 10.3 相关文件配置

==**rdb保存的文件是dump.rdb**  ===都是在我们的配置文件中快照中进行配置的！

![image-20210730142209041](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730142209041.png)

## 10.4 触发机制

1. save的规则满足的情况下，会自动触发rdb原则
2. 执行flushall命令，也会触发我们的rdb原则
3. 退出redis，也会自动产生rdb文件

4. **save命令：**

​    使用 `save` 命令，会立刻对当前内存中的数据进行持久化 ,但是会阻塞，也就是不接受其他操作了；

​    由于 `save` 命令是同步命令，会占用Redis的主进程。若Redis数据非常多时，`save`命令执行速度会非常慢，阻塞所有客户端的请求。

![image-20210730142609467](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730142609467.png)

5. bgsave

   bgsave是异步进行，进行持久化的时候，redis还可以继续相应客户端的请求

   ![image-20210730142829181](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730142829181.png)

**save命令和bgsave命令的对比**

![image-20210730142926183](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730142926183.png)

## 10.5 优缺点

优点：

1. 适合大规模的数据恢复
2. 对数据的完整性要求不高

缺点：

1. 需要一定的时间间隔进行操作，如果redis意外宕机了，这个最后一次修改的数据就没有了。
2. fork进程的时候，会占用一定的内容空间。

# 11、持久化AOF

**AOF(Append Only File):**将我们所有的命令都记录下来，history，恢复的时候就把这个文件全部再执行一遍

## 11.1 什么是AOF

![image-20210730144022549](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730144022549.png)

​    **以日志的形式来记录每个写操作，将Redis执行过的所有指令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。**

## 11.2 相关文件配置

AOF生成的是 appendonly.aof 文件来保存这些数据

![img](https://img-blog.csdnimg.cn/20200513215247113.png)

`appendonly no yes`则表示启用AOF

默认是不开启的，我们需要手动配置，然后重启redis，就可以生效了！

如果我们人为的将生成的aof文件给改动了，那么redis是启动不起来的！！！！！！！

redis 给我们提供了一个工具=== redis-check-aof --fix==

![image-20210730144742920](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730144742920.png)



**重写规则说明：**aof 默认就是文件的无限追加，文件会越来越大！

![image-20210730144913149](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730144913149.png)

如果 aof 文件大于 64m，太大了！ fork一个新的进程来将我们的文件进行重写！

![image-20210730145049858](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730145049858.png)

## 11.3 优缺点

优点：

1. 每一次修改都同步，文件的完整会更加好！
2. 每秒同步一次，可能会丢失一秒的数据
3. 从不同步，效率最高的！

缺点：

1. 相对于数据文件来说，aof是远远大于rdb的，修复的速度也比rdb慢。
2. aof的运行效率页比rdb慢，所以我们redis默认的就是rdb持久化。



## 11.4 RDB和AOF的选择

**扩展：**

1、RDB 持久化方式能够在指定的时间间隔内对你的数据进行快照存储
2、AOF 持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。
3、只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化
4、同时开启两种持久化方式
         在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。RDB 的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。
5、性能建议
         因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留 save 900 1 这条规则。如果Enable AOF ，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。如果不Enable AOF ，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统波动。代价是如果Master/Slave 同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个 Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。



**对比：**

![image-20210730151950283](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730151950283.png)

**那么应该选择哪种持久化方式呢？**

​    一般来说， 如果想达到足以媲美 PostgreSQL 的数据安全性， 你应该同时使用两种持久化功能。

​    如果你非常关心你的数据， 但仍然可以承受数分钟以内的数据丢失， 那么你可以只使用 RDB 持久化。

​    有很多用户都只使用 AOF 持久化， 但并不推荐这种方式： 因为定时生成 RDB 快照（snapshot）非常便于进行数据库备份， 并且 RDB 恢复数据集的速度也要比   AOF 恢复的速度要快。

# 12、发布和订阅

##  12.1 概念和基本过程

  Redis 发布订阅(pub/sub)是一种==**消息通信模式**==：发送者(pub)发送消息，订阅者(sub)接收消息。微信、微博、关注系统！
  Redis 客户端可以订阅任意数量的频道。

  **订阅/发布消息图：**

![image-20210730152601779](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730152601779.png)

**下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：**

![image-20210730152657872](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730152657872.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![image-20210730152720506](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730152720506.png)

## 12.2 相关命令

这些命令被广泛用于构建即时通信应用，比如网络聊天室(chatroom)和实时广播、实时提醒等。

![image-20210730152838290](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730152838290.png)

~~~bash
-----------------------------------------订阅-------------------------------------
127.0.0.1:6379> subscribe cctv  #订阅一个叫cctv的频道
Reading messages... (press Ctrl-C to quit)
1) "subscribe" #订阅成功信息
2) "cctv"
3) (integer) 1
1) "message"  #接受来自cctv的信息hello world
2) "cctv"
3) "hello world"
-----------------------------------------发布----------------------------------------
127.0.0.1:6379> publish cctv "hello world" #将信息发布到cctv
(integer) 1

~~~

## 12.3 原理

   每个 Redis 服务器进程都维持着一个表示服务器状态的 redis.h/redisServer 结构， 结构的 pubsub_channels 属性是一个字典， 这个字典就用于保存订阅频道的信息，其中，字典的键为正在被订阅的频道， 而字典的值则是一个链表， 链表中保存了所有订阅这个频道的客户端。
![image-20210730153855803](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730153855803.png)

客户端订阅，就被链接到对应频道的链表的尾部，退订则就是将客户端节点从链表中移除。

## 12.4 缺点

1. 如果一个客户端订阅了频道，但自己读取消息的速度却不够快的话，那么不断积压的消息会使redis输出缓冲区的体积变得越来越大，这可能使得redis本身的速度变慢，甚至直接崩溃。
2. 这和数据传输可靠性有关，如果在订阅方断线，那么他将会丢失所有在短线期间发布者发布的消息。

## 12.5 应用

1. 消息订阅：公众号订阅，微博关注等等（起始更多是使用消息队列来进行实现）
2. 多人在线聊天室。

​       稍微复杂的场景，我们就会使用消息中间件MQ处理。

# 13、Redis主从复制

## 13.1 概念

​      主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点（Master/Leader）,后者称为从节点（Slave/Follower）， 数据的复制是单向的！只能由主节点复制到从节点（主节点以写为主、从节点以读为主）。

​      默认情况下，每台Redis服务器都是主节点，一个主节点可以有0个或者多个从节点，但每个从节点只能有一个主节点。


## 13.2 作用

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余的方式。
2. 故障恢复：当主节点故障时，从节点可以暂时替代主节点提供服务，是一种服务冗余的方式
3. 负载均衡：在主从复制的基础上，配合读写分离，由主节点进行写操作，从节点进行读操作，分担服务器的负载；尤其是在多读少写的场景下，通过多个从节点分担负载，提高并发量。
4. 高可用基石：主从复制还是哨兵和集群能够实施的基础。

## 13.3 集群

**一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机），原因如下：**

1. 从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大；
2. 从容量上，单个Redis服务器内存容量有限，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，单台Redis最大使用内存不应该超过20G。电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是"多读少写"。对于这种场景，我们可以使如下这种架构：
3. 单台服务器故障率高，系统崩坏概率大

**电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是"多读少写"。对于这种场景，我们可以使如下这种架构：**

![image-20210730155434077](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730155434077.png)

​    主从复制，读写分离！ 80% 的情况下都是在进行读操作！减缓服务器的压力！架构中经常使用！ 一主二从！只要在公司中，主从复制就是必须要使用的，因为在真实的项目中不可能单机使用Redis！

## 13.4 环境配置

只需要配置从库，不需要配置主库。

~~~bas
127.0.0.1:6379> info replication  #查看当前库的信息
# Replication
role:master  # 角色 master
connected_slaves:0 # 没有从机
master_replid:b63c90e6c501143759cb0e7f450bd1eb0c70882a
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
~~~

复制3个配置文件，然后修改对应的信息
1、端口
2、pid 名字
3、log文件名字
4、dump.rdb 名字

修改完毕之后，启动我们的3个redis服务器，可以通过进程信息查看！

![image-20210730155858038](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730155858038.png)

## 13.5 一主二从

默认情况下，每台Redis服务器都是主节点； 我们一般情况下只用配置从机就好了！认老大！ 一主 （79）二从（80，81）

~~~bash
127.0.0.1:6380> SLAVEOF 127.0.0.1 6379  # SLAVEOF host 6379  找谁当自己的老大！
OK
127.0.0.1:6380> info replication
# Replication
role:slave  # 当前角色是从机
master_host:127.0.0.1  # 可以的看到主机的信息
master_port:6379

# 在主机中查看！
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:1  # 多了从机的配置
slave0:ip=127.0.0.1,port=6380,state=online,offset=42,lag=1   # 多了从机的配置
~~~

如果两个都配置完了，就是有两个从机的

![image-20210730160114805](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730160114805.png)

我们这里是使用命令搭建，是暂时的，==真实开发中应该在从机的配置文件中进行配置，==这样的话是永久的。

![img](https://img-blog.csdnimg.cn/20200513215654634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

## 13.6 使用规则

1. 主机既可以写也可以读，从机只能读

   ~~~bash
    127.0.0.1:6381> set name llq # 从机6381写入失败
   (error) READONLY You can't write against a read only replica.
   
   127.0.0.1:6380> set name llq # 从机6380写入失败
   (error) READONLY You can't write against a read only replica.
   
   127.0.0.1:6379> set name llq
   OK
   127.0.0.1:6379> get name
   "llq"
   ~~~

2. 当主机断电宕机后，默认情况下从机的角色不会发生变化 ，集群中只是失去了写操作，当主机恢复以后，又会连接上从机恢复原状。

3. 当从机断电宕机后，若不是使用配置文件配置的从机，再次启动后作为主机是无法获取之前主机的数据的，若此时重新配置成为从机，又可以获取到主机的所有数据。这里就要提到一个同步原理。
   **复制原理：**

​        Slave 启动成功连接到 master 后会发送一个sync同步命令Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执完毕之后，master将传送整个数据文件到slave，并完成一次完全同步。
​       全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
​       增量复制：Master 继续将新的所有收集到的修改命令依次传给slave，完成同步
​       但是只要是重新连接master，一次完全同步（全量复制）将被自动执行！ 我们的数据一定可以在从机中看到！

4. 第二条中提到，默认情况下，主机故障后，不会出现新的主机，有两种方式可以产生新的主机：

   1. 从机手动执行命令`slaveof no one`,这样执行以后从机会独立出来成为一个主机

      如果主机断开了连接，我们可以使用`SLAVEOF no one`让自己变成主机！其他的节点就可以手动连接到最新的主节点（手动）！如果这个时候老大修复了，那么就重新连接！

   2. 使用哨兵模式（自动选举）

# 14、 哨兵模式

（自动选举老大的模式）

  更多详细的信息参考博客：https://www.jianshu.com/p/06ab9daf921d

## 14.1 概述

   主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis从2.8开始正式提供了Sentinel（哨兵） 架构来解决这个问题。

​    哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。==**其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**==

![image-20210730162434672](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730162434672.png)

**这里的哨兵有两个作用：**
    通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。当哨兵监测到master宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他的从服务器，修改配置文件，让它们切换主机。
   然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了多哨兵模式。

![image-20210730162755355](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730162755355.png)

​    假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为==**主观下线**==。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一次投票，投票的结果由一个哨兵发起，进行failover[故障转移]操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为==**客观下线**==。

## 14.2 测试

我们目前的状态是 一主二从！

步骤一：配置哨兵配置文件sentinel.cof

我们可以从/www/server/redis 下复制一个sentinel.conf文件。也可以自定义一个。

~~~bash
sentinel monitor mymaster 127.0.0.1 6379 1 #这是核心配置
#后面的数字1表示：当一个哨兵主观的认为主机下线，就可以客观的的认为主机故障，然后开始选举新的主机
~~~

步骤二：启动哨兵模式

~~~bash
redis-sentinel lconfig/sentinel.conf  #启动哨兵模式
~~~

![image-20210730163643150](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730163643150.png)

**此时哨兵监视着我们的主机6379，当我们断开主机后：**

![img](https://img-blog.csdnimg.cn/20200513215806972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

## 14.3 优缺点

**优点：**

1. 哨兵集群，基于主从复制模式，所有主从复制的优点，它都有
2. 主从可以切换，故障可以转移，系统的可用性更好
3. 哨兵模式是主从模式的升级，手动到自动，更加健壮

**缺点：**

1. Redis不好在线扩容，集群容量一旦达到上限，在线扩容就十分麻烦
2. 实现哨兵模式的配置其实是很麻烦的，里面有很多配置项

## 14.4 哨兵模式的所有配置

完整的配置文件sentinel.conf

~~~bash
# Example sentinel.conf
 
# 哨兵sentinel实例运行的端口 默认26379
port 26379
 
# 哨兵sentinel的工作目录
dir /tmp
 
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 1
 
# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd
 
 
# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
 
# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
这个数字越小，完成failover所需的时间就越长，
但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
 
 
 
# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。  
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
 
# SCRIPTS EXECUTION
 
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
 
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
#这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，
#一个是事件的类型，
#一个是事件的描述。
#如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
  sentinel notification-script mymaster /var/redis/notify.sh
 
# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh

~~~

# 15、缓存穿透、击穿以及雪崩

## 15.1 缓存穿透（查不到）

**概念：**

​       在默认情况下，用户请求数据时，会先在缓存(Redis)中查找，若没找到即缓存未命中，再在数据库中进行查找，数量少可能问题不大，可是一旦大量的请求数据（例如秒杀场景）缓存都没有命中的话，就会全部转移到数据库上，造成数据库极大的压力，就有可能导致数据库崩溃。网络安全中也有人恶意使用这种手段进行攻击被称为洪水攻击。


**解决方案：**

方案一：布隆过滤器

  对所有可能查询的参数以Hash的形式存储，以便快速确定是否存在这个值，在控制层先进行拦截校验，校验不通过直接打回，减轻了存储系统的压力。

![image-20210730164944752](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730164944752.png)

方案二：缓存空对象

一次请求若在缓存和数据库中都没找到，就在缓存中方一个空对象用于处理后续这个请求。

![image-20210730165038384](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730165038384.png)

​     **这样做有一个缺陷：存储空对象也需要空间，大量的空对象会耗费一定的空间，存储效率并不高。解决这个缺陷的方式就是设置较短过期时间，即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。**

## 15.2 缓存击穿（量太大，缓存过期）

**概念：**

​     相较于缓存穿透，缓存击穿的目的性更强，一个存在的key，在缓存过期的一刻，同时有大量的请求，这些请求都会击穿到DB，造成瞬时DB请求量大、压力骤增。这就是缓存被击穿，只是针对其中某个key的缓存不可用而导致击穿，但是其他的key依然可以使用缓存响应。

​     比如热搜排行上，一个热点新闻被同时大量访问就可能导致缓存击穿。



**解决方案：**

方案一：设置热点数据永不过期

​     这样就不会出现热点数据过期的情况，但是当Redis内存空间满的时候也会清理部分数据，而且此种方案会占用空间，一旦热点数据多了起来，就会占用部分空间。

方案二：加互斥锁（分布式锁）

​    在访问key之前，采用SETNX（set if not exists）来设置另一个短期key来锁住当前key的访问，访问结束再删除该短期key。保证同时刻只有一个线程访问。这样对锁的要求就十分高。

## 15.3 缓存雪崩

**概念：**

  大量的key设置了同一过期时间，导致再缓存的同一时刻全部失效，造成瞬间数据库的请求量大，压力骤增引起雪崩。

 ![image-20210730170211388](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210730170211388.png)

**解决方案：**

方案一：redis高可用

​    这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群

方案二：限流降级

​    这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

方案三：数据预热

​    数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。
