# 一个 100G 的文件，内存只有 8G，如何给文件排序

将大文件拆分成小于8G的小文件组，分别快排， 然后多路归并存到磁盘中即可