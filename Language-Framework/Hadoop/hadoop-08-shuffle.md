# shuffle 机制



**Map Reduce 如何分配 Map 的并发个数**

* 通过切片的数量决定的，有多少个切片就启动多少个 map task
* 切片是一个**逻辑**的概念，指的是文件中数据的偏移范围
* 切片的具体大小应该依据所处理文件的大小进行调整



**Map --> Reduce 之间经历过了什么**



# 一个main里启动两个job

