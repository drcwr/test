# Linux内存管理
```
进程虚拟地址空间
页表
物理内存

vm_area_struct


```



# Linux进程调度

```
vruntime (小)=  pruntime(小) * NICE_0_LOAD/ weight(大)

Linux开始跑NORMAL的，它倾向于调度vruntime（虚拟运行时间）最小的普通进程，
vruntime要小，
要么分子小（喜欢睡，I/O型进程，pruntime不容易长大），
要么分母大（nice值低，优先级高，权重大）。

这样一个简单的公式，就同时照顾了普通进程的优先级和CPU/IO消耗情况。

```

```
深度讲解Linux内存管理和Linux进程调度
https://blog.csdn.net/21cnbao/article/details/77505330

Linux内存管理（最透彻的一篇）
https://blog.csdn.net/yanleizhouqing/article/details/90262778
```
