# redis学习 #

## 一、视频快速学习 ##
> 来自慕课网剑指java面试

### redis特点？ ###
	
1. 数据类型丰富，支持Sting、Hash、List、Set、Sorted Set等数据类型
2. 支持数据磁盘持久化
3. 支持主从
4. 支持分片

### redis为什么那么快？ ###
> 100000+QPS（QPS：query per second每秒查询次数）

1. 基于内存操作
2. 数据结构简单
3. 单线程
4. I/O多路复用模型，非阻塞IO

### IO多路复用模型 ###

**FD**（file descriptor，文件描述符）：一个打开的文件通过唯一的描述符进行引用，该描述符是打开文件元数据到文件本身的映射。

![](https://i.imgur.com/7MQvw8u.png)

![](https://i.imgur.com/OdMciqf.png)

### redis采用的I/O多路复用函数 ###

1. epoll
2. kqueue
3. evport
4. select（时间复杂度O(n)，其他的为O(1)）

### Redis简单使用 ###

1. String：最基本的数据类型，二进制安全

		>set name "zhangsan"
		OK
		>get name
		"zhangsan"

		>set count 1
		OK
		>incr count
		(integer)2
		>get count
		"2"

2. Hash:string元素组成的字典，适用于存储对象

		>hmset people name "zhangsan" age 16
		>hget people name
		"zhangsan"

3. List：按照String插入顺序排序

		>lpush list aaa
		(integer)1
		>lpush list bbb
		(integer)2
		>lrange list 0 5
		1)"aaa"
		2)"bbb"

4. Set：Sring元素组成的无序集合，通过哈希表实现，不允许重复

		>sadd set aaa
		(integer)1
		>sadd set bbb
		(integer)1
		>sadd set aaa
		(integer)0

		>smembers set
		1)"bbb"
		2)"aaa"

5. Sorted Set:通过分数来为集合中的元素进行从小到大的排序（分数可以重复）

		>zadd zset 1 a
		(integer)1
		>zadd zset 1 b
		(integer)1
		>zadd zset 2 c
		(integer)1
		>zadd zset 3 b
		(integer)0

		>zrangebyscore zset 0 6
		1)"b"
		2)"a"
		3)"c"

6. HyperLogLog用于计数，Geo用于保存地理位置


### 如何从海量redis数据中筛选指定前缀的key？ ###
>最好先了解数据量到底有多大

1. keys pattern：查找所有指定pattern模式的key。

		>keys k1*
	
	这种模式是一次查询所有数据并返回，会导致redis被阻塞，不建议在线使用
2. scan cursor [match pattern] [count count]

		>scan 0 match k1* count 6
	1. 基于游标的迭代器，需要基于上一次返回的游标延续迭代过程。
	2. 以0作为游标开始一次新的迭代，直到返回完成一次遍历。
	3. 不保证每次都返回一定数量的元素，支持模糊查询。
	4. 前次返回的游标可能比当前小，所以客户端要用hashmap等去重。


### 如何利用redis实现分布式锁？ ###
>利用redis对key操作的原子性


**setnx key value**：如果key不存在，则创建并赋值。设置成功返回1，设置失败返回0。
	
		>setkey lock 1
		(integer)1

问题是setnx设置的可以长期有效

**expire key seconds**：设置key在seconds后过期

	>expire lock 6

*在编程当中这两条语句不是原子的，线程操作不安全。所以使用下面命令*

**set key value [ex seconds] [px milliseonds] [nx|xx]**
	
- ex seconds：设置键的过期时间seconds秒
- px millisecond：过期时间millisecond毫秒
- nx：只在键不存在，才对键操作
- xx：健存在，才操作
- 成功返回ok，否则返回nil

### 大量key同一时间过期？ ###
可以为key的过期时间加上随机数，避免redis同时删除大量key


### redis如何实现异步队列？ ###
1. 使用list作为队列，rpush生产消息，lpop消费消息。
	
	缺点：不能等待队列里有值再消费
	补救：使用sleep重试消费

2. **blpop key [key...] timeout**：阻塞直到队列有值或超时。

		>blpop list 6

	缺点：只能被一个消费者消费

3. sub/pub主题订阅模式

		>subscribe topic
		>public topic "hello"
	缺点：消息发布是无状态的，无法保证可达

### redis RDB持久化 ###
**RDB（快照）持久化**：保存某一时刻的全量数据快照
	
	1. save：阻塞redis服务器进程，直到RDB文件创建完毕
	2. bgsave：fork子线程来创建RDB，不阻塞服务器进程

	lastsave：查看最近一次RDB保存时间戳
		

**自动触发RDB持久化方式？**

- 根据redis.conf配置中的`save m n`定时触发（bgsave）
- 主从复制，主节点自动触发
- 执行`debug reload`
- 执行shutdown且没有开启AOF持久化

![bgsave原理](https://i.imgur.com/yHkqb1D.png)

RDB缺点：

- 数据全量保存，影响io性能
- 定时保存快照数据，如果redis宕机会导致数据丢失
   
### Redis AOF持久化 ###
**AOF（Append Only File）持久化**：保存写状态

- 保存除查询以外的所有变更数据库状态的指令
- 以append的形式追加保存到AOF文件中（增量）

redis.conf文件
	
	appendonly no // 设置为yes
 	appendfsync everysec //always：一有修改就保存；everysec：每秒保存一次；no：根据操作系统决定

**日志重写解决AOF文件不断变大问题**

1. 调用fork（），创建子线程
2. 子进程把新的AOF写到临时文件，不依赖原来的AOF文件
3. 主进程持续将新的变动写到内存和原来的AOF文件
4. 主进程获取子进程重写AOF完成信号，向新的AOF同步增量变动
5. 使用新的AOF文件替换旧的AOF文件

### RDB和AOF ###
**redis数据恢复？**
![redis数据恢复图](https://i.imgur.com/uIdJ0Ab.png)

1. 存在AOF文件，就直接加载AOF恢复数据。否则执行步骤2
2. 存在RDB文件，加载RDB恢复数据。否则直接启动

**优缺点**

RDB优点：全量数据快照，文件小，恢复快

RDB缺点：无法保存最近一次快照之后的数据

AOF优点：可读性高，适合保存增量数据，数据不易丢失

AOF缺点：文件体积大，恢复时间长

**RDB-AOF混合持久化方式**：RDB做镜像全量持久化，AOF做增量持久化


### 使用pipleline ###
- 批量执行指令，节省指令传输时间
- pipleline执行没有顺序，有顺序依赖的指令分批发送

### redis同步机制 ###
**全同步**

1. slave发送sync命令到master
2. master启动一个后台进程，将redis快照信息保存到文件中
3. master在保存数据快照期间将收到的命令缓存起来
4. master完成文件操作后，将文件发给slave
5. slave使用新的AOF文件替换旧的aof文件
6. master将这期间收到的增量命令发送给slave

**增量同步**
