## 2022/05/03

### 消息队列如何保证消息的顺序性？

#### RabbitMq

再MQ里面创建多queue，同一规则的数据，有顺序的放入 MQ 的 queue 里面，消费者只取一个 queue 里面获取数据消费，这样执行的顺序是有序的。或者还是只有一个 queue 但是对应一个消费者，然后这个消费者内部用内存队列做排队，然后分发给底层不同的 worker 来处理。