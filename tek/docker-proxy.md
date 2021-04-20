https://blog.csdn.net/activity110/article/details/85241450

1.创建docker服务插件目录

sudo mkdir -p /etc/systemd/system/docker.service.d
1
2.创建一个名为http-proxy.conf的文件

sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf 
1
3.编辑http-proxy.conf的文件

sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf 
1
4.写入内容(将代理ip和代理端口修改成你自己的)

[Service]
Environment="HTTP_PROXY=socks5://代理ip:代理端口/"
1
2
5.重新加载服务程序的配置文件

sudo systemctl daemon-reload
1
6.重启docker

sudo systemctl restart docker
1
7.验证是否配置成功

systemctl show --property=Environment docker
————————————————
版权声明：本文为CSDN博主「大丶白」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/activity110/article/details/85241450
