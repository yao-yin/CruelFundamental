# 负载均衡原理 #
负载均衡策略主要分以下四种：  
    
    1.HTTP重定向：浏览器（客户端）发来请求时，网页服务器（服务端）修改http请求的location标记来实现把请求分发给其他服务器，做到均衡。优点在于实现简单，缺点在于二次响应性能差且不能感知其他服务器的性能。
    2.DNS：把客户端发来的url通过域名解析转发到目标服务器。优点在于预先写好的转发规则可以做到瞬间转发，性能极佳。缺点在于策略单一、策略不可定制。
    3.反向代理：在服务器集群的入口设置一台反向代理调度器，决定每一条http请求应该发给集群里面哪一个服务器。优点在于可以定制策略、部署简单。缺点在于对调度器并发要求很高，有可能成为系统瓶颈。
    4.IP负载均衡策略：和前面不同，工作在网络层，把发来请求的ip转为实际服务器的ip，并不修改http内容。优点在于性能高、稳定。缺点在于功能较为单一，难以配置。