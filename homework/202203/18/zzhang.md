中间件middleware：

- 分布式环境下support应用开发和集成
- 中间件涵盖了从 Web 服务器，到身份验证系统，再到消息传递工具等一切内容

作用：

- 容器层：带有CI/CD的DevOps能力、容器管理功能以及服务网格功能。
- 运行时层：为高度分布式云环境（微服务）、内存中缓存（用于快速访问数据）和消息传递（用于快速数据传输）提供轻量级运行时和框架。
- 集成层：通过消息传递、集成和 API 来连接自定义与购买的应用及 SaaS 资产，从而形成功能正常的系统。此外，它还可以提供内存数据库和数据缓存服务、数据/事件流以及API管理功能
- 流程自动

常见中间件：

- 路由与Web服务器：处理和转发其他服务器通信数据的服务器
- RPC框架：微服务时代的远程服务调用框架
- 消息中间件：Message queue，例如Kafka
- 缓存服务：分布式的高速数据存储层，例如Redis

- 配置中心：用来统一管理各个项目中所有配置的系统
- 分布式事务：事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上
- 任务调度：分布式环境下提供定时、任务编排、分布式跑批等功能的系统
- 数据库层：用于支持弹性扩容和分库分表的 TDDL，数据库连接池 Driud, Binlog 同步的 Canal 等





Reference: [RedHat](https://www.redhat.com/zh/topics/middleware/what-is-middleware)，[zhihu](https://www.zhihu.com/question/19730582)
