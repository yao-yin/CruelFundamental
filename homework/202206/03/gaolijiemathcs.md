### 什么是缓冲区溢出？有什么危害？

缓冲区溢出：计算机向缓冲区中填充数据的时候，超出了缓冲区本身的容量，而用户没有检查用户输入，导致溢出的数据部分会覆盖合法数据。

危害：

- 分段错误，程序崩溃
- 跳转执行恶意外码