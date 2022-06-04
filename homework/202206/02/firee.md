### C++: 内联函数的作用和缺点

内联函数有一点像时间换空间，通过膨胀代码的代价减少调用的开销。但是代码膨胀可能会增加操作系统 cache 机制未命中次数，比如指令 cache 或者页表。