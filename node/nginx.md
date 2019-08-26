
# 第01节：　初识Nginx

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

# 第02节： 基于Yum的方式安装Nginx

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

# 第03节： Nginx基本配置文件详讲

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

# 第04节： Nginx服务启动、停止、重启

#### 启动Nginx服务

默认的情况下，Nginx是不会自动启动的，需要我们手动进行启动，当然启动Nginx的方法也不是单一的。

1. nginx直接启动

在CentOS7.4版本里（低版本是不行的），是可以直接直接使用nginx启动服务的。
```
nginx
```

2. 使用systemctl命令启动

还可以使用个Linux的命令进行启动，我一般都是采用这种方法进行使用。因为这种方法无论启动什么服务，都是一样的，只是换一下服务的名字（不用增加额外的记忆点）。
```
systemctl start nginx.service
```
输入命令后，没有任何提示，那我们如何知道Nginx服务已经启动了哪？可以使用Linux的组合命令，进行查询服务的运行状况。
```
ps aux | grep nginx
```
如果启动成功会出现如下类似的结果。
```
[root@localhost conf.d]# ps aux | grep nginx
root       9011  0.0  0.0  46340   980 ?        Ss   10:41   0:00 nginx: master process nginx
nginx      9012  0.0  0.1  48836  2012 ?        S    10:41   0:00 nginx: worker process
root       9014  0.0  0.0 112724   984 pts/0    S+   10:41   0:00 grep --color=auto nginx
[root@localhost conf.d]# 
```
有这三条记录，说明我们Nginx被正常开启了。

#### 停止Nginx服务的四种方法

停止Nginx 方法有很多种，可以根据需求采用不一样的方法。

1. 立即停止服务
```
nginx  -s stop
```
这种方法比较强硬，无论进程是否在工作，都直接停止进程。

2. 从容停止服务
```
nginx -s quit
```
这种方法较stop相比就比较温和一些了，需要进程完成当前工作后再停止。

3. killall 方法杀死进程

这种方法也是比较野蛮的，我们直接杀死进程，但是在上面使用没有效果时，我们用这种方法还是比较好的。
```
killall nginx
```
tip:

centos下没有killall命令的解决方法：
```
yum install psmisc
```

4. systemctl 停止
```
systemctl stop nginx.service
```
#### 重启Nginx服务

有时候我们需要重启Nginx服务，这时候可以使用下面的命令。
```
systemctl restart nginx.service
```
#### 重新载入配置文件

在重新编写或者修改Nginx的配置文件后，都需要作一下重新载入，这时候可以用Nginx给的命令。
```
nginx -s reload
```
查看端口号

在默认情况下，Nginx启动后会监听80端口，从而提供HTTP访问，如果80端口已经被占用则会启动失败。我么可以使用netstat -tlnp命令查看端口号的占用情况。


# 第05节：自定义错误页和访问设置

多错误指向一个页面

在/etc/nginx/conf.d/default.conf 是可以看到下面这句话的。

error_page   500 502 503 504  /50x.html;
error_page指令用于自定义错误页面，500，502，503，504 这些就是HTTP中最常见的错误代码，/50.html 用于表示当发生上述指定的任意一个错误的时候，都是用网站根目录下的/50.html文件进行处理。

单独为错误置顶处理方式

有些时候是要把这些错误页面单独的表现出来，给用户更好的体验。所以就要为每个错误码设置不同的页面。设置方法如下：

error_page 404  /404_error.html;
然后到网站目录下新建一个404_error.html 文件，并写入一些信息。

<html>
<meta charset="UTF-8">
<body>
<h1>404页面没有找到!</h1>
</body>
</html>
然后重启我们的服务，再进行访问，你会发现404页面发生了变化。

把错误码换成一个地址

处理错误的时候，不仅可以只使用本服务器的资源，还可以使用外部的资源。比如我们将配置文件设置成这样。

error_page  404 http://jspang.com;
我们使用了技术胖的博客地址作为404页面没有找到的提示，就形成了，没有找到文件，就直接跳到了技术胖的博客上了。

简单实现访问控制

有时候我们的服务器只允许特定主机访问，比如内部OA系统，或者应用的管理后台系统，更或者是某些应用接口，这时候我们就需要控制一些IP访问，我们可以直接在location里进行配置。

可以直接在default.conf里进行配置。

 location / {
        deny   123.9.51.42;
        allow  45.76.202.231;
    }

配置完成后，重启一下服务器就可以实现限制和允许访问了。这在工作中非常常用，一定要好好记得。









