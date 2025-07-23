---
{"dg-publish":true,"permalink":"/tryhackme/tryhatryhackme & hackthebox Tcpdump小介绍/","title":"tryhackme & hackthebox Tcpdump小介绍","tags":["linux","#tryhackme"]}
---



# tryhackme的tcpdump介绍
来看看tcpdump这个工具 Tcpdump工具及其libpcap库是用 C 和 C++ 编写的

tcpdump的作用
- 捕获数据包并保存到文件
- 对捕获的数据包设置过滤器
- 控制捕获的数据包的显示方式

| 命令                     | 解释                          |
| ---------------------- | --------------------------- |
| `tcpdump -i INTERFACE` | 捕获特定网络接口上的数据包               |
| `tcpdump -w FILE`      | 将捕获的数据包写入文件                 |
| `tcpdump -r FILE`      | 从文件读取捕获的数据包                 |
| `tcpdump -c COUNT`     | 捕获特定数量的数据包                  |
| `tcpdump -n`           | 不解析 IP 地址                   |
| `tcpdump -nn`          | 不解析 IP 地址，也不解析协议号           |
| `tcpdump -v`           | `-vv`详细显示；详细程度可以通过和增加`-vvv` |

![task2](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/task2.png)

答案就是-n

## 过滤表达式

按主机过滤

1. 指定主机
- host
```
tcpdump -i eth0 host 192.168.10.1
```

2. 指定源地址和目标地址
- src dst
```
tcpdump -i eth0 src host 192.168.10.1

tcpdump -i eth0 dst host 192.168.10.1
```

3. 指定端口
- port
```
tcpdump -i eth0 port 80
```

4. 指定协议
- tcpdump 协议名
```
tcpdump -i eth0 arp
#捕获所有arp协议的数据包

tcpdump -i eth0 icmp
#捕获所有icmp协议的数据包

tcpdump -i eth0 tcp
#捕获所有tcp协议的数据包
```

tcpdump -i any tcp port 22
监听所有接口并捕获tcp往返的数据包port 22，

tcpdump -i wlo1 udp port 123
监听 WiFi 网卡并过滤发往网络时间协议 ( NTPudp ) 的流量。port 123

tcpdump -i eth0 host example.com and tcp port 443 -w https.pcap
将会监听eth0有线以太网接口 ，并过滤与example.com使用tcp和交换的流量port 443。换句话说，此命令正在过滤与相关的 HTTPS 流量example.com。

## 高级表达式
三种计算符 &、|和!(与，或，非)

可以使用tcp[tcpflags]来引用TCP标志字段。以下TCP标志可供比较：

tcp-syn TCP SYN（同步）
tcp-ack TCP ACK（确认）
tcp-fin TCP FIN（完成）
tcp-rst TCP RST（重置）
tcp-push TCP推送
基于上述内容，我们可以写出：

tcpdump "tcp[tcpflags] == tcp-syn"捕获仅设置了 SYN（同步）标志但所有其他标志均未设置的TCP数据包。

tcpdump "tcp[tcpflags] & tcp-syn != 0"捕获至少设置了 SYN（同步）标志的TCP数据包。

tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"捕获至少设置了SYN（同步）或ACK（确认）标志的TCP数据包。

看一下第四部分的题目

![task4](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/task4.png)

第一问 

这个就是参数直接套过去就行

```
tcpdump -r traffic.pcap "tcp[flags]==tcp-rst"
```

![task4](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/rst.png)


第二问
如果不加-n的话 不会显示主机的ip ip会被解析成主机名 加上-n参数可以直接显示数字格式的 IP 地址和端口号，避免了这些额外的解析过程。

```
tcpdump -r traffic.pcap greater 15000  -n
```

![task4](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/15000.png)

## 打印
- -q：快速输出；打印简要的数据包信息
- -e：打印链接级标题
- -A：以 ASCII 格式显示数据包数据
- -xx：以十六进制格式显示数据包数据，简称hex
- -X：以十六进制和 ASCII 显示数据包头和数据

来看第五部分的问题

![task5](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/task5.png)

加个-e参数就能查出mac地址

```
tcpdump -r traffic.pcap -e arp
```
![task5](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/arpmac.png)


# hackthebox的tcpdump补充
话题一转 来看看hackthebox的tcpdump有啥补充吗

其实讲的也和thm的差不多 简单扩充一下知识点


## 第一部分
看看题目

![htb](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/task1.png)

第一题需要下载个附件 就一张截图 仔细看开头的话 就是一个完整的tcp三次握手流程

-  [S] 表示客户端发送的 SYN 包。
-  [S.] 表示服务器发送的 SYN-ACK 包。
-  [.] 是客户端发送的 ACK 包，完成三次握手。

![task1](/images/tryhackme-Tcpdump-The-Basics小介绍/htb/question-1.PNG)

能看到ip192的给ip为174.143.213.184的服务器建立连接

服务器ip为174.143.213.184

捕获过程是相对序号

**相对序号** 是一种在抓包过程中为了**简化数据包分析**而使用的序号模式。它将TCP连接中的初始序列号（ISN）归零，从而简化了整个连接中的包的序列号计算。也就是说，所有包的序列号从 0 开始递增，不再显示真实的网络中传输的绝对序列号。

第三题

```
 -rvXc 100
```

![htb](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/task1.1.png)

第四题
```
sudo tcpdump -Xr /tmp/capture.pcap
```

第五题 是-v 还要更详细就-vv

第六题 man可以看更多信息

第七题 -W是写入文件

## 第二部分

![task2](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/task2.png)

第一问问的蛮好的 -l参数可以通过管道运输到另一个地方

过滤器确实可以这么做 true

想要某个协议的数据就直接后面加个协议名
不想要某个协议就协议名的前面加个not
例如题目问的：not icmp

问安装在哪可以用which

![task2](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/task2.1.png)

输出内容更加详细就 -v

-W 写入文件

-r 是从中读取捕获的内容

-X 以十六进制和ascii格式显示内容

## 第三部分

![task3](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/task3.png)

### 第一问
这个真的是好问题 琢磨半天了 需要下载他的附件pcap

一开始我也没怎么看这个包具体咋样 就是直接过滤去找第一个fin包 实际情况是找半天找不出

这个就是他的pcap包的内容 后面还有一大半

![task](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/port.png)

那只能老老实实慢慢看 

![task](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/port1.png)

在这里面，其实有三个端口发出来tcp的第一次握手分别是43804，43806，52520这三个端口 往下看 43804和52520都在最后收到了rst包 而43806成功三次握手

所以客户端端口应该是43806 而服务端则是http的80端口

### 第二问
第二问问的是dns服务器ip 那我们得过滤一下 加个port domain参数

过滤完之后 来检查一下记录

![dns](/img/user/images/tryhackme-Tcpdump-The-Basics小介绍/htb/dns1.png)

在这里面我们发现172.16.146.2的57752端口发出请求而下面172.6.46.1收到且回应了

在前面两个包有a？ aaaa?这种字样 

在 DNS 请求包中，通常会看到查询字段，比如 A?, AAAA?，表示正在请求域名解析。

所以172.6.46.1是dns服务器ip


# 结尾
感觉还是很遥远哦 才学了这么点东西 























































