

## src
- [git](https://github.com/istio/envoy) https://github.com/istio/envoy.git


## 架构分析
### 线程模型
1. 线程类型 

main worker 文件刷新器

- main
```
一切都是异步和“非阻塞”
启动和关闭服务器
xDS api 处理
运行时 统计刷新 管理
一般进程管理（信号，热启动）
单线程运行

```

- worker
```
硬件线程 生成一个 工作线程，-concurrency
非阻塞 事件循环
监听 接受新连接 实例化过滤器栈 处理连接生命周期内IO事件
近似单线程运行
```

- 文件刷新器
```
每个文件都有一个独立的刷新线程（主要是访问日志）
工作线程写文件，数据移入内存缓冲区；最终 文件刷新器刷新至磁盘
共享内存区域，worker线程 可能 阻塞
```



2. 连接映射到线程
3. 线程本地存储(TLS thread local store)

高度并行，且高性能





## 参考
1. [Envoy为什么能战胜Ngnix——线程模型分析篇](https://www.sohu.com/a/244966023_268033)
