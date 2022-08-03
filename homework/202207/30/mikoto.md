## 并发和并行的区别是什么？为什么并发比串行慢？

并发 指从宏观上看，任务是同时进行的，但微观上来看只是任务的不断切换

并行：CPU是真的有多个核心，可以同时进行不同的运算，宏观和微观上都是真实同时进行的


为什么并发比串行慢？

只有1个CPU的情况下，并发会涉及到比较多的上下文切换，在调度上会花费一定的时间