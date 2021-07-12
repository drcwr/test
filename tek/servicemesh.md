
## service mesh
1. 流量调度网格。
2. 控制面，数据面，分离。

## istio
开源实现
1. pilot
1. envoy 

## envoy
### 组件
1. lisenter     LDS
2. filter       
3. router       RDS
4. cluster      CDS

### 特点
1. 多线程 非阻塞 异步io
2. 热更新 xDS
3. 热重启 共享内存，rpc unix socket




https://blog.csdn.net/zhghost
- 蚂蚁金服 Service Mesh 实践探索 https://blog.51cto.com/u_15057858/2689325

