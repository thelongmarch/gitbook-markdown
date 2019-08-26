
# centos7 开启80端口

关闭与开启防火墙
```
systemctl stop firewalld.service
systemctl start firewalld.service
```

先查看防火墙是否开启的状态，以及开放端口的情况：
```
systemctl status firewalld.service
sudo firewall-cmd --list-all
```
如下显示，services: dhcpv6-client ssh 表示 ssh 服务是放行的，而 ports: 这里为空，表示无端口号放行。
```
[root@localhost sysconfig]# sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens33
  sources: 
  services: ssh dhcpv6-client
  ports: 
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
	
[root@localhost sysconfig]# 
```

接下来通过以下命令开放http 80 端口：
```
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-port=80/tcp --permanent
```
命令末尾的--permanent表示用久有效，不加这句的话重启后刚才开放的端口就又失效了。

然后重启防火墙：
```
sudo firewall-cmd --reload
```
再次查看端口的开放情况：
```
sudo firewall-cmd --list-all
```
就会发现 services: 里出现了 http 服务，ports：里也出现了 80 端口：
```
[root@localhost sysconfig]# sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens33
  sources: 
  services: ssh dhcpv6-client http
  ports: 80/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
	
[root@localhost sysconfig]#
```
最后换另一台电脑重新访问虚拟机的IP地址，成功了~

# centos7安装telnet服务

先检查CentOS7.0是否已经安装以下两个安装包:telnet-server、xinetd。命令如下：
```
rpm -qa telnet-server
rpm -qa xinetd
```

如果没有安装，则先安装。安装命令： 
1. 安装telnet
```
yum list |grep telnet
```
```
[root@localhost conf.d]# yum list |grep telnet
telnet.x86_64                               1:0.17-64.el7              base     
telnet-server.x86_64                        1:0.17-64.el7              base     
[root@localhost conf.d]#
```

```
yum install telnet-server.x86_64
yum install telnet.x86_64
```

2. 安装xinetd
```
yum list |grep xinetd
yum install xinetd.x86_64
```

3. 安装完成后，将xinetd服务加入开机自启动:
```
systemctl enable xinetd.service
```
将telnet服务加入开机自启动：
```
systemctl enable telnet.socket
```
最后，启动以上两个服务即可： 
由于telnet服务也是由xinetd守护的，所以安装完telnet-server，要启动telnet服务就必须重新启动xinetd 。
```
systemctl start telnet.socket
systemctl start xinetd
(或service xinetd start)
```