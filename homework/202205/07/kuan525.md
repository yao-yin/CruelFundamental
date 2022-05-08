#### 消息中间件如何做到高可用？

RabbitMQ是基于主从复制来实现高可用的，不支持分布式，既然是主从复制，那么肯定就不是单台机器能保证的，所以是通过集群来保证高可用，而RabbitMQ的集群模式有两种，普通集群和镜像集群，真正实现高可用的是镜像集群模式。



Kafka的一个基本架构：多个broker组成，一个broker是一个节点；一个topic可以划分成多个partition，每个partition可以存在于不同的broker上面，每个partition存放一部分数据。这是天然的分布式消息队列。

Kafka在0.8以前是没有HA机制的，也就是说任何一个broker宕机了，那个broker上的partition就丢了，没法读也没法写，没有什么高可用可言。

Kafka在0.8之后，提过了HA机制，也就是replica副本机制。每个partition的数据都会同步到其他机器上，形成自己的replica副本。然后所有的replica副本会选举一个leader出来，那么生产者消费者都和这个leader打交道，其他的replica就是follower。写的时候，leader会把数据同步到所有follower上面去，读的时候直接从leader上面读取即可。