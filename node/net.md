如何给一个公司组建一个局域网？IP地址是如何分配的？

1.需要设备:交换机或是路由器必不可少。
2.有多少人，就要分配多少私网ip地址，一般来说，ip地址的数量一定要大于使用人数。
以备不时之需。至于如何分配ip, 有很多方式，如果交换机或是路由器带有DHCP功能,可以配置DHCP地址池，让内网PC自动获取IP，一个地址池，一般可以分配254个可用IP，但也不一定，关键看掩码如何设置；如果交换机或是路由器不带DHCP功能，则需要在内网搭建一台DHCP服务器，在DHCP服务器上配置一个或是多个DHCP地址池，让PC从服务器上自动获取地址。
3.将设备连接好了，地址池配置好了，网关，VLAN设置好了，如果没有其它需求，一个简单的局域网就组建完毕了。如果要上网，还要在路由器上配置路由或是PPPOE
写了这么多，悬赏分还是0分，也算回答你了！


ipv4 ipv6


IPv4地址由4个字节构成(IPv6是16个字节)
IP地址=网络号(网络号+子网号)+主机号
IP分为五类，写成二进制，A类第一位是0，B类前两位是10，C类前三位是110
A类0.0.0.0-127.255.255.255
B类128.0.0.0-191.255.255.255
C类192.0.0.0-223.255.255.255
D类224.0.0.0-239.255.255.255
E类240.0.0.0-255.255.255.255
1、私有地址
不能用于Internet，只能用于局域网(如学校企业内网)，被局域网重复利用以缓解IP不足的问题
A类:10.0.0.1-10.255.255.254(2^24个)
B类:172.16.0.1-172.31.255.254(2^20个)
C类:192.168.0.1-192.168.255.254(2^16个)
2、127.0.0.1-127.255.255.254
环回地址，常用于浏览器进程测试同一台机器的Web服务进程，或者测试网络协议TCP/IP是否可用
3、0.0.0.0
代表本机所有IP地址，用于设备不清楚本机IP的情况
4、255.255.255.255
表示Interne所有设备地址，用于广播，会给整个互联网带来灾难性负担，因此所有路由器都阻止这种IP转发出去，此IP仅限于本地局域网广播
5、x.x.0.0(主机号全0)
主机号全0的地址为局域网的网络号，不能分配给主机


### IP是什么？
互联网协议地址（英语：Internet Protocol Address，又译为网际协议地址），缩写为IP地址（英语：IP Address），是分配给网络上使用网际协议（英语：Internet Protocol, IP）的设备的数字标签。常见的IP地址分为IPv4与IPv6两大类。

由32位二进制数组成，为便于使用，常以XXX.XXX.XXX.XXX形式表现，每组XXX代表小于或等于255的10进制数。例如维基媒体的一个IP地址是208.80.152.2。地址可分为A,B,C,D,E五大类，其中E类属于特殊保留地址。

### IP地址分类
拿那上面的百度IP，103.235.46.39作为例子，因为人类依赖十进制，所以这里的IP地址用十进制的方式表示，要让机器识别，还是得转为二进制
(十进制IP地址) 103.235.46.39
(二进制IP地址) 1110011.11101011.101110.100111
一共有8乘4=32位，这种IP方式的表示方法，叫做IPV4，这样的话，一共有多少个IP地址呢？
最小地址 00000000 00000000 00000000 00000000 也就是 0.0.0.0
最大地址 11111111 11111111 11111111 11111111 也就是 255.255.255.255
IPV4的方式，最多可以表示 255 乘 255 乘255 乘 255 = 4228250625 个ip地址 = 40亿,40亿的IPV4地址，预计将会在2020年年前后分配完毕 （具体数字记不清）,目前世界有70亿人口，本来这些IP就不够分的，偏偏还有人搞三六九等，搞出A类，B类，C类地址


### 局域网

TCP/IP 协议需要针对不同的网络进行不同的设置，且每个节点一般需要一个“IP地址”、一个“子网掩码”、一个“默认网关”。可以手动设定，也可以通过动态主机配置协议（DHCP），给客户端自动分配一个 IP 地址，避免出错，也简化了 TCP/IP 协议的设置。

互联网上的 IP 地址统一由一个叫 IANA（Internet Assigned Numbers Authority，互联网网络号分配机构）的组织来管理。由于分配不合理以及 IPv4 协议本身存在的局限，现在互联网的 IP 地址资源越来越紧张，为了解决这一问题，IANA 将A、B、C 类 IP 地址的一部分保留下来，留作局域网（LAN，Local Area Network）使用的 IP 地址空间，保留 IP 的范围如下表所示。

局域网使用的IP地址范围
网络类别	IP 地址范围	           网络数
A 类网	10.0.0.0~10.255.255.255	     1
B 类网	172.16.0.0~172.31.255.255	16
C 类网	192.168.0.0~192.168.255.255	255
保留的IP地址段不会在互联网上使用，因此与广域网相连的路由器在处理保留 IP 地址时，只是将该数据包丢弃处理，而不会路由到广域网上去，从而将保留 IP 地址产生的数据隔离在局域网内部。

在局域网内计算机数量少于 254 台的情况下，一般在 C 类 IP 地址段里选择 IP 地址范围就可以了，如从“192.168.1.1”到“192.168.255. 254”。



# IP地址块222.125.80.128/26包含的可用主机数是多少，最小的地址是多少，最大的地址是多少？
 

IP/26 是CIDR的格式，全称是classless inter domain route 叫做无类域间路由，就是说32位IP的前26位为网络号，后面的全部都可以分给主机： 

222.125.80.128------------1101 1110.0111 1101.0101 0000.10（00 0000）

1101 1110.0111 1101.0101 0000.10  总共26位为网络号
括号里面是主机位，总共六位。 2^6=64
再除掉222.125.80.128即主机位全零地址，和222.125.80.191即广播地址外
还剩64-2=62个主机地址。
其中最小地址主机位00 0001即222.125.80.129
最大地址主机位11 1110即222.125.80.190


### 子网掩码

子网掩码——屏蔽一个IP地址的网络部分的“全1”比特模式。对于A类地址来说，默认的子网掩码是255.0.0.0；对于B类地址来说默认的子网掩码是255.255.0.0；对于C类地址来说默认的子网掩码是255.255.255.0。

利用子网掩码可以把大的网络划分成子网，即VLSM（可变长子网掩码），也可以把小的网络归并成大的网络即超网。


作用：
子网掩码是一个32位地址，是与IP地址结合使用的一种技术。它的主要作用有两个，一是用于屏蔽IP地址的一部分以区别网络标识和主机标识，并说明该IP地址是在局域网上，还是在远程网上。二是用于将一个大的IP网络划分为若干小的子网络。

使用子网是为了减少IP的浪费。因为随着互联网的发展，越来越多的网络产生，有的网络多则几百台，有的只有区区几台，这样就浪费了很多IP地址，所以要划分子网。使用子网可以提高网络应用的效率。
通过IP 地址的二进制与子网掩码的二进制进行与运算，确定某个设备的网络地址和主机号，也就是说通过子网掩码分辨一个网络的网络部分和主机部分。子网掩码一旦设置，网络地址和主机地址就固定了。子网一个最显著的特征就是具有子网掩码。与IP地址相同，子网掩码的长度也是32位，也可以使用十进制的形式。例如，为二进制形式的子网掩码：1111 1111.1111 1111.1111 1111.0000 0000，采用十进制的形式为：255.255.255.0。

通过计算机的子网掩码判断两台计算机是否属于同一网段的方法是，将计算机十进制的IP地址和子网掩码转换为二进制的形式，然后进行二进制“与”(AND)计算（全1则得1，不全1则得0），如果得出的结果是相同的，那么这两台计算机就属于同一网段。


### IP

常见的IP地址，分为IPv4与IPv6两大类。

IPV4采用32位地址长度，有4段数字，每一段最大不超过255，只有大约43亿个地址，随着互联网的迅速发展，IPv4定义的有限地址空间将被耗尽。

而IPv6采用128位地址长度，几乎可以不受限制地提供地址。现有的互联网是在IPv4协议的基础上运行的，IPv6是下一版本的互联网协议，暂时还未推广使用。


IP地址分为两个部分： 网络号（网络地址） 和 主机号（主机地址），
网络号是说明该计算所属于的网络，主机号是说明该计算机在网络中的位置.


网络地址的表示法：主机位全为0，如IP 120.1.1.1，它的网络地址就是 120.0.0.0 。

备注：

IP地址=网络地址+主机地址 这是通常的地址格式，

如果涉及到子网划分，那么地址格式应该是：

IP地址 = 主机地址+子网地址+主机地址。。

###　特殊的IP地址

    就是我们主机不可以配置的IP
1、环路测试地址： （十进制表示情况下，以127开头的地址）

127.0.0.1-127.255.255.255 。 用来确定主机的TCP/IP协议是否正常，可以使用ping该地址段的任何一个IP。



2、全球广播地址：（二进制表示情况下，每一位都是1的地址）

255.255.255.255 ，这是IP地址中的每一位都为1的IP地址。如果有一台主机的发送数据时目标地址是255.255.255.255，那么所有的机子都将收到它所发出的信息。
3、全球网络地址：（二进制表示情况下，每一位都是0的地址）

0.0.0.0 代表任何的网络，可以说他是全球最大的网络，即任意网络。

### 以下列出留用的内部私有地址

A类 10.0.0.0--10.255.255.255
B类 172.16.0.0--172.31.255.255
C类 192.168.0.0--192.168.255.255


### IP地址分类

IP地址分为五类：

·A类用于大型网络（能容纳网络126个，主机1677214台），A类保留给政府机构。
·B类用于中型网络（能容纳网络16384个，主机65534台），B类分配给中等规模的公司。
·C类用于小型网络（能容纳网络2097152个，主机254台），C类分配给任何需要的人。
·D类用于组播（多目的地址的发送）
·E类用于实验。

备注：以上“能容纳网络”的意思就是“所能够分配的网络号”。
   示例：“能容纳网络16384个，每个网络下主机65534台”这句话的意思就是
“该类IP地址最多能分配的网络号有16384个，而每一个网络号下，又可以分配65534个主机号”

A、B、C三类IP地址的特征：

当将IP地址写成二进制形式时，A类地址的第一位总是 0 ，B类地址的前两位总是 10，C类地址的前三位总是 110。



A类地址
（1） A类IP地址。由1个字节的网络地址和3个字节的主机地址，网络地址的最高位必须是“0”。

如：0XXXXXXX.XXXXXXXX.XXXXXXXX.XXXXXXXX（X代表0或1）
（2）A类IP地址范围：1.0.0.1---126.255.255.254
（3）A类IP地址中的私有地址和保留地址：
① 10.X.X.X是私有地址（所谓的私有地址就是在互联网上不使用，而被用在局域网络中的地址）。
  范围（10.0.0.1---10.255.255.254）
② 127.X.X.X是保留地址，用做循环测试用的。也就是上述说到的特殊IP的第1种。



详解：


一个A类IP地址仅使用第一个8位位组表示网络地址。剩下的3个8位位组表示主机地址。A类地址的第一个位总为0，这一点在数学上限制了A类地址的范围小于127,127是64+32+16+8+4+2+1的和。最左边位表示128，在这里空缺。因此仅有127个可能的A类网络。A类地址后面的24位(3个点-十进制数)表示可能的主机地址，A类网络地址的范围从1.0.0.0到126.0.0.0。注意只有第一个8位位组表示网络地址，剩余的3个8位位组用于表示第一个8位位组所表示网络中惟一的主机地址，当用于描述网络时这些位置为0。注意技术上讲，127.0.0.0 也是一个A类地址，但是它已被保留作闭环（look back ）测试之用而不能分配给一个网络。每一个A类地址能支持16777214个不同的主机地址，这个数是由2的24次方再减去2得到的。减2是必要的，因为IP把全0保留为表示网络而全1表示网络内的广播地址。其中10.0.0.0 到10.255.255.255保留。



A类范围记忆：

第一字节最小为1（0开头的是特殊IP，表示任意网络，不能用） ，最大为126，本来应是255-128=127 ，但127闭环（look back）测试之用 ，所以A类网络地址的范围从1.0.0.0到126.0.0.0。



B类地址
（1） B类地址第1字节和第2字节为网络地址，其它2个字节为主机地址。它的第1个字节的前两位固定为10.
（2） B类地址网络号范围：128.0.0.0---191.255.0.0。
（3） B类地址的私有地址和保留地址
① 172.16.0.0---172.31.255.255是私有地址
② 169.254.X.X是保留地址。如果你的IP地址是自动获取IP地址，而你在网络上又没有找到可用的DHCP服务器。就会得到其中一个IP。
191.255.255.255是广播地址，不能分配。


详解：


设计B类地址的目的是支持中到大型的网络。B类网络地址范围从128.1.0.0到191.254.0.0。B 类地址蕴含的数学逻辑是相当简单的。一个B类IP地址使用两个8位位组表示网络号，另外两个8位位组表示主机号。B类地址的第1个8位位组的前两位总置为10，剩下的6位既可以是0也可以是1，这样就限制其范围小于等于191，由128+32+16+8+4+2+1得到。最后的16位( 2个8位位组)标识可能的主机地址。每一个B类地址能支持65534 个惟一的主机地址，这个数由2的16次方减2得到。B类网络仅有16382个，其中172.16.0.0到172.31.255.255保留。



B类范围记忆：


第一字节最小是128，最大是255-64=191 。B类网络地址的范围从128.1.0.0到191.254.0.0。 最大本来应该是191.255.0.0，但是191.255.0.0是该网段的广播地址，所以不算。


C类地址
（1）    C类IP地址。由3个字节的网络地址和1个字节的主机地址，网络地址的最高位必须是“110”。

如：110XXXXX.XXXXXXXX.XXXXXXXX.XXXXXXXX（X代表0或1）

（2）C类IP地址范围：192.0.0.1---223.255.255.254。

（3）C类地址中的私有地址：

        192.168.X.X是私有地址。（192.168.0.1---192.168.255.255)


详解：

C类地址用于支持大量的小型网络。这类地址可以认为与A类地址正好相反。A类地址使用第一个8位位组表示网络号，剩下的3个表示主机号，而C类地址使用三个8位位组表示网络地址，仅用一个8位位组表示主机号。C类地址的前3位数为110，前两位和为192(128+64)，这形成了C类地址空间的下界。第三位等于十进制数32,这一位为0限制了地址空间的上界。不能使用第三位限制了此8位位组的最大值为255-32等于223。因此C类网络地址范围从192.0.1.0 至223.255.254.0。最后一个8位位组用于主机寻址。每一个C类地址理论上可支持最大256个主机地址(0～255)，但是仅有254个可用，因为0和255不是有效的主机地址。可以有2097150个不同的C类网络地址，其中192.168.0.0到192.168.255.255保留。



c类范围记忆：


第一字节最小是128+64=192，最大是255-32=223 。C类网络地址的范围从192.0.1.0 至223.255.254.0。最大本来应该是223.255.255.0，但是223.255.255.0是该网段的广播地址，所以不算。


D类地址
（1） D类地址不分网络地址和主机地址，它的第1个字节的前四位固定为1110。

如：1110XXXX.XXXXXXXX.XXXXXXXX.XXXXXXXX（X代表0或1）
（2） D类地址范围：224.0.0.1---239.255.255.254


详解：


 D类地址用于在IP网络中的组播( multicasting ，又称为多目广播)。D类地址的前4位恒为1110 ，预置前3位为1意味着D类地址开始于128+64+32等于224。第4位为0意味着D类地址的最大值为128+64+32+8+4+2+1为239，因此D类地址空间的范围从224.0.0.0到239.255. 255.254。



D类范围记忆：

第一字节最小是128+64+32=224，最大是255-16=239 。所以D类网络地址的范围从224.0.0.0到239. 255. 255.254。最大本来应该是239. 255. 255.255，但是239. 255. 255.255是该网段的广播地址，所以不算。



E类地址
（1） E类地址不分网络地址和主机地址，它的第1个字节的前四位固定为 1111。

如：1111XXXX.XXXXXXXX.XXXXXXXX.XXXXXXXX（X代表0或1）

（2） E类地址范围：240.0.0.1---255.255.255.254



详解：

E类地址保留作研究之用。因此Internet上没有可用的E类地址。E类地址的前4位恒为1，因此有效的地址范围从240.0.0.0至255.255.255.254。 总的来说，ip地址分类由第一个八位组的值来确定。任何一个0到127 间的网络地址均是一个A类地址。任何一个128到191间的网络地址是一个B类地址。任何一个192到223 间的网络地址是一个C类地址。任何一个第一个八位组在224到239 间的网络地址是一个组播地址即D类地址。E类保留。


E类范围记忆：

第一字节最小是128+64+32+16=240，最大是255 。所以E类网络地址的范围从240.0.0.0到255.255.255.254。



备注：

前面提到“该网段的广播地址”有什么用，就是说如果往这个广播地址上发送数据，

那么IP属于该网络地址的所有主机都能接收到消息。

子网掩码：
https://blog.csdn.net/jason314/article/details/5447743


### CIDR

CIDR采用各种长度的"网络前缀"来代替分类地址中的网络号和子网号，其格式为：IP地址 = {<网络前缀>,<主机号>}。为了区分网络前缀，通常采用"斜线记法"（CIDR记法），即IP地址/网络前缀所占比特数。例如：192.168.24.0/22 表示32位的地址中，前22位为网络前缀，后10(32-22=10)位代表主机号。在换算中，192.168.24.0/22 对应的二进制为：

1100 0000(192)，1010 1000(168)，0001 1000(24)，0000 0000(0)

                    

其中红色为主机号，总共有10位。当这10位全为0时，取最小地址192.168.24.0，当这10位全为1时，取最大地址192.168.27.255。但请注意，在实际中，主机号全为0或者全为1的地址一般不使用，作为预留地址另有作用。所以第一个地址为1100 0000，1010 10000，0001 1000，0000 0001，即192.168.24.1 最后一个地址为：1100 0000，1010 10000，0001 1011，1111 1110，即192.168.27.254

 

因此，本例中将第三段地址数据中最小是00011000（24），最大是00011011（27），第四段地址数据中最小为0000 0001（1），最大为1111 1110（254），以上括号中数据为十进制，其前面为二进制。所以本例中192.168.24.0/22 对应地址段为192.168.24.1-192.168.27.254，共4个网段。


### IP查询工具

http://ipblock.chacuo.net/


### py大法

发现python居然有现成的类库支持此类网络操作与换算。 将以上的需求翻译成python只要几行代码搞定：

# 确定起始和结尾IP，无论多复杂都可以转换
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

链接：https://segmentfault.com/a/1190000007493686