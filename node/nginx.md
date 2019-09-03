
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

1. 多错误指向一个页面

在/etc/nginx/conf.d/default.conf 是可以看到下面这句话的。

```
error_page   500 502 503 504  /50x.html;
```
error_page指令用于自定义错误页面，500，502，503，504 这些就是HTTP中最常见的错误代码，/50.html 用于表示当发生上述指定的任意一个错误的时候，都是用网站根目录下的/50.html文件进行处理。

2. 单独为错误置顶处理方式

有些时候是要把这些错误页面单独的表现出来，给用户更好的体验。所以就要为每个错误码设置不同的页面。设置方法如下：
```
error_page 404  /404_error.html;
```
然后到网站目录下新建一个404_error.html 文件，并写入一些信息。
```
<html>
<meta charset="UTF-8">
<body>
<h1>404页面没有找到!</h1>
</body>
</html>
```
然后重启我们的服务，再进行访问，你会发现404页面发生了变化。

3. 把错误码换成一个地址

处理错误的时候，不仅可以只使用本服务器的资源，还可以使用外部的资源。比如我们将配置文件设置成这样。
```
error_page  404 http://www.baidu.com;
```
我们使用了百度地址作为404页面没有找到的提示，就形成了，没有找到文件，就直接跳到了百度首页了。

#### 简单实现访问控制

有时候我们的服务器只允许特定主机访问，比如内部OA系统，或者应用的管理后台系统，更或者是某些应用接口，这时候我们就需要控制一些IP访问，我们可以直接在location里进行配置。

可以直接在default.conf里进行配置。
```
 location / {
        deny   123.9.51.42;
        allow  45.76.202.231;
    }
```
下面(deny   123.9.51.42/32;)，ip范围的cidr表示方法
```
 location / {
        deny   123.9.51.42/32; 
        allow  45.76.202.231;
    }
```
下面表示只容许192.168.118.1访问
```
 location / {
      allow  192.168.118.1;
      deny   all; 
    }
```
如果先写"deny all；"再写"allow 192.168.118.1;",测试发现都不可以访问。所以deny和allow是从上往下执行，如果设置了"deny all;" ,那么永远都是403。

tip: location后面的 / 表示所有文件
    


配置完成后，重启一下服务器就可以实现限制和允许访问了。


# 第06节：Nginx访问权限详讲

在工作中，访问权限的控制需求更加复杂，例如，对于网站下的img（图片目录）是运行所有用户访问，但对于网站下的admin目录则只允许公司内部固定IP访问。这时候仅靠deny和allow这两个指令，是无法实现的。我们需要location块来完成相关的需求匹配。

上面的需求，配置代码如下：

```
    location =/img{
        allow all;
    }
    location =/admin{
        deny all;
    }
```

=号代表精确匹配，使用了=后是根据其后的模式进行精确匹配。

### 使用正则表达式设置访问权限

只有精确匹配有时是完不成我们的工作任务的，比如现在我们要禁止访问所有php的页面，php的页面大多是后台的管理或者接口代码，所以为了安全我们经常要禁止所有用户访问，而只开放公司内部访问的。

代码如下：
```
 location ~\.php$ {
        deny all;
    }

```
这样我们再访问的时候就不能访问以php结尾的文件了

### 正则相关

1、^： 匹配字符串的开始位置；
 
2、 $：匹配字符串的结束位置；
 
3、.*:   .匹配任意字符，*匹配数量0到正无穷；
 
4、\. 斜杠用来转义，\.匹配 .    特殊使用方法，记住记性了；
 
5、（值1|值2|值3|值4）：或匹配模式，例：（jpg|gif|png|bmp）匹配jpg或gif或png或bmp
 
6、i不区分大小写
　

一．正则表达式匹配，其中：
* ~ 为区分大小写匹配
* ~* 为不区分大小写匹配
* !~和!~*分别为区分大小写不匹配及不区分大小写不匹配
二．文件及目录匹配，其中：
* -f和!-f用来判断是否存在文件
* -d和!-d用来判断是否存在目录
* -e和!-e用来判断是否存在文件或目录
* -x和!-x用来判断文件是否可执行
三．rewrite指令的最后一项参数为flag标记，flag标记有：
1.last    相当于apache里面的[L]标记，表示rewrite。
2.break本条规则匹配完成后，终止匹配，不再匹配后面的规则。
3.redirect  返回302临时重定向，浏览器地址会显示跳转后的URL地址。
4.permanent  返回301永久重定向，浏览器地址会显示跳转后的URL地址。

# 第07节：Nginx设置虚拟主机

虚拟主机是指在一台物理主机服务器上划分出多个磁盘空间，每个磁盘空间都是一个虚拟主机，每台虚拟主机都可以对外提供Web服务，并且互不干扰。在外界看来，虚拟主机就是一台独立的服务器主机，这意味着用户能够利用虚拟主机把多个不同域名的网站部署在同一台服务器上，而不必再为简历一个网站单独购买一台服务器，既解决了维护服务器技术的难题，同时又极大地节省了服务器硬件成本和相关的维护费用。

配置虚拟主机可以基于端口号、基于IP和基于域名，这节课我们先学习基于端口号来设置虚拟主机。

### 基于端口号配置虚拟主机

基于端口号来配置虚拟主机，算是Nginx中最简单的一种方式了。原理就是Nginx监听多个端口，根据不同的端口号，来区分不同的网站。

我们可以直接配置在主文件里etc/nginx/nginx.conf文件里， 也可以配置在子配置文件里etc/nginx/conf.d/default.conf。我这里为了配置方便，就配置在子文件里了。当然你也可以再新建一个文件，只要在conf.d文件夹下就可以了。

修改配置文件中的server选项，这时候就会有两个server。
```
server{
        listen 8080;
        server_name localhost;
        root /usr/share/nginx/html8080;
        index index.html;
}
```
编在usr/share/nginx/html/html8080/目录下的index.html文件并查看结果。
```
<h1>welcome 8080</h1>
```
最后在浏览器中分别访问地址和带端口的地址。看到的结果是不同的。

然后我们就可以在浏览器中访问http://ip:8080了，当然你的IP跟这个肯定不一样，这个IP过几天就会过期的。

基于IP的虚拟主机

基于IP和基于端口的配置几乎一样，只是把server_name选项，配置成IP就可以了。

比如上面的配置，我们可以修改为：

server{
        listen 80;
        server_name 192.168.45.131;
        root /usr/share/nginx/html/html8080;
        index index.html;
}
这种演示需要多个IP的支持，由于我们的阿里ECS只提供了一个IP，所以这里就不给大家演示了，如果工作中用到，只要安装这种方法配置就可以了。

tip:
VMware 虚拟机下Centos7无法访问指定端口
解决方案：关闭防火墙：systemctl stop firewalld.service

怎么开启一个端口

添加

firewall-cmd --zone=public --add-port=5005/tcp --permanent   （--permanent永久生效，没有此参数重启后失效）

添加端口外部访问权限（这样外部才能访问）
firewall-cmd --add-port=5005/tcp

重新载入，添加端口后重新载入才能起作用

firewall-cmd --reload

这些之后，端口是开启成功的，如果没有成功，重启系统试试。

https://blog.csdn.net/Honnyee/article/details/81535464

# 第08节：Nginx使用域名

先要对域名进行解析，这样域名才能正确定位到你需要的IP上。 我这里新建了两个解析，分别是:

nginx.jspang.com :这个域名映射到默认的Nginx首页位置。
nginx2.jspang.com : 这个域名映射到原来的8001端口的位置。
配置以域名为划分的虚拟主机

我们修改etc/nginx/conf.d目录下的default.conf 文件，把原来的80端口虚拟主机改为以域名划分的虚拟主机。代码如下：

server {
    listen       80;
    server_name  nginx.jspang.com;
我们再把同目录下的8001.conf文件进行修改，改成如下：

server{
        listen 80;
        server_name nginx2.jspang.com;
        location / {
                root /usr/share/nginx/html/html8001;
                index index.html index.htm;
        }
}
然后我们用平滑重启的方式，进行重启，这时候我们在浏览器中访问这两个网页。

其实域名设置虚拟主机也非常简单，主要操作的是配置文件的server_name项，还需要域名解析的配合。


