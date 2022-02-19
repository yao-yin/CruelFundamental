# 如何用Redis实现一个消息队列?

主要有三种方法：

- List队列
- 发布/订阅模型 Pub/Sub
- Stream

### List队列

List底层为链表，头部尾部操作时间复杂度为O(1)。List作为队列。

#### LPUSH和RPOP

生产者使用LPUSH发布消息

```redis
127.0.0.1:6379> LPUSH queue msg1
(Integer) 1
127.0.0.1:6379> LPUSH queue msg2
(Integer) 2
```

消费者使用RPOP拉取消息

```
127.0.0.1:6379> RPOP queue
"msg1"
127.0.0.1:6379> RPOP queue
"msg2"
```

但此处有问题，当队列中没有消息，执行RPOP时会返回NULL。

```
127.0.0.1:6379> RPOP queue
(nil)
```

但是编写消费者模型，消费者会不断从队列中拉取消息进行处理，如果此时队列为空，那消费者会频繁拉取消息，会导致浪费CPU资源，Redis会造成压力。

可以修改消费者为当队列为空，休眠一会再去拉取新消息。但是又存在问题，有新消息来，消费者可能还在休眠，消费者处理新消息会存在延迟。

#### 阻塞式拉取消息：BRPOP/BLPOP

因此Redis还存在【阻塞式拉取消息：BRPOP/BRPOP】

BRPOP阻塞方式拉取消息，支持传入一个超时时间，如果设置为0，表示不超时，直到有新的消息才返回，否则会在指定的超时时间后返回NULL.（如果超时时间过长，连接太久没活跃，可能会被Redis Server 判定下线，所以客户端要有重连机制）

#### 缺点

解决了消息处理不及时，但是有缺点：

- 不支持重复消费：消费者拉取消息之后，从List中删除，无法被其他消费者再次消费，不支持多个消费者消费同一批数据。List只支持一组生产者对应一组消费者，无法满足多组生产者消费者的场景。
- 消息丢失：消费者拉取消息，如果发生异常，消息丢失。List中POP后，消息直接从链表删除，无论消费者是否成功处理，消息无法再次消费，消息相当于丢失了。



### 发布/订阅模型：Pub/Sub

Redis专门针对【发布 订阅】队列模型设计。

解决重复消费问题，多组生产者、消费者场景

两个消费者，同时消费同一个生产者的数据。

#### PUBLISH/SUBSCRIBE 发布、订阅

使用SUBSCRIBE命令，启动2个消费者，订阅同一个队列。

```
// 两个消费者订阅同一个队列
127.0.0.1:6379> SUSCRIBE queue
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "queue"
3) (integer) 1
```

两个消费者都被阻塞，等待新消息的到来。

之后，启动一个生产者，发布一条消息。

```
127.0.0.1:6379> PUBLISH queue msg1
(integer) 1
```

这个时候两个消费者都会接触阻塞，收到生产者发来的新消息。

```
127.0.0.1:6379> SUBSCRIBE queue
// 收到消息
1) "message"
2) "queue"
3) "msg1"
```

使用Pub/Sub方案，支持阻塞拉取消息，很好的满足了多组消费者，消费同一批数据的业务需求。



同时也可以订阅多个队列

```
127.0.0.1:6379> PSUBSCRIBE queue.*
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "queue.*"
3) (integer) 1
```

订阅了queue.*相关的队列。

生产者可以分别向queue.p1和queue.p2发布消息

```
127.0.0.1:6379> PUBLISH queue.p1 msg1
(integer) 1
127.0.0.1:6379> PUBLISH queue.p2 msg2
(integer) 1
```

此时看消费者可以接收到2个生产者的消息。

```
127.0.0.1:6379> PSUBSCRIBE queue.*
Reading messages... (press Ctrl-C to quit)
...
// 来自queue.p1 的消息
1) "pmessage"
2) "queue.*"
3) "queue.p1"
4) "msg1"

// 来自queue.p2 的消息
1) "pmessage"
2) "queue.*"
3) "queue.p2"
4) "msg2"
```



#### 优缺点

优点：支持多组生产者、消费者处理消息。

缺点：丢数据

如果出现：

- 消费者下线
- Redis宕机
- 消息堆积

原因：Redis Pub/Sub实现机制简单，没有基于任何数据类型实现，没有做任何的数据存储，只是为消费者、生产者建立数据转发通道，将符合规则的数据从一端转发到另外一端。

- 消费者订阅指定队列，Redis记录一个映射关系。
- 生产者向队列发送消息，Redis从映射关系找到对应的消费者，将消息转发。

（1）消费者异常下限会导致消息丢失问题

例如如果消费者异常挂掉，重新上线以后只能接受新的消息，下限期间生产者发布消息，因为找不到消费者转发，就会挂掉。

所以使用的时候必须，消费者先订阅队列，生产者才能发布消息，否则消息会丢失。

（2）Redis宕机数据丢失

Pub/Sub的操作不会写入RDB和AOF当中，Redis宕机重启，Pub/Sub数据会丢失。

（3）消息堆积，缓冲区溢出，消费者会强制下线，数据丢失

Redis发布消息，消息写入消费者缓冲区，消费者读取缓冲区消息。黄春去有上限，消费者消费速度慢，生产者发布的消息在缓冲区积压，当超过缓冲区上限，会将消费者下线。

（4）Pub/Sub消息处理异常，消息无法再次消费。

Pub/Sub缓冲区读取数据，数据从Redis缓冲区删除了，消费者异常，无法重新消费。



### Stream

#### XADD/XREAD生产、消费模型

XADD：发布消

XREAD：读取消息

#### 功能

- 支持阻塞式拉取消息：增加BLOCK 开启阻塞等待
- Stream支持发布/订阅模式
  - XGROUP：创建消费者组
  - XREADGROUP：在指定消费组，开启消费组拉取消息
- 消息处理异常，Stream保障消息不丢失。
  - 当一组消费者处理完消息，需要执行XACK告知Redis，处理完毕。
- 支持持久化
  - Stream为新的数据类型，支持RDB/AOF持久化。配置好策略，Redis宕机可恢复。
- 支持消息堆积：
  - 发布消息，指定队列最大长度，当队列长度超过上限，旧消息被删除，只保留固定长度新消息。



#### 相比专业消息中间件Redis优缺点

缺点：

1.Redis队列中间件本身可能丢消息

与专业的消息队列对比，生产者与消费者端的丢消息问题可以解决。但是队列中间件本身有可能会丢消息。

- AOF持久化每秒写盘，写盘为异步，Redis宕机可能存在数据丢失问题。
- 主从复制异步，主从切换也可能丢失消息。

2.消息积压问题

Redis Stream提供最大队列，但是仍然在内存中存储，存在Redis内存资源紧张。

但是Kafka与RabbitMQ，数据存在磁盘上，成本比内存低。



优点：

如果业务场景简单，丢数据不敏感，消息堆积少，可以用Redis。Redis运维部署轻量。



![image-20220212152345925](https://raw.githubusercontent.com/gaolijiemathcs/image-hosting/master/image-20220212152345925.png)



ref：https://www.zhihu.com/question/20795043