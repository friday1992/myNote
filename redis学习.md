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

> geospatial  地理位置定位

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

