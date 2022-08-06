### 什么是QUIC协议？它有哪些优点？

#### 什么是QUIC协议？

QUIC协议是Google提出的基于UDP的传输协议，具有高效的传输效率和多路并发的能力，称为HTTP3.0的底层协议。

#### 优点

- 握手建立速度快：UDP减少了三次握手的延迟，加密协议采用TLS1.3版本，不需要TLS握手完成才能进行通信。
- 避免队头阻塞的多路复用：支持多路复用，QUIC的流和流之间完全隔离，没有时序依赖，不会出现队首阻塞。
- 支持连接迁移：移动数据和Wifi之间进行切换的时候，原有协议需要重新建立TCP连接，但是QUIC支持ConnectionID标识连接，即使出现IP端口变化，只要ConnectionID一致就可以保持连接。
- 可以拔插的拥塞控制。
- 向前纠错。

ref:https://segmentfault.com/a/1190000041234654