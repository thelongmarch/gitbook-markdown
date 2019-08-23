
###　初识Nginx

Nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的IO性能，时常用于服务端的反向代理和负载均衡。


### Nginx的优点

* 支持海量高并发：采用IO多路复用epoll。官方测试Nginx能够支持5万并发链接，实际生产环境中可以支撑2-4万并发连接数。

* 内存消耗少：在主流的服务器中Nginx目前是内存消耗最小的了，比如我们用Nginx+PHP，在3万并发链接下，开启10个Nginx进程消耗150M内存。

* 免费使用可以商业化：Nginx为开源软件，采用的是2-clause BSD-like协议，可以免费使用，并且可以用于商业。

* 配置文件简单：网络和程序配置通俗易懂，即使非专业运维也能看懂。

###  用yum进行安装必要程序

```
yum -y install gcc gcc-c++ autoconf pcre-devel make automake
yum -y install wget httpd-tools vim

```

### Nginx版本说明

* Mainline version ：开发版,主要是给广大Nginx爱好者，测试、研究和学习的，但是不建议使用于生产环境。

* Stable version : 稳定版,也就是我们说的长期更新版本。这种版本一般比较成熟，经过长时间的更新测试，所以这种版本也是主流版本。

* legacy version : 历史版本，如果你需要以前的版本，Nginx也是有提供的。

### 基于Yum的方式安装Nginx

1. 查看yum是否已经存在，命令如下：

```
yum list | grep nginx

```
如果出现类似下面的内容，说明yum源是存在的
![yum 源](http://qiniu.themarch.cn/nginxgrep.png  ''yum 源'')

(原来的源只支持1.1.12版本，版本有些低)

不存在：
![yum 源](http://qiniu.themarch.cn/nginx.png  ''yum 源'')

2. 如果不存在，或者不是你需要的版本，那我们可以自行配置yum源，下面是官网提供的源，我们可以放心大胆的使用。

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/OS/OSRELEASE/$basearch/
gpgcheck=0
enabled=1
```
复制上面的代码，然后在终端里输入：
```
vim /etc/yum.repos.d/nginx.repo
```

然后把代码复制进去。

赋值完成后，你需要修改一下对应的操作系统和版本号，因为我的是centos和7的版本，所以改为这样。
```
baseurl=http://nginx.org/packages/centos/7/$basearch/
```
你可以根据你的系统或需要的版本进行修改。

更新了yum源之后：
![yum 源](http://qiniu.themarch.cn/yumyuan.png  ''yum 源'')

tip:官网源地址
![源地址](http://qiniu.themarch.cn/yuan.png  ''源地址'')


3. 如果都已经准备好了，那就可以开始安装了，安装的命令非常简单：
```
yum install nginx
```
4. 安装完成后可以使用命令，来检测Nginx的版本。
```
nginx -v
```
如果出现下面图片的内容，说明Nginx就安装成功了。
![版本](http://qiniu.themarch.cn/nginxversion.png  ''版本'')

tip:区别与nginx -V（大写），此命令用于查看所有命令

### Nginx基本配



