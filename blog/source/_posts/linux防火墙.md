---
title: linux防火墙
date: 2018-01-19 21:27:52
tags:
---
#### <font color=#0088 face="">摘要
<font color=#0099ff face="">本章将分别使用iptables、firewall-cmd、firewall-config和TCP Wrappers等防火墙策略配置服务来完成数十个根据真实工作需求而设计的防火墙策略配置实验。

#### <font color=#0088 face="">防火墙种类


1. 内外网形式的防火墙，在外网和企业内网之间架设一台服务器或者路由器，配好过滤策略用来作为公司的防火墙。工作流程如图：
![](/assets/img/防火墙2.png)

2. 本地主机形式的防火墙：在本主机前面没有任何服务器对流量进行规则过滤，主机会收到所有的流量，这就需要在本地主机配置过滤规则，对所有的流量进行监控和过滤，即防火墙放在本地。
<!---more--->
#### <font color=#0088 face="">防火墙策略
防火墙策略可以基于流量的源目地址、端口号、协议、应用等信息来定制，然后防火墙使用预先定制的策略规则监控出入的流量，若流量与某一条策略规则相匹配，则执行相应的处理，反之则丢弃。这样一来，就可以保证仅有合法的流量在企业内网和外部公网之间流动了。说的更简单一点就是，先定义好监控策略，然后在有流量经过时会根据这些策略对流量进行过滤，保证主机的安全。

在RHEL 7系统中，firewalld防火墙取代了iptables防火墙。对于接触Linux系统比较早或学习过RHEL 6系统的读者来说，当他们发现曾经掌握的知识在RHEL 7中不再适用，需要全新学习firewalld时，难免会有抵触心理。<font color=#0099ff face="">其实，iptables与firewalld都不是真正的防火墙，它们都只是用来定义防火墙策略的防火墙管理工具而已，或者说，它们只是一种服务。iptables服务会把配置好的防火墙策略交由内核层面的netfilter网络过滤器来处理，而firewalld服务则是把配置好的防火墙策略交由内核层面的nftables包过滤框架来处理。</font>换句话说，当前在Linux系统中其实存在多个防火墙管理工具，旨在方便运维人员管理Linux系统中的防火墙策略，我们只需要配置妥当其中的一个就足够了。虽然这些工具各有优劣，但它们在防火墙策略的配置思路上是保持一致的。大家甚至可以不用完全掌握本章介绍的内容，只要在这多个防火墙管理工具中任选一款并将其学透，就足以满足日常的工作需求了。

CentOS7默认采用的是firewalld管理netfilter子系统，底层调用的仍然是iptables命令。不同的防火墙软件相互间存在冲突，使用某个时应禁用其他的。
![工作原理](/assets/img/过滤.png)
#### <font color=#0088 face="">Netfilter
netfilter是Linux 2.4内核引入的全新的包过滤引擎。由一些数据包过滤表组成，这些表包含内核用来控制信息包过滤的规则集。iptables等等都是在用户空间修改过滤表规则的便捷工具。

netfilter在数据包必须经过且可以读取规则的位置，共设有5个控制关卡。这5个关卡处的检查规则分别放在5个规则链中（有的叫钩子函数（hook functions）。也就是说5条链对应着数据包传输路径中的5个控制关卡，链中的规则会在对应的关卡检查和处理。任何一个数据包，只要经过本机，必然经过5个链中的某个或某几个。
~~~
在进行路由选择前处理数据包（PREROUTING）；

处理流入的数据包（INPUT）；

处理流出的数据包（OUTPUT）；

处理转发的数据包（FORWARD）；

在进行路由选择后处理数据包（POSTROUTING）。
~~~
#### <font color=#0088 face="">数据包的传输过程

1、当一个数据包进入网卡时，它首先进入PREROUTING链，内核根据数据包目的IP判断是否需要转送出去。

2、如果数据包就是进入本机的，它就会沿着图向下移动，到达INPUT链。数据包到了INPUT链后，任何进程都会收到它。本机上运行的程序可以发送数据包，这些数据包会经过OUTPUT链，然后到达POSTROUTING链输出。

3、如果数据包是要转发出去的，且内核允许转发，数据包就会如图所示向右移动，经过FORWARD链，然后到达POSTROUTING链输出。

#### <font color=#0088 face="">Iptables
起源于freeBSD的ipfirewall(内核1.x时代)，所有规则都在内核中；2.0x后更名为ipchains，可以定义多条规则并串用；现在叫iptables，使用表组织规则链。

iptablses按照用途和使用场合，将5条链各自切分到五张不同的表中。也就是说每张表中可以按需要单独为某些链配置规则。例如，mangle表和filter表中都能为INPUT链配置规则，当数据包流经INPUT位置（进入用户空间），这两个表中INPUT链的规则都会用来做过滤检查。
![](/assets/img/iptables表.jpg)

五张表，每张表侧重于不同的功能

* filter        数据包过滤功能。只涉及INPUT, FORWARD, OUTPUT三条链。是iptables命令默认操纵的表。
* nat          地址转换功能。NAT转换只涉及PREROUTING, OUTPUT, POSTOUTING三条链。可通过转发让局域网机器连接互联网
* mangle     数据包修改功能。每条链上都可以做修改操作。修改报文元数据，做防火墙标记等。
* raw          快速通道功能。为了提高效率，优先级最高，符合raw表规则的数据包会跳过一些检查。
* security    需要和selinux结合使用，内置规则比较复杂，通常都会被关闭

<font color=#0088 face="">基本的命令

  iptables是一款基于命令行的防火墙策略管理工具，具有大量参数，学习难度较大。好在对于日常的防火墙策略配置来讲，大家无需深入了解诸如“四表五链”的理论概念，只需要掌握常用的参数并做到灵活搭配即可，这就足以应对日常工作了。

  iptables命令可以根据流量的源地址、目的地址、传输协议、服务类型等信息进行匹配，一旦匹配成功，iptables就会根据策略规则所预设的动作来处理这些流量。另外，再次提醒一下，防火墙策略规则的匹配顺序是从上至下的，因此要把较为严格、优先级较高的策略规则放到前面，以免发生错误。下表总结归纳了常用的iptables命令参数。再次强调，我们无需死记硬背这些参数，只需借助下面的实验来理解掌握即可。
[](/assets/img/iptables.png)

一、<font color=#0088 face="">命令格式：
<font color=#0099ff face="">iptables [-t TABLE] COMMAND [CHAIN] [CRETIRIA]...  [-j  ACTION]

1、COMMAND 命令选项
```
-A|--append  CHAIN                                 //链尾添加新规则
-D|--delete  CHAIN [RULENUM]                       //删除链中规则，按需序号或内容确定要删除的规则
-I|--insert  CHAIN [RULENUM]                       //在链中插入一条新的规则，默认插在开头
-R|--replace CHAIN  RULENUM                        //替换、修改一条规则，按序号或内容确定
-L|--list   [CHAIN [RULENUM]]                      //列出指定链或所有链中指定规则或所有规则
-S|--list-urles [CHAIN [RULENUM]]                  //显示链中规则
-F|--flush [CHAIN]                                 //清空指定链或所有链中规则
-Z|--zero [CHAIN [RULENUM]]                        //重置指定链或所有链的计数器(匹配的数据包数和流量字节数)
-N|--new-chain CHAIN                               //新建自定义规则链
-X|--delete-cahin [CHAIN]                          //删除指定表中用户自定义的规则链
-E|--rename-chain OLDCHAIN NEWCHAIN                //重命名链，移动任何引用
-P|-policy CHAIN TARGET                            //设置链的默认策略，数据包未匹配任意一条规则就按此策略处理
```
2、CRETIRIA 条件匹配  

```
-p|--proto  PROTO                      //按协议匹配，如tcp、udp、icmp，all表示所有协议。 （/etc/protocols中的协议名）
-s|--source ADDRESS[/mask]...          //按数据包的源地址匹配，可使用IP地址、网络地址、主机名、域名
-d|--destination ADDRESS[/mask]...     //按目标地址匹配，可使用IP地址、网络地址、主机名、域名
-i|--in-interface INPUTNAME[ +]        //按入站接口(网卡)名匹配，+用于通配。如 eth0, eth+ 。一般用在INPUT和PREROUTING链
-o|--out-interface OUTPUTNAME[+]       //按出站接口(网卡)名匹配，+用于通配。如 eth0, eth+ 。一般用在OUTPUT和POSTROUTING链

可使用 ! 可以否定一个子句，如-p !tcp
```
3、 扩展匹配

-m|--match MATCHTYPE  EXTENSIONMATCH...    //扩展匹配，可能加载extension

如: -p tcp  -m tcp  --dport 80

隐式扩展匹配(对-p PROTO的扩展，或者说是-p PROTO的附加匹配条件)

<font color=#0099ff face="">-m PROTO 可以省略，所以叫隐式

```
-m tcp   //-p tcp的扩展
　　　　--sport  [!]N[:M]                      //源端口, 服务名、端口、端口范围。
　　　　--dport  [!]N[:M]                      //目标端口，服务名、端口、端口范围
　　　　--tcp-flags CHECKFLAGS FLAGSOFTRUE  //TCP标志位:SYN(同步),ACK(应答),RST(重置),FIN(结束),URG(紧急),PSH(强迫推送)。多个标志位逗号分隔。
　　　　　　　　　　　　　　　　　　　　　　　　　//CHECKFLAGS为要检查的标志位，FLAGSOFTRUE为必须为1的标志位（其余的应该为0）
　　　　--syn                               //第一次握手。 等效于 --tcpflags syn,ack,fin,rst syn   四个标志中只有syn为1
-m udp   //-p udp的扩展
　　　　--sport N[-M]
　　　　--dport N[-M]
-m icmp  //隐含条件为-p icmp
　　　　--icmp-type  N             //8:echo-request  0:echo-reply
```

<font color=#0099ff face="">显示扩展匹配

```
-m state
　　　　--state    //连接状态检测，NEW,ESTABLISHED,RELATED,INVALID
-m multiport
　　　　--source-ports   PORT[,PORT]...|N:M            //多个源端口，多个端口用逗号分隔，
　　　　--destination-ports PORT[,PORT]...|N:M         //多个目的端口
　　　　--ports     　　　　　　　　　　　　　　　　　　　　 //多个端口，每个包的源端口和目的端口相同才会匹配
-m limit
　　　　--limit   N/UNIT    //速率，如3/minute, 1/s, n/second , n/day
　　　　--limit-burst N     //峰值速率，如100，表示最大不能超过100个数据包
-m connlimit
　　　　--connlimit-above N  //多于n个，前面加!取反
-m iprange
　　　　--src-range IP-IP
　　　　--dst-range IP-IP
-m mac                    
　　　　--mac-source         //mac地址限制，不能用在OUTPUT和POSTROUTING规则链上，因为封包要送到网卡后，才能由网卡驱动程序透过ARP 通讯协议查出目的地的MAC 地址
-m string
　　　　--algo [bm|kmp]      //匹配算法
　　　　--string "PATTERN"   //匹配字符模式
-m recent
　　　　--name               //设定列表名称，默认为DEFAULT
　　　　--rsource            //源地址
　　　　--rdest              //目的地址
　　　　--set                //添加源地址的包到列表中
　　　　--update             //每次建立连接都更新列表
　　　　--rcheck             //检查地址是否在列表
　　　　--seconds            //指定时间。必须与--rcheck或--update配合使用
　　　　--hitcount           //命中次数。必须和--rcheck或--update配合使用
　　　　--remove             //在列表中删除地址
-m time
　　　　--timestart h:mm
　　　　--timestop  hh:mm
　　　　--days DAYS          //Mon,Tue,Wed,Thu,Fri,Sat,Sun; 逗号分隔
-m mark
　　　　--mark N            //是否包含标记号N
-m owner
　　　　--uid-owner 500   //用来匹配来自本机的封包，是否为某特定使用者所产生的,可以避免服务器使用root或其它身分将敏感数据传送出
　　　　--gid-owner O     //用来匹配来自本机的封包，是否为某特定使用者群组所产生的
　　　　--pid-owner 78    //用来匹配来自本机的封包，是否为某特定进程所产生的
　　　　--sid-owner 100   //用来匹配来自本机的封包，是否为某特定连接（Session ID）的响应封包

```
4、 ACTION 目标策略(TARGET)
~~~
-j|--jump TARGET                //跳转到目标规则，可能加载target extension
-g|--goto  CHAIN                //跳转到指定链，不再返回
ACCEPT             规则验证通过，不再检查当前链的后续规则，直接跳到下一个规则链。
DROP                直接丢弃数据包，不给任何回应。中断过滤。
REJECT             拒绝数据包通过，会返回响应信息。中断过滤。
--reject-with  tcp-reset|port-unreachable|echo-reply
LOG                  在/var/log/messages文件中记录日志，然后将数据包传递给下一条规则。详细位置可查看/etc/syslog.conf配置文件
--log-prefix "INPUT packets"
ULOG                更广范围的日志记录信息
QUEUE              防火墙将数据包移交到用户空间，通过一个内核模块把包交给本地用户程序。中断过滤。
RETURN            防火墙停止执行当前链中的后续规则，并返回到调用链。主要用在自定义链中。
custom_chain    转向自定义规则链
DNAT                目标地址转换，改变数据包的目标地址。外网访问内网资源，主要用在PREROUTING。完成后跳到下一个规则链
--to-destination ADDRESS[-ADDRESS][:PORT[-PORT]]
SNAT                源地址转换，改变数据包的源地址。内网访问外网资源。主机的IP地址必须是静态的，主要用在POSTROUTING。完成后跳到下一个规则链。
--to-source ADDRESS[-ADDRESS][:PORT[-PORT]]
MASQUERADE   源地址伪装，用于主机IP是ISP动态分配的情况，会从网卡读取主机IP。直接跳到下一个规则链。
--to-ports 1024-31000
REDIRECT        数据包重定向，主要是端口重定向，把包分流。处理完成后继续匹配其他规则。能会用这个功能来迫使站点上的所有Web流量都通过一个Web高速缓存，比如Squid。
--to-ports 8080
MARK                 打防火墙标记。继续匹配规则。
--set-mark 2
MIRROR           发送包之前交换IP源和目的地址，将数据包返回。中断过滤。
~~~  

二、 <font color=#0088 face="">实例

```
iptables -F           //删除iptables现有规则
iptables -L [-v[vv] -n]   //查看iptables规则
iptables -A INPUT -i eth0 -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT       //在INPUT链尾添加一条规则
iptables -I INPUT 2 -i eth0 -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT     //在INPUT链中插入为第2条规则
iptables -D  INPUT 2      //删除INPUT链中第2条规则
iptables -R INPUT 3 -i eth0 -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT    //替换修改第三条规则
iptables -P INPUT DROP    //设置INPUT链的默认策略为DROP
1、允许远程主机进行SSH连接
iptables -A INPUT -i eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
2、允许本地主机进行SSH连接
iptables -A OUTPUT -o eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A INTPUT -i eth0 -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
3、允许HTTP请求
iptables -A INPUT -i eth0 -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -o eth0 -p tcp --sport 80 -m state --state ESTABLISHED -j ACCEPT
4、限制ping 192.168.146.3主机的数据包数，平均2/s个，最多不能超过3个
iptables -A INPUT -i eth0 -d 192.168.146.3 -p icmp --icmp-type 8 -m limit --limit 2/second --limit-burst 3 -j ACCEPT
5、限制SSH连接速率（默认策略是DROP）
iptables -I INPUT 1 -p tcp --dport 22 -d 192.168.146.3 -m state --state ESTABLISHED -j ACCEPT  
iptables -I INPUT 2 -p tcp --dport 22 -d 192.168.146.3 -m limit --limit 2/minute --limit-burst 2 -m state --state NEW -j ACCEPT
6、防止syn攻击（限制syn的请求速度）
iptables -N syn-flood
iptables -A INPUT -p tcp --syn -j syn-flood
iptables -A syn-flood -m limit --limit 1/s --limit-burst 4 -j RETURN
iptables -A syn-flood -j DROP
7、防止syn攻击（限制单个ip的最大syn连接数）
iptables –A INPUT –i eth0 –p tcp --syn -m connlimit --connlimit-above 15 -j DROP
iptables -I INPUT -p tcp -dport 22 -m connlimit --connlimit-above 3 -j DROP   //利用recent模块抵御DOS攻击
iptables -I INPUT -p tcp --dport 22 -m state --state NEW -m recent --set --name SSH   //单个IP最多连接3个会话
Iptables -I INPUT -p tcp --dport 22 -m state NEW -m recent --update --seconds 300 --hitcount 3 --name SSH -j DROP  //只要是新的连接请求，就把它加入到SSH列表中。5分钟内你的尝试次数达到3次，就拒绝提供SSH列表中的这个IP服务。被限制5分钟后即可恢复访问。
iptables -I INPUT -p tcp --dport 80 -m connlimit --connlimit-above 30 -j DROP     
8、防止单个IP访问量过大
iptables –A OUTPUT –m state --state NEW –j DROP  //阻止反弹木马
iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/m -j ACCEPT   //防止ping攻击
9、只允许自己ping别人，不允许别人ping自己
iptables -A OUTPUT -p icmp --icmp-type 8 -j ACCEPT
iptables -A INPUT -p icmp --icmp-type 0 -j ACCEPT
10、对于127.0.0.1比较特殊，我们需要明确定义它
iptables -A INPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT
iptables -A OUTPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT
11、SNAT 基于原地址转换。许多内网用户通过一个外网 口上网的情况。将我们内网的地址转换为一个外网的IP，共用外网IP访问外网资源。
iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -j SNAT --to-source 172.16.100.1
12、当外网地址不是固定的时候。将外网地址换成 MASQUERADE(动态伪装):它可以实现自动读取外网网卡获取的IP地址。
iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -j MASQUERADE
13、DNAT 目标地址转换。目标地址转换要做在到达网卡之前进行转换,所以要做在PREROUTING这个位置上
iptables -t nat -A PREROUTING -d 192.168.10.18 -p tcp --dport 80 -j DNAT --to-destination 172.16.100.2

```

 #### <font color=#0088 face="">Firewalld
 dynamic firewall daemon。支持ipv4和ipv6。Centos7中默认将防火墙从iptables升级为了firewalld。firewalld相对于iptables主要的优点有：

 firewalld可以动态修改单条规则，而不需要像iptables那样，在修改了规则后必须得全部刷新才可以生效；
 firewalld在使用上要比iptables人性化很多，即使不明白“五张表五条链”而且对TCP/IP协议也不理解也可以实现大部分功能。　　

一.firewalld的主要概念

1、过滤规则集合：zone
