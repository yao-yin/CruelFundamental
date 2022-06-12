页面置换算法有哪些

进程运行时，若其访问的页面不在内存而需将其调入，但内存已无空闲空间时，就需要从内存中调出一页程序或数据，送入磁盘的对换区。选择调出页面的算法就称为页面置换算法。

最佳置换算法(OPT): 所选择的被淘汰页面将是以后永不使用的，或者是在最长时间内不再被访问的页面,这样可以保证获得最低的缺页率。

先进先出(FIFO)页面置换算法:优先淘汰最早进入内存的页面，亦即在内存中驻留时间最久的页面。

最近最久未使用(LRU)置换算法:选择最近最长时间未访问过的页面予以淘汰，它认为过去一段时间内未访问过的页面，在最近的将来可能也不会被访问。

时钟(CLOCK)置换算法:又称为最近未用(Not Recently Used, NRU)算法。为每个页面设置一个访问位，再将内存中的页面都通过链接指针链接成一个循环队列。当某页被访问时，其访问位置为1，当需要淘汰一个页面时，只需检查页的访问位。如果是0，就选择该页换出。