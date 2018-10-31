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
