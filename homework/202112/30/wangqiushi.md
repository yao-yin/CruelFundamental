## 简述TCP三次握手过程，为什么TCP握手需要3次，两次行不行？
三次握手：客户端发送SYN包，服务端同意连接，回复SYN+ACK包，之后客户端回复ACK包，连接建立。为什么是3次？防止因为已失效的请求报文，突然传到服务器引起错误，造成连接状态不一致，即服务器收到请求后发送确认报文段，进入连接状态，但客户端却忽略了这次确认，不进入连接状态，服务器便会一直等待客户端发送的数据，浪费资源。3次握手本质上解决网络信道不可靠的问题，在不可靠的信道上建立起可靠的连接。经过3次握手后，客户端服务端都进入了数据传输状态。 需要确认双方的接收和发送能力，不然容易出现丢包的现象。