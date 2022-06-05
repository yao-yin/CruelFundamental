# 什么是缓冲区溢出？有什么危害？

函数调用时, 会在栈上开辟缓冲区, 如果用户输入的字符串超出了缓冲区导致缓冲区溢出, 会将函数的返回地址修改成用户的输入部分. 这会导致缓冲区溢出. 有可能执行恶意代码片段. 

防御的方法一方面是随机化栈帧起始位置, 还有一种是"金丝雀", 即查看缓冲区之外的"水印内容"是否被破坏. 还有就是代码段设置不可执行的权限, 防止执行恶意代码片段.