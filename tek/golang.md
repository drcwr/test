## GPM
- goroutine
- scheduler

## CHANNEL

## GC

## context

## atomic

## demo
[code](https://github.com/drcwr/godemos)


# go runtime 
## gpm调度模型 
```
g 应用逻辑 goroutine 协程 用户态
m 干活的   machine 对应一个内核线程，g在m上执行，默认最大限制为10000个
p 调度的   process 代表处理器 P的个数就是GOMAXPROCS（最大256），并发任务的数量，每一个运行的M都必须绑定一个P，每一个P保存着本地G任务队列；还有一个全局G任务队列，P/M需要进行绑定
```

![scheduler](files/22-go-goroutine.svg)
```
调度流程
在M与P绑定后，M会不断从P的Local队列(runq)中取出G(无锁操作)，切换到G的堆栈并执行，当P的Local队列中没有G时，再从Global队列中返回一个G(有锁操作，因此实际还会从Global队列批量转移一批G到P Local队列)，当Global队列中也没有待运行的G时，则尝试从其它的P窃取(steal)部分G来执行

```
- sysmon

#### gotoutine 状态
- gopark()
- goready()


参考
- https://blog.csdn.net/jinyidong/article/details/88235290
- https://www.cnblogs.com/lvpengbo/p/13973906.html
- https://blog.csdn.net/liangzhiyang/article/details/52669851
- https://zhuanlan.zhihu.com/p/95056679


## 并发原语

## 内存空间结构
- tcmalloc span,go page=8kB,
### 两阶稀疏索引
- 在1.11 中
- 可以超过 512G 内存, 
- 也可以允许内存空间扩展时不连续.
 
 在全局的 mheap struct 中有个 arenas 二阶数组
 - slot ，4M 个。在 linux amd64 上,一阶只有一个 slot, 二阶有 4M 个 slot
 - heapArena ，64M 内存。每个 slot 指向一个 heapArena 结构, 每个 heapArena 结构可以管理 64M 内存
 - 256TB 内存。go 可以管理 4M*64M=256TB 内存, 即目前 64 位机器中 48bit 的寻址总线全部 256TB 内存




## gc
## 性能问题排查和优化


- 自举
https://blog.csdn.net/byxiaoyuonly/article/details/112430074

