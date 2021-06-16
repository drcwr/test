## git init repository
    git init
    git add .
    git commit -m "init"
    git remote add origin https://github.com/drcwr/kubeopdemo.git


## 软连接
```
git clone的时候加入-c core.symlinks=true这个参数（并且重点！需要安装最新版的git for windows，我安装的v2.19.1，并且需要用管理员权限运行，原因见git-for-windows/git，软连接的建立需要管理员的特定权限）
如果windows cmd里集成了git命令（默认安装是这样的），可以通过管理员权限执行cmd，
然后到$GOPATH/src/k8s.io目录（这个也是重点，目录不要错）下执行git clone -c core.symlinks=true https://github.com/kubernetes/kubernetes，即可。
```

- https://zhuanlan.zhihu.com/p/52056165