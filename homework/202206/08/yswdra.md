### mmap 原理和优势，shared memory原理和优势

------

mmap 是操作系统提供将文件映射到内存的机制，使得在逻辑上操作文件等同于操作内存
shared memory 则是将不同进程的虚拟地址映射到同一物理地址上，从而实现内存共享