# test
* test
***
### CentOS-7-x86_64-DVD-1804.iso
http://101.96.10.63/isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso
http://mirrors.aliyun.com/centos/7.5.1804/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso

***
### dash vs bash
后来发现，因为Ubuntu默认的sh是连接到dash的,又因为dash跟bash的不兼容所以出错了.执行时可以把sh换成bash文件名.sh来执行.成功.dash是什么东西,查了一下,应该也是一种shell,貌似用户对它的诟病颇多。

修改sh默认连接到bash的方法：

* 第一种  
    sudo dpkg-reconfigure dash  
    选择no 即可！  
    再次编译！通过！搞定！  



* 第二种  
    通过bash命令执行  
    bash install.sh  

***
### smartgithg
~~https://blog.csdn.net/u011534057/article/details/54582951~~

 ~~sudo add-apt-repository ppa:eugenesan/ppa  
    sudo apt-get update  
    sudo apt-get install smartgithg~~
https://www.syntevo.com/smartgit/download/
https://www.syntevo.com/downloads/smartgit/smartgit-linux-18_1_5.tar.gz

***

### HyperLedger Fabric ChainCode开发——shim.ChaincodeStubInterface用法


http://www.cnblogs.com/studyzy/p/7360733.html

***

### go time string

    current := time.Now()

    fmt.Println("origin : ", current.String())
    // origin :  2016-09-02 15:53:07.159994437 +0800 CST

    fmt.Println("mm-dd-yyyy : ", current.Format("01-02-2006"))
    // mm-dd-yyyy :  09-02-2016


### Hyperledger Fabric 1.0 链码（chaincode）的原理、接口和结构
https://blog.csdn.net/so5418418/article/details/78668109

***

### ubuntu ip config

* static ip

    cat /etc/network/interfaces
    #interfaces(5) file used by ifup(8) and ifdown(8)
    auto enp0s3
    iface enp0s3 inet static
    address 192.168.1.40
    netmask 255.255.255.0
    gateway 192.168.1.1


**dns-nameservers 202.38.64.1**

    cat /etc/resolv.conf 
    # Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
    #     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
    nameserver 192.168.1.1





* dhcp 

    cat /var/lib/dhcp/dhclient.leases  
     lease {  
     interface "enp0s3";  
     fixed-address 192.168.1.18;  
     option subnet-mask 255.255.255.0;  
     option dhcp-lease-time 86400;  
     option routers 192.168.1.1;  
     option dhcp-message-type 5;  
     option dhcp-server-identifier 192.168.1.1;  
     option domain-name-servers 192.168.1.1;  
     renew 2 2018/11/13 21:38:06;  
     rebind 3 2018/11/14 08:33:45;  
     expire 3 2018/11/14 11:33:45;  
     }  
     lease {   
     interface "enp0s3";  
     fixed-address 192.168.1.18;  
     option subnet-mask 255.255.255.0;  
     option routers 192.168.1.1;  
     option dhcp-lease-time 86400;  
     option dhcp-message-type 5;  
     option domain-name-servers 192.168.1.1;  
     option dhcp-server-identifier 192.168.1.1;  
     renew 2 2018/11/13 23:17:51;  
     rebind 3 2018/11/14 08:33:47;    
     expire 3 2018/11/14 11:33:47;    
     }

* dns
    /etc/resolvconf/resolv.conf.d/base
    nameserver xxx.xxx.xxx.xxx
***
### go network transfer
    recv  
    url.QueryUnescape  
    json.Unmarshal  
    modify  
    json.Marshal  
    url.QueryEscape  
    send  

***
### fabric源码解析

https://blog.csdn.net/idsuf698987/
