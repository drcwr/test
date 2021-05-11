### ubuntu环境，升级golang版本，从15到1.16.3

    ll /usr/bin/go
    #/usr/bin/go -> /usr/lib/go15/bin/go

    cd /usr/lib/
    wget https://dl.google.com/go/go1.16.3.linux-amd64.tar.gz
    tar zxf go1.16.3.linux-amd64.tar.gz
    mv go go1.16.3
    cd /usr/bin/
    ln -s /usr/lib/go1.16.3/bin/go go
    ls go -l
    #go -> /usr/lib/go1.16.3/bin/go
    go version
    #go version go1.16.3 linux/amd64

***

