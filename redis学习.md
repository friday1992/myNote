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

select 3 默认是 0

flushdb 清空数据库  flushall 清空所有数据库