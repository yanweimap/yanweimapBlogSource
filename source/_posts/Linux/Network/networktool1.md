---
title: 网络常用工具命令(1)
description: traceroute netstat ss命令
toc: true
tag: 网络命令
---
# traceroute

## traceroute介绍
traceroute (Windows系统下是tracert) 命令利用ICMP 协议定位您的计算机和目标计算机之间的所有路由器。TTL值可以反映数据包经过的路由器或网关的数量，通过操纵独立ICMP呼叫报文的TTL值和观察该报文被抛弃的返回信息，traceroute命令能够遍历到数据包传输路径上的所有路由器。traceroute是一条缓慢的命令，因为每经过一台路由器都要花去大约10到15秒。
```
$ traceroute 10.1.6.32
traceroute to 10.1.6.32 (10.1.6.32), 30 hops max, 60 byte packets
1  10.1.152.254 (10.1.152.254)  4.318 ms  4.603 ms  4.878 ms
2  10.1.6.32 (10.1.6.32)  0.257 ms  0.219 ms  0.211 ms
```
## traceroute工作原理
其详细过程如下：  
将传递到目的IP地址的ICMP Echo消息的TTL值被设置为1，该消息报经过第一个路由器时，其TTL值减去1，此时新产生的TTL值为0。  
由于TTL值被设置为0，路由器判断此时不应该尝试继续转发数据报，而是直接抛弃该数据报。由于数据报的生存周期（TTL值）已经到期，这个路由器会发送一个一个ICMP时间超时，即TTL值过期信息返回到客户端计算机。  
此时，发出traceroute命令的客户端计算机将显示该路由器的名称，之后可以再发送一个ICMP Echo消息并把TTL值设置为2。  
第1个路由器仍然对这个TTL值减1，然后，如果可能的话，将这个数据报转发到传输路径上的下一跳。当数据报抵达第2个路由器，TTL值会再被减去1，成为0值。  
第2个路由器会像第1个路由器一样，抛弃这个数据包，并像第1个路由器那样返回一个ICMP消息。  
该过程会一直持续，traceroute命令不停递增TTL值，而传输路径上的路由器不断递减该值，直到数据报最终抵达预期的目的地。  
当目的计算机接收到ICMP Echo消息时，会回传一个ICMP Echo Reply消息。  
## traceroute常见用法
traceroute的用法为:   
traceroute [ -46dFITnreAUDV ] [ -f first_ttl ] [ -g gate,... ] [ -i device ] [ -m max_ttl ] [ -N squeries ] [ -p port ] [ -t tos ] [ -l flow_label ] [ -w MAX,HERE,NEAR ] [ -q nqueries ] [ -s src_addr ] [ -z sendwait ] [ --fwmark=num ] host [ packetlen ]

常用options参数:  
-4: 使用IPv4  
-6: 使用IPv6  
-d: 启用套接字级调试  
-F: 不分段数据包  
-f: 从first_ttl hop开始（而不是从1开始）  
-g: 通过指定的网关路由数据包 (IPv4最多8个，IPv6最多127个）  
-I: 使用ICMP ECHO进行跟踪路由  
-T: 使用TCP SYN进行跟踪路由（默认端口为80）  
-i: 指定要使用的网络接口  
-m: 设置最大跳数（最大TTL为到达）。默认值为30  
-N: 设置要同时尝试的探测器数量（默认值为16）  
-n: 不解析其域名的IP地址
-p: 设置要使用的目标端口。它是“default”方法的初始udp端口值（按每个探测增加，默认为33434），或“icmp”的初始seq（也增加了，默认来自1），或一些不变的目的地其他方法的端口（默认值为80）“tcp”，53表示“udp”等）  
-t: 设置TOS（IPv4服务类型）或TC（IPv6流量包的值)  
-l: 将指定的flow_label用于IPv6数据包  
-W: 等待探测不超过此（默认3） 时间长于同一跳的响应时间，或者不超过一些（默认10次）下一跳，或MAX（默认5.0）秒（浮点数也允许点值）
-q: 设置每跳的探测次数。默认是3  
更多参数和完整解释见man traceroute  

# netstat
## netstat介绍
netstat列出系统上所有的网络套接字连接情况，包括 tcp, udp 以及 unix 套接字，另外它还能列出处于监听状态（即等待接入请求）的套接字。如果你想确认系统上的 Web 服务有没有起来，你可以查看80端口有没有打开。以上功能使 netstat 成为网管和系统管理员的必备利器。  
## netstat常见用法
1. -a 显示所有监听或非监听socket网络链接
```
$ netstat -a
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 0.0.0.0:netbios-ssn     0.0.0.0:*               LISTEN     
tcp        0      0 localhost:5037          0.0.0.0:*               LISTEN     
tcp        0      0 localhost:domain        0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN     
tcp        0      0 localhost:ipp           0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:microsoft-ds    0.0.0.0:*               LISTEN     
tcp        0      1 NSGWD180027:44946       10.1.6.6:domain         SYN_SENT   
tcp        0      0 NSGWD180027:60634       113.96.233.144:http     CLOSE_WAIT
tcp        0      0 NSGWD180027:45626       47.95.47.253:https      ESTABLISHED
...
```
2. -r 显示IP路由表(跟**route**指令显示内容一样)
```
$ netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         _gateway        0.0.0.0         UG        0 0          0 enp1s0
10.1.152.0      0.0.0.0         255.255.255.0   U         0 0          0 enp1s0
link-local      0.0.0.0         255.255.0.0     U         0 0          0 enp1s0
```
3. -i 显示网络接口(查看网络接口相关建议使用**ifconfig**)
```
$ netstat -i
Kernel Interface table
Iface      MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
enp1s0    1500  6400427      0      0 0       2132187      0      0      0 BMRU
lo       65536   532597      0      0 0        532597      0      0      0 LRU
```
4. -s 显示网络统计信息
```
$ netstat -s
Ip:
    Forwarding: 2
    4803573 total packets received
    9283 with invalid addresses
    0 forwarded
    0 incoming packets discarded
    4665596 incoming packets delivered
    2659745 requests sent out
    2 outgoing packets dropped
    511 dropped because of missing route
    ...
```
5. -n 不解析名称,直接显示IP地址()
```
netstat -nv
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 10.1.152.229:43548      203.208.41.47:443       ESTABLISHED
tcp        0      1 10.1.152.229:52668      10.128.161.27:445       SYN_SENT   
tcp        0      0 10.1.152.229:56046      96.43.145.26:443        ESTABLISHED
tcp        0      1 10.1.152.229:52584      10.128.161.27:445       SYN_SENT   
tcp        0      1 10.1.152.229:54060      216.58.200.14:443       SYN_SENT   
tcp        0      1 10.1.152.229:60748      10.128.161.27:139       SYN_SENT   
```

6. -p 显示套接字的pid程序名称
```
$ netstat -np |head -n 10
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 10.1.152.229:37044      113.96.230.37:443       ESTABLISHED -                   
tcp        0      1 10.1.152.229:42914      216.58.199.14:443       SYN_SENT    29400/firefox       
tcp        0      1 10.1.152.229:42916      216.58.199.14:443       SYN_SENT    29400/firefox
```
7. -l 仅显示处于监听状态的网络连接
8. -t/-u 显示tcp/udp连接
9. -npe 同时显示用户和进程信息
```
$netstat -nep
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode      PID/Program name
tcp        0      1 10.1.152.229:51524      172.217.24.46:443       SYN_SENT    1000       3498922041 29400/firefox       
tcp        0      0 10.1.152.229:35182      54.149.188.75:443       ESTABLISHED 1000       19360947   29400/firefox       
tcp        0      1 10.1.152.229:51526      172.217.24.46:443       SYN_SENT    1000       3498922042 29400/firefox       
```
10. 如果需要过滤某个服务或用户,可以结合grep或watch指令使用

# ss
## ss介绍
ss-socket statistics
类似netstat,统计socket信息,ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比netstat更快速更高效  
当服务器的socket连接数量变得非常大时，无论是使用netstat命令还是直接cat /proc/net/tcp，执行速度都会很慢。  
ss快的秘诀在于，它利用到了TCP协议栈中tcp_diag。tcp_diag是一个用于分析统计的模块，可以获得Linux 内核中第一手的信息，这就确保了ss的快捷高效。   
## ss常见用法
ss的参数类似netstat:  
1. -a 显示所有socket
2. -r 解析主机名(默认不解析)
3. -m 显示socket memory使用情况
```
ss -am |head -n 10
NetidState      Recv-Q Send-Q                                     Local Address:Port                                         Peer Address:Port                                                                                                  
nl   UNCONN     0      0                                                   rtnl:avahi-daemon/6836                                        *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
nl   UNCONN     0      0                                                   rtnl:-2034236502                                              *                      	 skmem:(r0,rb425984,t0,tb65536,f0,w0,o0,bl0,d0)                                
nl   UNCONN     0      0                                                   rtnl:systemd-resolve/860                                      *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
nl   UNCONN     0      0                                                   rtnl:packagekitd/1229                                         *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
nl   UNCONN     0      0                                                   rtnl:goa-daemon/3578                                          *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
nl   UNCONN     0      0                                                   rtnl:kernel                                                   *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
nl   UNCONN     0      0                                                   rtnl:firefox/29400                                            *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
nl   UNCONN     0      0                                                   rtnl:chrome/22570                                             *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
nl   UNCONN     0      0                                                   rtnl:whoopsie/1333                                            *                      	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)                               
```
4. dst/src 匹配目的/源地址socket
```
$ss dst 10.1.6.11
Netid              State                Recv-Q                Send-Q                                Local Address:Port                                Peer Address:Port                       
tcp                ESTAB                20                    0                                      10.1.152.229:60804                                  10.1.6.11:microsoft-ds
$ ss src 10.1.152.229
Netid             State                    Recv-Q               Send-Q                              Local Address:Port                                 Peer Address:Port                      
tcp               ESTAB                    0                    0                                    10.1.152.229:36916                                   10.1.6.26:microsoft-ds              
tcp               SYN-SENT                 0                    1                                    10.1.152.229:34324                              172.217.24.206:https                     
tcp               SYN-SENT                 0                    1                                    10.1.152.229:34388                              172.217.24.206:https
```

其他一些详细的参数请查询man ss

参考:  
1. [traceroute命令初探](https://www.cnblogs.com/beyond_dxb/p/8383821.html)  
2. [netstat 的10个基本用法](https://blog.csdn.net/u010739551/article/details/80736032)  
3. [netstat 命令详解](https://www.cnblogs.com/xieshengsen/p/6618993.html)
4. [Linux ss命令详解](https://www.cnblogs.com/ftl1012/p/ss.html)
