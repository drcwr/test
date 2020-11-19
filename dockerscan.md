
DockerScan：Docker安全分析&测试工具
Alpha_h4ck 2017-08-22 17:13:25 337991 1
今天给大家介绍的是一款名叫DockerScan的工具，我们可以用它来对Docker进行安全分析或者安全测试。

a1.png

项目主页	http://github.com/cr0hn/dockerscan
 提交问题 	 https://github.com/cr0hn/dockerscan/issues/ 
 开发者 	 Daniel Garcia (cr0hn)  / Roberto Munoz (robskye) 
 使用文档 	 http://dockerscan.readthedocs.org 
 最新版本 	 1.0.0-Alpha-02 
 Python版本 	 3.5及以上版本 
工具快速安装
> python3.5 -m pip install -U pip
> python3.5 -m pip install dockerscan
显示选项：
> dockerscan -h
功能浏览
DockerScan目前支持以下功能：

1.      扫描：扫描网络并尝试定位Docker注册信息；

2.      注册：

a)        Delete：删除远程镜像/标签；

b)       Info：显示远程注册表信息；

c)        Push：推送镜像文件（与Docker客户端类似）；

d)       Upload：上传随机文件；

3.      镜像：

a)        Analyze：查找一个Docker镜像中的敏感信息（包括环境变量中的密码、URL和IP等等）；

b)       Extract：提取Docker镜像；

c)        Info：获取镜像元数据；

d)       Modify：修改Docker的入口点、向Docker镜像中注入反向Shell、修改Docker镜像的运行用户；

使用文档
我们现在还在完善DockerScan的使用文档，目前只能给大家提供我们在RootedCON大会上的演讲PPT【点我获取】，非常抱歉。

我们建议大家通过视频来了解DockerScan的使用方法：

视频资料1：https://youtu.be/OwX1e4y4JMk

视频资料2：https://youtu.be/UvtBGIb3E3o

其他
请不要在没有得到目标用户事先同意的情况下实用DockerScan来进行测试，由使用者自身使用不当所带来的问题开发人员不承担任何责任，同时我们也对DockerScan所带来的损失概不负责，请大家妥善使用。

除此之外，希望有能力的用户能够为我们贡献代码，并帮助我们继续完善DockerScan。如果使用过程中遇到了任何的问题，可以在本项目的GitHub主页中留言或者Pull Request。

许可证协议
本项目遵循BSDlicense许可证协议。

* 参考来源：DockerScan， FB小编Alpha_h4ck编译，转载请注明来自FreeBuf.COM