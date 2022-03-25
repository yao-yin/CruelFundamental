#### C++: 指针和引用的差别

总结起来就是：

1. 指针是一个实体，而引用是一个别名；在汇编上，引用的底层是以指针的方式实现的，定义一个引用变量，相当于就是定义了一个指针，然后把引用内存的地址写到这个指针里面，当通过引用变量修改它所引用的内存时，它先访问了指针里面的地址，然后在这个地址的内存里面对值进行修改

2. 指针可以不初始化，通过赋值可以指向任意同类型的内存；但是引用必须初始化，而且引用一经引用一块内存，再也不能引用其它内存了，即引用不能被改变

3. 在进行 sizeof 操作时， sizeof 指针在 32 位系统下永远是 4 个字节，而 sizeof 引用计算的 是它所引用内存的大小

4. 引用是内存单元的别名，不是数值的别名。

5. 引用只能使用引用变量所引用的数据，例如b是a的别名，b只能使用a的数据