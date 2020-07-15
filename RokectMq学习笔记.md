# RokectMq学习笔记

#### 1.市面上常见的消息中间件

| ActiveMq | 1.适用于小公司，无法使用高并发，性能不好，集群架构模式还行（master-slave，netWork模式） |
| -------- | :----------------------------------------------------------- |
| Kafaka   | 面向大数据方向，不支持事务，高性能，对消息的错误，重复，消失没有高要求，追求高吞吐量 |
| RokectMq | 高吞吐量，高可用性，使用大规模分布式系统，name-server集群代替zookeeper |
| RabbitMq | 基于AMQPQP协议，面向消息，队列，路由，可靠，安全，不追求高性能和吞吐量 |

#### 2.AMQP是什么

AMQP（Advanced Message Queuing Protocol，高级消息队列协议）是一个进程间传递**异步消息**的**网络协议**

![amqp](H:\firefoxdoawnload\amqp.png)

server:broker，接受客户端的连接，实现AMQP实体服务

connection ：应用程序与broker的网络连接

channel：数据读写在channel种进行，客户端建立多个channel,每个channel是一个会话

message:服务器和应用程序之间传送数据，有properties和body组成，properties装饰消息，比如消息优先级，延迟等，body就是消息本体

virtual host ：虚拟主机。用于逻辑隔离，最上层的消息路由。

exchange:交换机,接受消息，根据路由键转发消息到绑定队列

bind：绑定，exchange和queue的连接

routing key ：一个路由规则，虚拟机由它来确定如何路由一个消息

queue:储存消息的队列