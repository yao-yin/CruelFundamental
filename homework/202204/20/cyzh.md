## 2022/04/20

### TCP是怎么保证有序传输的

对于TCP传输的包，都有基础编号，在N大小的滑动窗口里，每个包的编号是唯一的

### 知不知道 time_wait状态，这个状态出现在什么地方，有什么用

Time_wait出现在TCP传输的关闭阶段

#### 防止当前丢失的包影响下一次连接的包

#### 防止陷入半关闭的状态
