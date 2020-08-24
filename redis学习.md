# redis学习

> 1.nosql  not only sql

```
1.关系型数据库
	分库分表+水平拆分+集群

2.非关系型数据库
	数据之间没有关系，方便扩展
	大数据高性能，1秒写8万次，读11万次
	数据是多样性的，8种类型
	大数据时代的3v+3高

```

> 2.NoSql 四大分类

- KV键值对
  	redis+Tair+memcache

- 文档型数据库

  ​    mongodb 基于分布式文件存储的数据库，C++编写，介于关系型数据库与非关系型数据库中间的产品，是非关系型数据库种功能最丰富的，最像关系型数据库的

-  列储存

  Hbase

  分布式文件系统

- 存储

  ​	社交网络 neo4jKV键值对

> 3.redies 概述

1.  remote dictionary server 远程字典服务

   c语言编写，支持网络，可基于内存也可持久化的，key-value数据库

   > 能干嘛

   内存存储，

   持久化，

   高效率高速缓存，

   发布订阅系统，

   计数器浏览量

> redis linux 安装

```
1.下载安装包，并解压 tar –xvf
2. 安装gcc yum install gcc-c++
3，make  make install 开始安装
4.cp redis.conf /config  拷贝配置文件
5.修改 daemonize 为yes
6.启动  redis-server /config/redis.conf
7.redis-cli -h localhost   redis-cli -p 6379
8.shutdown 
9.exit
10.ps -ef|grep redis
```

> redis-benchmark  压力测试

redis-benchmark -h localhost -c 100 -n 100000

> 基础知识

database 默认有16个 

select 3 进行切换 默认是 0

flushdb 清空数据库  flushall 清空所有数据库

> redis 数据类型

![image-20200716111919614](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200716111919614.png)

```
1.redis-key
EXPIRE mykey 10 设置过期时间
EXISTS mykey 1存在 0 不存在
MOVE key db 移动key到指定数据库
keys * 查看所有key
TTL mykey # 查看mykey剩余的过期时间
type mykey 查看数据类型
```

```
2.String 类型 大部分使用场景
append mykey value 拼接字符串
strlen mykey  获取长度
incr mykey +1
decr mykey -1
incrby mykey 10 步增
decrby mykey 10 步减
getrange mykey start end 截取
setrange mykey offset value 替换
setex key4 30 "1234" 设置过期时间
setnx key1 323 如果不存在则设置值，存在则不操作s
mset k1 v1 k2 v2 批量设置值
mget k1 k2	批量获取值
msetnx 如果不存在则设置值，存在则不操作 如果有1个失败，则全部失败
set user:1:age 2
set user:1:name 张三
getset 先获取再赋值

```

> List 类型

```
可以当成栈或者阻塞队列
lpush list 1 添加数据
lrange list	0 1遍历数据
llen list 
lpop list 
lrem list count value 移除特定数量的value
ltrim list start end 截取 改变 list 
rpoplpush list list1 移除一个列表最后一个，放入新的列表中
lset list index newvalue
 linsert list after/before 2 agfg



```

> set set集合 无序不重复集合

```
sadd
smembers 
sismember set 5
scard set 计算长度
srandmember set count
spop set  
smove set set2 2   移动到另外一个
sdiff set set2 第一个单独有的
sinter set set2  交集
sunion set set2  并集
```

> hash

```
 hgetall set 
 hset set fildes value
 hget set fildes
 hlen set  获取长度
 hexists set filde2  判断是否存在
hkeys set 获取所有key
hvals set 获取所有值
hincrby set filde1 1
hsetnx set filde1 21312  存在不修改，不存在则添加
应用：用户信息保存，string 更适合字符串


```

> zset 有序集合

```
zset set index value  再index加入元素
 zrange zset 0 3  遍历元素
zrangebyscore zset -inf +inf  更具index 排序
zrangebyscore zset -inf +inf withscores  从低到高
zrevrangebyscore zset +inf -inf  从高到低
zcount zset 0 3000 获取区间指定数量
 应用：有序的可以存储工资表成绩表 ，带权重的消息
```

> geospatial  地理位置定位  可以做附近的人

```
GEOADD
GEODIST  返回给定2个位置的距离
GEOHASH   返回11个字符串的hash长度
GEOPOS  geopos Sicily Catania  获取经度和纬度
GEORADIUS  已给定的经度，纬度为中心，找出半径内的位置
GEORADIUSBYMEMBER  找出给定滴定200KM范围内的元素  GEORADIUSBYMEMBER   Sicily  Catania 200km

GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
redis> GEODIST Sicily Palermo Catania  km
"166274.1516"
redis> GEORADIUS Sicily 15 37 100 km
1) "Catania"
redis> GEORADIUS Sicily 15 37 200 km
1) "Palermo"
2) "Catania"

底层是zset ，可以用zrange zrem
```

> hyperloglog 

```
1.什么是基数
不重复的元素=5，可以接收误差
2.应用
网站uv ,同一个人多次访问算一次
传统方式是set,不重复，但是如果存大量id,会占用大量内存
3.hyperloglog优点
占用内存是固定的，2^64的数据，只占用12kb
4.使用
pfadd mylog  1 1 3 413 2 2
pfcount mylog  会去掉重复的元素
pfmerge mylog3 mylog1 mylog2  合并2个集合，去重

```

> bitmaps   位存储

```
1.应用场景
统计人数，活跃用户，打卡
只要有2个状态的可以使用bitmaps
2.使用
setbit mybit 0 1  记录一个星期的打卡
getbit mybit 0
bitcount mybit  统计

```

## Redis 事务

```
1.开启事务
multi
2.执行操作 进入队列
3.提交执行
exec 
4.取消操作
discard

```

```
编译型异常 在第二步操作中出现了命令报错，如输入参数错误，则最后exec执行失败
运行时异常  对一个字符串进行加1 ，这时候其他命令会得到执行，报错的这条不会执行
```

> 悲观锁和乐观锁

```
1.悲观锁 无论什么时候都会加解锁
2.乐观锁 更新的时候去判断是否有人更改过数据
watch money 监视乐观锁的操作，如果有其他线程修改了money值，提交事务的时候会失败
unwatch exec discard都会自动解锁
```

## Jedis

```
1.连接数据库
 Jedis jedis = new Jedis("127.0.0.1",6379);
2.  //开启事务
        Transaction transaction = jedis.multi();
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello", "world");
        jsonObject.put("i", "love");
        String result = jsonObject.toJSONString();
        Response<String> name = transaction.get("username");
        try {
            transaction.watch("username");
            System.out.println(name.toString());
            transaction.set("json", result);
            transaction.set("json2", result);
            transaction.set("username", "2345");
            Thread.sleep(10000);
//          int i = 1 /0;
            transaction.exec();
        } catch (Exception e) {
            transaction.discard();
            e.printStackTrace();
        } finally {
            System.out.println(jedis.get("json"));
            System.out.println(jedis.get("json2"));

            transaction.close();
        }
```

> springboot 集合redis

```
1.底层没有使用jedis ，因为jedis是直连redis,在多线程下不安全
2.lettuce底层是netty,实例可以在多线程下共享
3.配置redis
spring.redis.url=localhost
spring.redis.port=6379
4.使用RedisTemplate进行操作
5.源码看RedisAutoConfiguration    RedisProperties
```

> Redis.config

![image-20200723150339978](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200723150339978.png)

大小写不敏感

![image-20200723150432906](C:\Users\pc\AppData\Roaming\Typora\typora-user-images\image-20200723150432906.png)

```
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice  日志级别
 
# Specify the log file name. Also 'stdout' can be used to force
# Redis to log on the standard output. 
logfile ""   日志文件地址
```

> 持久化配置

```
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 900 1  900秒内修改一次，持久化，以下类推
save 300 10
save 60 10000

# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes  //持久化出错是否继续工作
# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes  //是否压缩rdb文件
# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes              //保存rdb的时候是否检查错误

# The filename where to dump the DB
dbfilename dump.rdb

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
# 
# The Append Only File will also be created inside this directory.
# 
# Note that you must specify a directory here, not a file name.
dir ./  rdb文件路径

################################## SECURITY ###################################

# Require clients to issue AUTH <PASSWORD> before processing any other
# commands.  This might be useful in environments in which you do not trust
# others with access to the host running redis-server.
#
# This should stay commented out for backward compatibility and because most
# people do not need auth (e.g. they run their own servers).
# 
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared  设置密码
命令行设置  config set requirepass 123456   
config get requirepass 

# maxclients 10000 //最大客户端数量
 maxmemory <bytes>  //最大内存
 # maxmemory-policy noeviction 内存满后，执行哪种策略
 # volatile-lru -> remove the key with an expire set using an LRU algorithm
# allkeys-lru -> remove any key according to the LRU algorithm
# volatile-random -> remove a random key with an expire set
# allkeys-random -> remove a random key, any key
# volatile-ttl -> remove the key with the nearest expire time (minor TTL)
# noeviction -> don't expire at all, just return an error on write operations

 
appendonly no AOF模式默认关闭，RDB默认
appendfilename "appendonly.aof"
# appendfsync always
appendfsync everysec
# appendfsync no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb  //超过64m就重写


AOF是已日志的形式记录全部写记录，重启的时候恢复所有操作记录
AOF 修复
如果aof文件被破坏
可以使用  redis-check-aof 修复
优点和缺点
优点：
同步效果好，可以设置每秒同步，每次同步
缺点:
aof比rdb大
io操作多，比rdb慢
```

> 发布订阅者模式

```
1.订阅频道
subsribe luonan
2.发送消息
publish luoann "hello"
redis是C语言写的，可以再publish.c查看源码。订阅一个频道，redis就会维护一个字典，而字典的值就是一个链表
```

> 主从复制

```
1，master负责写，slave负责读
2.作用：
 	1.数据冗余，实现了数据的热备份
 	2.故障修复，
 	3，负载均衡
 	4.高可用
3.配饰集群
	info replication  查看是否是master
	# Replication
    role:master
    connected_slaves:0
    master_replid:f37c0ae1cae28c4edfc0d33745ddd31b63ef8d4c
    master_replid2:0000000000000000000000000000000000000000
    master_repl_offset:0
    second_repl_offset:-1
    repl_backlog_active:0
    repl_backlog_size:1048576
    repl_backlog_first_byte_offset:0
    repl_backlog_histlen:0
一主二从
slaveof IP port
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:192.168.31.128
master_port:6379
master_link_status:down
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:0
master_link_down_since_seconds:1595734481
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:a94d3b953152721b323e48c31254c2132df72cc3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
也可以再配置文件中配置
replicaof ip port

slaveof no one 使自己变成主机
```

```
哨兵模式
```

```
监控后台主机是否故障，通过投票，重新选取主机
sentinel.conf 
sentinel monitor mymaster 127.0.0.1 6379 2
启动哨兵
redis-sentinel conf

```

> 内存穿透和雪崩

```
1。加上布隆过滤器
2.缓存空对象
3.缓存击穿
```

