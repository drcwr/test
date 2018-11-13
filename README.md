# test
test

http://101.96.10.63/isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso
http://mirrors.aliyun.com/centos/7.5.1804/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso


dash vs bash
后来发现，因为Ubuntu默认的sh是连接到dash的,又因为dash跟bash的不兼容所以出错了.执行时可以把sh换成bash文件名.sh来执行.成功.dash是什么东西,查了一下,应该也是一种shell,貌似用户对它的诟病颇多。

修改sh默认连接到bash的方法：

第一种
sudo dpkg-reconfigure dash

选择no 即可！

再次编译！通过！搞定！



第二种
通过bash命令执行
bash install.sh


https://blog.csdn.net/u011534057/article/details/54582951

sudo add-apt-repository ppa:eugenesan/ppa

sudo apt-get update

sudo apt-get install smartgithg




HyperLedger Fabric ChainCode开发——shim.ChaincodeStubInterface用法

http://www.cnblogs.com/studyzy/p/7360733.html



commit b259996fd4dc006432a9555ac29e5d049f682eef
Author: jia.hu <jia.hu@yefeng.com>
Date:   Tue Nov 6 18:47:25 2018 +0800

    temp work

current := time.Now()

    fmt.Println("origin : ", current.String())
    // origin :  2016-09-02 15:53:07.159994437 +0800 CST

    fmt.Println("mm-dd-yyyy : ", current.Format("01-02-2006"))
    // mm-dd-yyyy :  09-02-2016


Hyperledger Fabric 1.0 链码（chaincode）的原理、接口和结构
https://blog.csdn.net/so5418418/article/details/78668109
