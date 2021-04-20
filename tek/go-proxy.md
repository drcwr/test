    https://pylist.com/t/1575004245
    
    
    export GOPROXY=https://goproxy.cn
    
    全局代理
    这是最优先的也是最方便的方案，前提是有可以使用的或可以切换的全局代理
    
    使用 GOPROXY
    在终端实行
    
    export GO111MODULE=on
    go env -w GOPROXY=https://goproxy.cn,direct
    或
    
    export GO111MODULE=on
    export GOPROXY=https://goproxy.cn
    也可配置文件修改
    
    $ echo "export GOPROXY=https://goproxy.cn" >> ~/.bash_profile && source ~/.bash_profile
    使用 socks5 代理
    上面两种方式都不能解决时，就可用下面方法试试
    
    设置 git 代理
    
    git config --global http.proxy 'socks5://127.0.0.1:10086'
    git config --global https.proxy 'socks5://127.0.0.1:10086'
    设置 go get 代理来安装
    
    http_proxy=socks5://127.0.0.1:10086 https_proxy=socks5://127.0.0.1:10086 go get -u ...
    重设 git 代理
    
    git config --global --unset http.proxy
    git config --global --unset https.proxy
    上面是使用 socks5 作代理，你可以使用其它的方式如socks 或http、https 等。
