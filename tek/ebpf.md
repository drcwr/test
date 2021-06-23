
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




## ubuntu 18.04 安装 ebpf 运行环境
1. 按照 bcc/INSTALL.md 操作
```
sudo apt-get install bpfcc-tools linux-headers-$(uname -r)
```

然后运行 bcc/tools/softirqs.py，异常。

```
Failed to attach BPF to kprobe
```

使用 bcc/QUICKSTART.md 中的容器方式运行，对比
```bash
docker run -it --rm \
  --privileged \
  -v /lib/modules:/lib/modules:ro \
  -v /usr/src:/usr/src:ro \
  -v /etc/localtime:/etc/localtime:ro \
  --workdir /usr/share/bcc/tools \
  zlim/bcc
```

可以正常运行。

由于/lib/modules和/usr/src使用的是宿主机上的内容，所以可以排除内核配置，kernel-dev,kernel-headler等方面的问题。

查看容器中的bcc版本是0.8.0

```
root@0c36b375377b:/usr/share/bcc/tools# ll /usr/lib/x86_64-linux-gnu/libbcc.so*
lrwxrwxrwx 1 root root       11 Feb 24  2019 /usr/lib/x86_64-linux-gnu/libbcc.so -> libbcc.so.0
lrwxrwxrwx 1 root root       15 Feb 24  2019 /usr/lib/x86_64-linux-gnu/libbcc.so.0 -> libbcc.so.0.8.0
-rw-r--r-- 1 root root 57803032 Feb 24  2019 /usr/lib/x86_64-linux-gnu/libbcc.so.0.8.0

```


宿主机的版本是ubuntu 18.04源自带的 0.5.0 版本。


## 20.04
而 20.04源的libbcc 版本是 0.12.0。// bcc/tools/tcpdrop.py 异常
在 /etc/apt/sources.list.d/ 添加 20.04 的源

```
cat /etc/apt/sources.list.d/sources20.4.list
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

```
然后更新libbcc

```bash
sudo apt-get update
```
```
apt-cache madison bpfcc-tools
bpfcc-tools |   0.12.0-2 | http://mirrors.aliyun.com/ubuntu focal/universe amd64 Packages
bpfcc-tools |   0.12.0-2 | http://mirrors.aliyun.com/ubuntu focal/universe i386 Packages
bpfcc-tools | 0.5.0-5ubuntu1 | http://mirrors.aliyun.com/ubuntu bionic/universe amd64 Packages
bpfcc-tools | 0.5.0-5ubuntu1 | http://mirrors.aliyun.com/ubuntu bionic/universe i386 Packages
     bpfcc | 0.5.0-5ubuntu1 | http://mirrors.aliyun.com/ubuntu bionic/universe Sources
     bpfcc |   0.12.0-2 | http://mirrors.aliyun.com/ubuntu focal/universe Sources
```
```
sudo apt install bpfcc-tools=0.12.0-2
```
```
apt-cache policy   bpfcc-tools
bpfcc-tools:
  Installed: 0.12.0-2
  Candidate: 0.12.0-2
  Version table:
 *** 0.12.0-2 500
        500 http://mirrors.aliyun.com/ubuntu focal/universe amd64 Packages
        500 http://mirrors.aliyun.com/ubuntu focal/universe i386 Packages
        100 /var/lib/dpkg/status
     0.5.0-5ubuntu1 500
        500 http://mirrors.aliyun.com/ubuntu bionic/universe amd64 Packages
        500 http://mirrors.aliyun.com/ubuntu bionic/universe i386 Packages
```
```
ll /usr/lib/x86_64-linux-gnu/libbcc.so*
lrwxrwxrwx 1 root root  16 2月   7  2020 /usr/lib/x86_64-linux-gnu/libbcc.so.0 -> libbcc.so.0.12.0
-rw-r--r-- 1 root root 58M 2月   7  2020 /usr/lib/x86_64-linux-gnu/libbcc.so.0.12.0

```

再次运行，已经正常。

```
tools git:(master) ✗ ./softirqs.py
Tracing soft irq event time... Hit Ctrl-C to end.
^C
SOFTIRQ          TOTAL_usecs
net_tx                    16
hi                      1536
hrtimer                 5430
tasklet                13722
timer                  27854
net_rx                 45018
rcu                    93992
sched                 138172

```

## 使用 The iovisor packages 
```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4052245BD4284CDD
echo "deb https://repo.iovisor.org/apt/$(lsb_release -cs) $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/iovisor.list
sudo apt-get update
sudo apt-get install bcc-tools libbcc-examples linux-headers-$(uname -r)
```

```
ll /usr/lib/x86_64-linux-gnu/libbcc*
lrwxrwxrwx 1 root root   15 5月  29  2019 /usr/lib/x86_64-linux-gnu/libbcc_bpf.so -> libbcc_bpf.so.0
lrwxrwxrwx 1 root root   20 5月  29  2019 /usr/lib/x86_64-linux-gnu/libbcc_bpf.so.0 -> libbcc_bpf.so.0.10.0
-rw-r--r-- 1 root root 129K 5月  29  2019 /usr/lib/x86_64-linux-gnu/libbcc_bpf.so.0.10.0
lrwxrwxrwx 1 root root   11 5月  29  2019 /usr/lib/x86_64-linux-gnu/libbcc.so -> libbcc.so.0
lrwxrwxrwx 1 root root   16 5月  29  2019 /usr/lib/x86_64-linux-gnu/libbcc.so.0 -> libbcc.so.0.10.0
-rw-r--r-- 1 root root  56M 5月  29  2019 /usr/lib/x86_64-linux-gnu/libbcc.so.0.10.0
```

bcc/tools/tcpdrop.py // v0.9.0 可以正常运行。 v0.13.0

```
v0.14.0
commit 7f6066d250efa138419c16c150ad8711d6528d29
Author: Sumanth Korikkar <sumanthk@linux.ibm.com>
Date:   Wed May 20 10:49:56 2020 -0500

    bcc/tools: Replace bpf_probe_read with bpf_probe_read_kernel
    
    It is recommended to use bpf_probe_read_kernel_{str} in the bpf tools.
    See kernel commit 0ebeea8ca8a4 ("bpf: Restrict bpf_probe_read{, str}()
    only to archs where they work")
    
    Signed-off-by: Sumanth Korikkar <sumanthk@linux.ibm.com>
    Acked-by: Ilya Leoshkevich <iii@linux.ibm.com>

diff --git a/tools/tcpdrop.py b/tools/tcpdrop.py
index 14515f75..aceff871 100755
--- a/tools/tcpdrop.py
+++ b/tools/tcpdrop.py
@@ -128,9 +128,9 @@ int trace_tcp_drop(struct pt_regs *ctx, struct sock *sk, struct sk_buff *skb)
         struct ipv6_data_t data6 = {};
         data6.pid = pid;
         data6.ip = 6;
-        bpf_probe_read(&data6.saddr, sizeof(data6.saddr),
+        bpf_probe_read_kernel(&data6.saddr, sizeof(data6.saddr),
             sk->__sk_common.skc_v6_rcv_saddr.in6_u.u6_addr32);
-        bpf_probe_read(&data6.daddr, sizeof(data6.daddr),
+        bpf_probe_read_kernel(&data6.daddr, sizeof(data6.daddr),
             sk->__sk_common.skc_v6_daddr.in6_u.u6_addr32);
         data6.dport = dport;
         data6.sport = sport;

```