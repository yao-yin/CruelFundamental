进程同步方式

* 信号量

用于进程间传递信号的一个整数值。在信号量上只有三种操作可以进行：初始化，P操作和V操作，这三种操作都是原子操作。

P操作(递减操作)可以用于阻塞一个进程，V操作(增加操作)可以用于解除阻塞一个进程。

基本原理是两个或多个进程可以通过简单的信号进行合作，一个进程可以被迫在某一位置停止，直到它接收到一个特定的信号。该信号即为信号量s。

为通过信号量s传送信号，进程可执行原语semSignal(s);为通过信号量s接收信号，进程可执行原语semWait(s);如果相应的信号仍然没有发送，则进程会被阻塞，直到发送完为止。

可把信号量视为一个具有整数值的变量，在它之上定义三个操作：

一个信号量可以初始化为非负数 semWait操作使信号量s减1.若值为负数，则执行semWait的进程被阻塞。否则进程继续执行。 semSignal操作使信号量加1，若值大于或等于零，则被semWait操作阻塞的进程被解除阻塞。

* 管程

管程是由一个或多个过程、一个初始化序列和局部数据组成的软件模块，其主要特点如下：

局部数据变量只能被管程的过程访问，任何外部过程都不能访问。 一个进程通过调用管程的一个过程进入管程。 在任何时候，只能有一个进程在管程中执行，调用管程的任何其他进程都被阻塞，以等待管程可用。
管程通过使用条件变量提供对同步的支持，这些条件变量包含在管程中，并且只有在管程中才能被访问。有两个函数可以操作条件变量：

cwait(c)：调用进程的执行在条件c上阻塞，管程现在可被另一个进程使用。 csignal(c)：恢复执行在cwait之后因为某些条件而阻塞的进程。如果有多个这样的进程，选择其中一个；如果没有这样的进程，什么以不做。

* 消息传递

消息传递的实际功能以一对原语的形式提供：

send(destination,message)
receive(source,message)
这是进程间进程消息传递所需要的最小操作集。

一个进程以消息的形式给另一个指定的目标进程发送消息；

进程通过执行receive原语接收消息，receive原语中指明发送消息的源进程和消息
***
线程同步方式

* 互斥量
* 读写锁
* 条件变量
* 信号量

***
differences between process synchronization and thread synchronization

* 线程间资源是共享的，讲安全：信号量，锁，原子操作。
* 进程间资源是独立的，讲通讯：管道，共享内存。

***
states of a process

* 创建、
* 就绪
* 执行
* 阻塞
* 终止