
eBPF 
通用执行引擎

由：
- 执行字节码指令
- 存储对象
- Helper 帮助函数




eBPF 相关的知名的开源项目包括但不限于以下：

- Facebook 高性能 4 层负载均衡器 [Katran](https://github.com/facebookincubator/katran)；
- Cilium 为下一代微服务 ServiceMesh 打造了具备API感知和安全高效的容器网络方案；底层主要使用 XDP 和 TC 等相关技术；
- IO Visor 项目开源的 BCC、 BPFTrace 和 [Kubectl-Trace](https://github.com/iovisor/kubectl-trace)： BCC 提供了更高阶的抽象，可以让用户采用 Python、C++ 和 Lua 等高级语言快速开发 BPF 程序；BPFTrace 采用类似于 awk 语言快速编写 eBPF 程序；Kubectl-Trace 则提供了在 kubernetes 集群中使用 BPF 程序调试的方便操作；
- CloudFlare 公司开源的 eBPF Exporter 和 bpf-tools：eBPF Exporter 将 eBPF 技术与监控 Prometheus 紧密结合起来；bpf-tools 可用于网络问题分析和排查；




## 参考
https://cloudnative.to/blog/bpf-intro/