

netmask -c 36.96.0.0:36.223.255.255


解释：

36.96.0.0 就是 00100100.01100000.00000000.00000000
36.223.255.255  就是 00100100.11011111.11111111.11111111
主要找的是96到223之间存在的子网掩码

11111111.01100000.00000000.00000000
11111111.10000000.00000000.00000000
11111111.11000000.00000000.00000000

主要看第二个数
01100000 指的是 96 
10000000 指的是 128 
11000000 指的是 192 
11100000 指的是 224 （已经超了）


32 位整数→做异或→取反→查表→解决

https://cloud.tencent.com/developer/ask/187657

# 在线工具ip--cidr

https://ip2cidr.com/

https://ip2cidr.com/bulk-ip-to-cidr-converter.php

# netmaks

1、netmask的安装：

netmaks可以在 IP范围、子网掩码、cidr、cisco等格式中互相转换，并且提供了IP地址的点分十进制、16进制、8进制、2进制之间的互相转换！

```
wget http://mirrors.sohu.com/ubuntu/pool/universe/n/netmask/netmask_2.3.12.tar.gz
tar xf netmask_2.3.12.tar.gz
cd netmask-2.3.12
./configure
make && make install
```
提示：地址已经失效,可以从下载 (https://launchpad.net/ubuntu/+source/netmask/2.3.12)压缩包,通过ftp上传到root目录（cd ~）。


2、安装完成以后，先来看一下帮助文档：

```
 # netmaks -h
 This is netmask, an address netmask generation utility
  Usage: netmask spec [spec ...]
-h, --help                    Print a summary of the options
        -v, --version                 Print the version number
        -d, --debug                   Print status/progress information
        -s, --standard                Output address/netmask pairs
           转换到标准的 ip地址/子网掩码
         -c, --cidr                    Output CIDR format address lists
           转换到CIDR格式
         -i, --cisco                   Output Cisco style address lists
           转换到Cisco反向子网掩码
         -r, --range                   Output ip address ranges
           转换到IP地址范围
         -x, --hex                     Output address/netmask pairs in hex
           转换到16进制
         -o, --octal                   Output address/netmask pairs in octal
           转换到8进制
         -b, --binary                  Output address/netmask pairs in binary
           转换到2进制
         -n, --nodns                   Disable DNS lookups for addresses
      Definitions:
        a spec can be any of:
        netmask命令接受的ip地址格式 :
          address              单独IP
          address:address      开始IP:结束IP
          address:+address     开始IP:+IP个数
          address/mask         IP/掩码
```     
3、使用情况如下：

ip范围转换到cidr格式
```
# netmask -c 192.168.0.0:192.168.2.255
```
192.168.0.0/23
192.168.2.0/24
ip范围转换到标准的子网掩码格式
```
# netmask -s 192.168.0.0:192.168.2.255
```
192.168.0.0/255.255.254.0
192.168.2.0/255.255.255.0
ip范围转换到cisco格式
```
# netmask -i 192.168.0.0:192.168.2.255
```
192.168.0.0 0.0.1.255
192.168.2.0 0.0.0.255
cidr个数转换到ip范围格式
```
# netmask -r 192.168.0.0/23
```
192.168.0.0-192.168.1.255   (512)
把点分10进制的ip转换到二进制

```
# netmask -b 192.168.0.0
```
11000000 10101000 00000000 00000000 / 11111111 11111111 11111111 11111111


# python有现成的类库支持此类网络操作与换算

```
startip = '208.130.29.30'
endip = '208.130.29.35'
cidrs = netaddr.iprange_to_cidrs(startip, endip)
for k, v in enumerate(cidrs):
    iplist = v
    print iplist
```
输出：
208.130.29.30/31
208.130.29.32/30

反过来，CIDR也能直接转成IP地址段：
```
from netaddr import *

ip = IPNetwork('192.0.2.16/29')
ip_list = list(ip)
print(ip_list)
```
输出：
[IPAddress('192.0.2.16'), IPAddress('192.0.2.17'), …, IPAddress('192.0.2.22'), IPAddress('192.0.2.23')]