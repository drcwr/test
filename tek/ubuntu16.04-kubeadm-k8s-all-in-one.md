```
ubuntu16.04 kubeadm安装k8s all-in-one单机测试环境

Hansyang84 2021-02-23 15:51:17  75  收藏
文章标签： docker linux
版权
ubuntu16.04 kubeadm安装k8s all-in-one单机测试环境
1.把源换成阿里的源
2.关闭swap
3.关闭防火墙
4.关闭selinux
5.配置hosts文件
6.安装docker
7.添加Kubernetes软件源
8.安装kubectl，kubelet，kubeadm
9.查看kubeadm安装k8s需要的镜像
10.从阿里下载相关的镜像
11.把镜像重新命名原来的名字
12.kubeadm部署k8s相关服务
13.kubeadm部署k8s网络服务
14.查看部署情况
16.过程中遇到的问题

1.把源换成阿里的源
cp -rp /etc/apt/sources.list /etc/apt/sources.listbak && > /etc/apt/sources.list 
cat <<EOF > /etc/apt/sources.list 
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse  
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse  
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse  
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
EOF

2.关闭swap
swapoff  -a
sed -ri 's/.*swap.*/#&/' /etc/fstab

3.关闭防火墙
systemctl stop firewalld && systemctl disable firewalld

4.关闭selinux
setenforce 0

5.配置hosts文件
echo "192.168.239.132 host01" >> /etc/hosts	

6.安装docker
# 1.先安装相关工具
apt-get update && apt-get install -y apt-transport-https curl
# 2.添加密钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
# 3.安装docker
apt-get install docker.io -y
# 查看docker版本
docker version
# 启动docker service
systemctl enable docker
systemctl start docker
systemctl status docker
# 使用阿里云加速器, 修改文件
echo > /etc/docker/daemon.json
cat <<EOF > /etc/docker/daemon.json
{
    "registry-mirrors": ["https://alzgoonw.mirror.aliyuncs.com"],
    "live-restore": true
}
EOF

# 重起docker服务
systemctl daemon-reload
systemctl restart docker

7.添加Kubernetes软件源
> /etc/apt/sources.list.d/kubernetes.list
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial main
EOF

8.安装kubectl，kubelet，kubeadm
apt-get update && apt-get install -y kubelet kubeadm kubectl 
apt-get install -y kubelet=1.20.4-00 kubeadm=1.20.4-00 kubectl=1.20.4-00 --allow-downgrades

9.查看kubeadm安装k8s需要的镜像
kubeadm config images list
k8s.gcr.io/kube-apiserver:v1.20.4
k8s.gcr.io/kube-controller-manager:v1.20.4
k8s.gcr.io/kube-scheduler:v1.20.4
k8s.gcr.io/kube-proxy:v1.20.4
k8s.gcr.io/pause:3.2
k8s.gcr.io/etcd:3.4.13-0
k8s.gcr.io/coredns:1.7.0

10.从阿里下载相关的镜像
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.20.4
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.20.4
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.20.4
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.2
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.4.13-0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.7.0
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.20.4

11.把镜像重新命名原来的名字
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.20.4 k8s.gcr.io/kube-controller-manager:v1.20.4
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.20.4 k8s.gcr.io/kube-scheduler:v1.20.4
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.20.4 k8s.gcr.io/kube-proxy:v1.20.4
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.2 k8s.gcr.io/pause:3.2
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.4.13-0 k8s.gcr.io/etcd:3.4.13-0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.7.0 k8s.gcr.io/coredns:1.7.0
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.20.4 k8s.gcr.io/kube-apiserver:v1.20.4

12.kubeadm部署k8s相关服务
kubeadm init --pod-network-cidr=10.244.0.0/16  --kubernetes-version=v1.20.4 --ignore-preflight-errors=Swap

13.kubeadm部署k8s网络服务
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml


14.0
cp /etc/kubernetes/admin.conf ~/.kube/config


14.查看部署情况
kubectl get nodes
NAME     STATUS   ROLES                  AGE     VERSION
ubuntu   Ready    control-plane,master   2d21h   v1.20.4
# master节点负载node
kubectl taint nodes --all node-role.kubernetes.io/master-

16.过程中遇到的问题
apt-get update && apt-get install -y kubelet kubeadm kubectl 

Hit:1 http://mirrors.aliyun.com/ubuntu xenial InRelease
Hit:2 http://mirrors.aliyun.com/ubuntu xenial-security InRelease                                                                       
Get:3 http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial InRelease [9,383 B]                  
Hit:4 http://mirrors.aliyun.com/ubuntu xenial-updates InRelease                                                    
Ign:3 http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial InRelease       
Hit:5 http://mirrors.aliyun.com/ubuntu xenial-backports InRelease                 
Fetched 9,383 B in 0s (20.2 kB/s)
Reading package lists... Done
W: GPG error: http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 6A030B21BA07F4FB  NO_PUBKEY 8B57C5C2836F4BEB
W: The repository 'http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial InRelease' is not signed.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
#  -recv-keys BA07F4FB 为上面报错信息中NO_PUBKEY的；后8位
gpg --keyserver keyserver.ubuntu.com --recv-keys BA07F4FB
gpg: directory `/root/.gnupg' created
gpg: new configuration file `/root/.gnupg/gpg.conf' created
gpg: WARNING: options in `/root/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/root/.gnupg/secring.gpg' created
gpg: keyring `/root/.gnupg/pubring.gpg' created
gpg: requesting key BA07F4FB from hkp server keyserver.ubuntu.com
gpg: /root/.gnupg/trustdb.gpg: trustdb created
gpg: key BA07F4FB: public key "Google Cloud Packages Automatic Signing Key <gc-team@google.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
#  --armor  BA07F4FB 为上面报错信息中NO_PUBKEY的；后8位
root@ubuntu:~# gpg --export --armor BA07F4FB | sudo apt-key add -
————————————————
版权声明：本文为CSDN博主「Hansyang84」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Hansyang84/article/details/113991769
```