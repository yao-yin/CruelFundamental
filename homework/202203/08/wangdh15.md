## 请你说一下MySQL引擎和区别

MySQL的引擎有三种：

1. InnoDB存储引擎
2. MyISAM存储引擎
3. MEMORY存储引擎

### InnoDB存储引擎

支持ACID事务特性。所以对事务要求比较高的场景，可以采用该引擎。同时如果更新和删除操作比较频繁的话，也可以选择InnoDB。

### MyISAM存储引擎

插入数据较快，空间和内存使用比InnoDB相比较低，没有完整的事务支持，如果主要用于插入记录和读取记录，可以考虑使用。

### MEMORY存储引擎

所有的数据都在内存中，自然不满足事务的D的特性。数据处理速度快，数据安全性较低。