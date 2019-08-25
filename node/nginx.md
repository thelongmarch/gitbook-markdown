
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

### Nginx基本配置文件详讲

1. 查看Nginx的安装目录

在使用yum安装完Nginx后，需要知道系统中多了那些文件，它们都安装到了那里。可以使用下面的命令进行查看：
```
rpm -ql nginx
```
rpm 是linux的rpm包管理工具，-q 代表询问模式，-l 代表返回列表，这样我们就可以找到nginx的所有安装位置了。

拓展：

使用ls命令即可区分文件夹和文件。

示例： ls -la 

说明：

-l 列出文件的详细信息。

-a 列出目录下的所有文件，包括以 . 开头的隐含文件。


要判断是文件还是文件夹，看第一列的内容即可，第一列的第一个字母指明了文件类型：-”表示普通文件，“d”代表目录，“l”代表连接文件，“b”代表设备文件。

只显示文件夹     ls -l | grep ^d

只显示文件        ls -l | grep ^-

```
[root@localhost ~]# rpm -ql nginx
/etc/logrotate.d/nginx
/etc/nginx
/etc/nginx/conf.d
/etc/nginx/conf.d/default.conf
/etc/nginx/fastcgi_params
/etc/nginx/koi-utf
/etc/nginx/koi-win
/etc/nginx/mime.types
/etc/nginx/modules
/etc/nginx/nginx.conf
/etc/nginx/scgi_params
/etc/nginx/uwsgi_params
/etc/nginx/win-utf
/etc/sysconfig/nginx
/etc/sysconfig/nginx-debug
/usr/lib/systemd/system/nginx-debug.service
/usr/lib/systemd/system/nginx.service
/usr/lib64/nginx
/usr/lib64/nginx/modules
/usr/libexec/initscripts/legacy-actions/nginx
/usr/libexec/initscripts/legacy-actions/nginx/check-reload
/usr/libexec/initscripts/legacy-actions/nginx/upgrade
/usr/sbin/nginx
/usr/sbin/nginx-debug
/usr/share/doc/nginx-1.16.1
/usr/share/doc/nginx-1.16.1/COPYRIGHT
/usr/share/man/man8/nginx.8.gz
/usr/share/nginx
/usr/share/nginx/html
/usr/share/nginx/html/50x.html
/usr/share/nginx/html/index.html
/var/cache/nginx
/var/log/nginx
[root@localhost ~]# 
```
###　nginx.conf文件解读

nginx.conf 文件是Nginx总配置文件，在我们搭建服务器时经常调整的文件。

进入etc/nginx目录下，然后用vim进行打开
```
cd /etc/nginx
vim nginx.conf

```

下面是文件的详细注释:

```
#运行用户，默认即是nginx，可以不进行设置
user  nginx;
#Nginx进程，一般设置为和CPU核数一样
worker_processes  1;   
#错误日志存放目录
error_log  /var/log/nginx/error.log warn;
#进程pid存放位置
pid        /var/run/nginx.pid;


events {
    worker_connections  1024; # 单个后台进程的最大并发数
}


http {
    include       /etc/nginx/mime.types;   #文件扩展名与类型映射表
    default_type  application/octet-stream;  #默认文件类型
    #设置日志模式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;   #nginx访问日志存放位置

    sendfile        on;   #开启高效传输模式
    #tcp_nopush     on;    #减少网络报文段的数量

    keepalive_timeout  65;  #保持连接的时间，也叫超时时间

    #gzip  on;  #开启gzip压缩

    include /etc/nginx/conf.d/*.conf; #包含的子配置项位置和文件

```

conf.d 文件夹中只有一个默认配置文件default.conf。

default.conf 配置项讲解 我们看到最后有一个子文件的配置项，那我们打开这个include子文件配置项看一下里边都有些什么内容。

进入conf.d目录，然后使用vim default.conf进行查看。

```
server {
    listen       80;   #配置监听端口
    server_name  localhost;  //配置域名

    #charset koi8-r;     
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;     #服务默认启动目录
        index  index.html index.htm;    #默认访问文件
    }

    #error_page  404              /404.html;   # 配置404页面

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;   #错误状态码的显示页面，配置后需要重启
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}

```

### Nginx服务启动、停止、重启







