## 2022/05/17

### C++: STL中deque底层原理是什么？

deque由一段一段连续的**等长**的空间结构构成，维护一个数组来管理各个等长区间的空间地址，如果前面的空间用完了，就会新申请一个等长区间，后面同理；如果数组用完了，就会申请一更大的数组，把数据拷贝过去，然后释放掉原来的数组空间