## 架构

```
中间的四个动词就是 Istio 的主要功能，官方也各有一句话的说明。这里再阐释一下：

连接（Connect）：智能控制服务之间的调用流量，能够实现灰度升级、AB 测试和红黑部署等功能
安全加固（Secure）：自动为服务之间的调用提供认证、授权和加密。
控制（Control）：应用用户定义的 policy，保证资源在消费者中公平分配。
观察（Observe）：查看服务运行期间的各种数据，比如日志、监控和 tracing，了解服务的运行情况。
```


### 控制面
```
控制面是istio的核心
```
- pilot 主要控制组件
- mixer 前置条件检查和收集遥测数据
```
将策略逻辑从业务的微服务中移出到mixer，并由运维人员控制。

前置条件检查 acl、黑白名单
收集遥测数据 log，监控

配置模型
适配器

```

- citadel(auth) 安全

### 数据面 
- envoy 
```
"sidecar",在Kubernets中自动注入方式到业务pod中
```

### 流程




## 参考
- https://www.cnblogs.com/juchangfei/p/12802672.html
- https://cloud.tencent.com/developer/article/1552356
- https://blog.csdn.net/luanpeng825485697/article/details/84560659
- [Istio：Mixer功能架构与实践](https://blog.csdn.net/fly910905/article/details/104040395)
- Istio是啥？一文带你彻底了解！ http://www.uml.org.cn/wfw/201909063.asp
