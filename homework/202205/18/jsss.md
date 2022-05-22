# 什么是粘包，TCP粘包是怎么产生的？怎么解决拆包和粘包？

## 粘包

1. 发送方设置了`Nagle`, 并且包的长度比较短, 导致多个包站在一起后发送给对端. 
2. 接收方接收速率太慢, 发生粘包.

## 拆包

1. 发送端发送的包长度大于最大报文长度, 将进行拆包.
2. 发送端写入时缓冲区容量不足, 导致拆包.

## 解决方法

1. 添加包首部, 里面包含了包的长度信息.
2. 定长包长度, 不足的填0, 接收端每次读取固定长度的数据.
3. 特殊符号进行首尾的分割.