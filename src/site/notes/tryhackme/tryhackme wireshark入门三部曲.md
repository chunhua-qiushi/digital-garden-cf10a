---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme wireshark入门三部曲/","title":"tryhackme wireshark入门三部曲","tags":["渗透工具","#tryhackme"]}
---


wireshark 一个流量分析的工具 一个抓取流量包的工具
一个暗藏~~只因~~(玄机)的工具 只要擅长使用这个工具能带来很大的帮助 

简单看看tryhackme的wireshark教程 总共是分为三个部分

![title](/img/user/images/tryhackme-wireshark小tips/title.png)

# the basic

基础部分当然是介绍解释wireshark的操作界面和基础功能

先来看看初始界面

![chushi](/img/user/images/tryhackme-wireshark小tips/zhuomian.png)

从上往下分别是工具栏，显示过滤栏(用来过滤我们需要的东西)，最近文件，捕获过滤器和接口，状态栏

![in](/img/user/images/tryhackme-wireshark小tips/inzhuo.png)

这个界面就是一般我们平时使用的上面红色的部分就是数据包列表，中间的分别是数据包细节和数据包的字节，最下面是告诉我们有几个数据包 以及展示的数据包数目。

我们来看一下task2 讲了啥东西

## task 2
Wireshark 是目前最强大的流量分析工具之一。它有多种用途：

- 检测并排除网络问题，例如网络负载故障点和拥塞。
- 检测安全异常，例如恶意主机、异常端口使用和可疑流量。
- 调查和了解协议细节，例如响应代码和有效负载数据。

科普完毕 来看一下他要求

![task](/img/user/images/tryhackme-wireshark小tips/task2.png)

需要我们去查看文件信息 点击这个就可以查看文件信息 我们往下滑就能看到 全部题目的答案

![st](/img/user/images/tryhackme-wireshark小tips/st.png)

![st](/img/user/images/tryhackme-wireshark小tips/hash.png)

![st](/img/user/images/tryhackme-wireshark小tips/flag.png)


第一题的flag就在底下 

```
ans:TryHackMe_Wireshark_Demo
```

数据包的总数为58620个 

```
ans:57620
```

hash值也显而易见

```
f446de335565fb0b0ee5e5a3266703c778b2f3dfad7efeaeccb2da5641a6d6eb
```

## task3

![task](/img/user/images/tryhackme-wireshark小tips/task3.png)

查找数据包的编号先

![task](/img/user/images/tryhackme-wireshark小tips/finds.png)


![task](/img/user/images/tryhackme-wireshark小tips/38.png)

可以发现使用了xml语言

```
ans:eXtensible Markup Language
```

看时间的话 就得点进去慢慢找了 其实点进去就能发现到达时间

![time](/img/user/images/tryhackme-wireshark小tips/time.png)

```
ans:13/05/2004
```

ttl就是time to live 存活周期 是在osi的第三层 网络层那里 我们就到ip的那个部分找找

![time](/img/user/images/tryhackme-wireshark小tips/ttl.png)

```
ans:47
```

tcp的有效负载大小 就得在数据包细节找到tcp的部分 负载应该能理解成一端的大小

tcp是在第四层 传输层

![tcp](/img/user/images/tryhackme-wireshark小tips/tcp.png)

```
ans:424
```

etag也在这附近 在底下找找就有了

![tcp](/img/user/images/tryhackme-wireshark小tips/etag.png)

## task4

![task4](/img/user/images/tryhackme-wireshark小tips/task4.png)

查r4w这个字串 的第一个艺术家叫啥名字

![task4](/img/user/images/tryhackme-wireshark小tips/r4w.png)

![task4](/img/user/images/tryhackme-wireshark小tips/r4w1.png)

能看到第一个艺术家的名字 

```
ans:r4w8173
```

去编号是12的数据包那看评论是啥 这个就有意思了 

![task4](/img/user/images/tryhackme-wireshark小tips/packet1.png)

![task4](/img/user/images/tryhackme-wireshark小tips/packet2.png)

![task4](/img/user/images/tryhackme-wireshark小tips/12.png)

又让我去39765这个包里面找照片 一开始我是没想到这东西还能直接导出来 后面有一个note.txt也是一样 要我们导出来 cat一下就行 照片就不太一样 他说了要md5 我们用md5sum来把他提取出来

![task4](/img/user/images/tryhackme-wireshark小tips/img.png)

导出之后直接md5sum 加你保存的文件名

![task4](/img/user/images/tryhackme-wireshark小tips/MD5.png)

```
ans:911cd574a42865a956ccde2d04495ebf
```

需要找一个txt文件 先用过滤器去过滤一下 找到txt文件在哪

![task4](/img/user/images/tryhackme-wireshark小tips/note0.png)

然后我们导出http 把note.txt导出来

![task4](/img/user/images/tryhackme-wireshark小tips/note.png)

![task4](/img/user/images/tryhackme-wireshark小tips/note2.png)

保存之后 我们直接查看就能看到外星人名字

![task4](/img/user/images/tryhackme-wireshark小tips/name.png)

```
ans:packermaster
```

## task5

![task5](/img/user/images/tryhackme-wireshark小tips/task5.png)

第一题就是http

```
ans：http
```

第二题我们过滤了http之后 在底下会有显示展示了多少个数据包

![shujubao](/img/user/images/tryhackme-wireshark小tips/http.png)

下面显示1089个

```
ans:1089
```

第三题 去到33790的包中查看一下细节

![shujubao](/img/user/images/tryhackme-wireshark小tips/secend.png)

进http流里面搜索artists 就能看到艺术家名字 会发现艺术家共有三个 第二个叫做Blad3

![shujubao](/img/user/images/tryhackme-wireshark小tips/band.png)

```
ans:3

ans:Blad3
```

第一个部分 基础部分结束了 我们来看下一个部分

# Packet Operations
数据包操作 我们来看点小难的来练练 主要也是深入了解一点wireshark

这个地方是统计 可以看协议的数据包 主要是对数据包进行统计 可以在里面找到你想要的数据包

![qwe](/img/user/images/tryhackme-wireshark小tips/packet/ipvx.png)

## task2

![task2](/img/user/images/tryhackme-wireshark小tips/packet/task2.png)

第一题我们得在resolved addresses里面去找 因为是在找hostname是bbc的ip

![task2](/img/user/images/tryhackme-wireshark小tips/packet/bbc.png)

ip为199.232.24.81

第二题是在找ipv4的对话 那我们要在对话里面去找ipv4

![task2](/img/user/images/tryhackme-wireshark小tips/packet/ipv4.png)

共有435个对话

第三题 这次我们现在resolved addresses找一下micro-st的地址 然后去endpoint查询这个地址的字节

![task2](/img/user/images/tryhackme-wireshark小tips/packet/micro.png)

![task2](/img/user/images/tryhackme-wireshark小tips/packet/mac.png)

这两招下来就能找到字节大小 7474bytes

第三题依然是在endpoints里面找 找到city 筛选一下。能找到四个Kansas City有关的地址

![city](/img/user/images/tryhackme-wireshark小tips/packet/city.png)

第四题在endpoints 里面找组织 也是筛选一下 就能知道ip是188.246.82.7

![city](/img/user/images/tryhackme-wireshark小tips/packet/blicnet.png)

## task3

直接看题目要求

![task3](/img/user/images/tryhackme-wireshark小tips/packet/task3.png)

第一问 问最多用的ipv4的目的地址

![task3](/img/user/images/tryhackme-wireshark小tips/packet/destination.png)

![task3](/img/user/images/tryhackme-wireshark小tips/packet/destination1.png)

ip为10.100.1.33 我搞不懂为啥前面还多一个 看格式就是第二个 第一个不知道咋蹦出来的

第二问 我们还是在熟悉的地方 就是statistics 统计这 点开dns

![task3](/img/user/images/tryhackme-wireshark小tips/packet/ipvx.png)

在这找到services里的request-response time 能看到时间 0.467897

![task3](/img/user/images/tryhackme-wireshark小tips/packet/dns1.png)

第三rad.msn.com的http Requests 

![task3](/img/user/images/tryhackme-wireshark小tips/packet/http.png)

你会发现有两个rad.msn.com 直接相加就完事了

![task3](/img/user/images/tryhackme-wireshark小tips/packet/http1.png)

## task4
这个是科普环节 将怎么使用wireshark的过滤器

### 捕获过滤器
| 过滤规则                 | 说明                         |
| -------------------- | -------------------------- |
| `host 192.168.1.1`   | 仅捕获来自或发送到 192.168.1.1 的数据包 |
| `net 192.168.1.0/24` | 仅捕获 192.168.1.0/24 子网的数据包  |
| `port 80`            | 仅捕获端口 80（HTTP）的流量          |
| `tcp`                | 仅捕获 TCP 数据包                |
| `udp`                | 仅捕获 UDP 数据包                |
| `tcp port 443`       | 仅捕获 TCP 端口 443（HTTPS）的流量   |
| `src host 10.0.0.1`  | 仅捕获源 IP 为 10.0.0.1 的流量     |
| `dst port 53`        | 仅捕获目标端口为 53（DNS）的数据包       |


### 显示过滤器
| 过滤规则                                  | 说明                                      |
|-------------------------------------------|------------------------------------------|
| `ip.addr == 192.168.1.1`                  | 过滤与 192.168.1.1 相关的所有流量        |
| `ip.src == 10.0.0.1`                      | 仅显示源 IP 为 10.0.0.1 的数据包         |
| `ip.dst == 8.8.8.8`                       | 仅显示目标 IP 为 8.8.8.8 的数据包        |
| `tcp.port == 80`                          | 仅显示 TCP 端口 80 的流量                |
| `udp.port == 53`                          | 仅显示 UDP 端口 53（DNS）的流量          |
| `http`                                    | 仅显示 HTTP 流量                         |
| `dns`                                     | 仅显示 DNS 流量                          |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | 仅显示 TCP SYN 包（新建连接）           |
| `frame contains "password"`               | 仅显示数据包内容包含 "password" 的数据包 |
| `eth.addr == aa:bb:cc:dd:ee:ff`           | 仅显示特定 MAC 地址的流量                |

## task5

![task3](/img/user/images/tryhackme-wireshark小tips/packet/task5.png)

第一问 我们直接在过滤器里面输入ip就能知道 共81420个数据包

![ip](/img/user/images/tryhackme-wireshark小tips/packet/ip.png)

第二问 问我们ttl值小于10的数据包有几个 能查到有66个

![ttl](/img/user/images/tryhackme-wireshark小tips/packet/ttl.png)

```
ip.ttl<10
```

第三问 端口是4444的tcp数据包有几个

![4444](/img/user/images/tryhackme-wireshark小tips/packet/tcp4444.png)

有632个数据包

```
tcp.port ==4444
```

第四问 80端口的http get数据包有几个

![tcp 80](/img/user/images/tryhackme-wireshark小tips/packet/tcp 80.png)

共527个

```
http.request.method="GET" && tcp.port==80
```

第五问 dns a级查询

![tcp 80](/img/user/images/tryhackme-wireshark小tips/packet/dnsa.png)

用这个查询会发现 这个数字并不是题目要求

我们看一下显示过滤器

![tcp 80](/img/user/images/tryhackme-wireshark小tips/packet/dns2.png)

![tcp 80](/img/user/images/tryhackme-wireshark小tips/packet/dnsaa.png)

![tcp 80](/img/user/images/tryhackme-wireshark小tips/packet/dnsa.png)


## task6
![task6](/img/user/images/tryhackme-wireshark小tips/packet/task6.png)

ok 第一问找不是80端口的iis服务器的数据包 这里是两个条件 一个是iis服务器 一个是不能是80端口

![ask](/img/user/images/tryhackme-wireshark小tips/packet/port80 .png)

```
http.server contains "iis" && tcp.scrcport= 80 
```

就能筛选出来 共有21个

第二问 找到所有版本是7.5的iis服务器

![iis](/img/user/images/tryhackme-wireshark小tips/packet/7.5.png)

```
http.server contains "iis" && http.server contains "7.5" 
```

用这个命令就能筛选出来 共71个

第三问 3333，,4444，,9999端口共有几个数据包

![sidan](/img/user/images/tryhackme-wireshark小tips/packet/3333.png)

```
tcp.port in {3333 4444 9999}
```
共计2235个数据包

第四问
ttl为偶数的数据包个数

![ttl](/img/user/images/tryhackme-wireshark小tips/packet/ttl1.png)

```
strings(ip.ttl) matches "[02468]$"
```

过滤出来能发现有77289个数据包

第五问
这个要去改config文件 改完使用专家视图去查看

![config](/img/user/images/tryhackme-wireshark小tips/packet/config.png)

![config](/img/user/images/tryhackme-wireshark小tips/packet/config1.png)

改完之后就可以去专家视图那边去查看Bad TCP Checksum的个数

![config](/img/user/images/tryhackme-wireshark小tips/packet/config3.png)

![config](/img/user/images/tryhackme-wireshark小tips/packet/config4.png)

最后一问 
总算是轻松一点了 
用已经存在的过滤器来筛选数据包  轻轻松松 能发现261个  

![shujubao](/img/user/images/tryhackme-wireshark小tips/packet/5.png)

这两个wireshark就写到这里 收益还是很多的 在筛选器那里和导出文件 对wireshark的功能也熟悉了很多 至少不会像之前一样两眼懵逼的盯着屏幕
现在剩下最后一个问题了 流量分析！！！

## traffic Analysis
前面的看完 我们看看流量分析

这个是真有点小难度

## task2
![task2](/img/user/images/tryhackme-wireshark小tips/traffic/task2.png)

### 第一问
其实在这个文章中就有介绍怎么去过滤tcp connect的数据包

```
tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size <= 1024
```

- syn == 1就是用来确立连接的请求报文  
- ack==0就是三次握手的第一步 
- tcp窗口的大小字段小于等于1024

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/tcp connect.png)

由图就能看出总数为1000个

### 第二问
![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/tcpconnect.png)

```
tcp.port == 80
```

第一个包会发现是tcp connect的数据包

### 第三问

![ask3](/img/user/images/tryhackme-wireshark小tips/traffic/udpguabi.png)

```
icmp.type == 3 && icmp.code == 3
```

icmp.type == 3意思就是目的地不可达

icmp.code == 3意思就是端口不可达

这样就能查出有1083个关闭的端口

## 第四问

![ask4](/img/user/images/tryhackme-wireshark小tips/traffic/udp1.png)

```
udp.port in {55..70}
```

我们过滤出来之后分析一下这个数据包走向 前面67和69端口的数据包后面都显示没到达目的地 而68端口则显示成功 数据包一直在传输。说明68端口开放

## task3

![task3](/img/user/images/tryhackme-wireshark小tips/traffic/task3.png)

### 第一问
![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/284.png)

我们仔细观察就能发现攻击者的物理地址 就图片蓝色标的

```
(arp.opcode== 1) && (eth.src == 00:0c:29:e2:18:b4)
```

arp.opcode == 1 就是过滤arp请求包

其实单点一下你要的选项 把他拖到搜索栏那里就能自动输入 没有这么麻烦 就这个问题举例子 点到源地址这个选项 单击拖到搜索栏 选择and selected 就能自动输入

### 第二问

![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/httppacket.png)

这题是说攻击者收到的http包的数目 意思就是目的地应该是攻击者的地址才对 

```
(http) && (eth.dst == 00:0c:29:e2:18:b4)
```

### 第三问
问用户名和密码的话 八层应该是与登录有关 我们搜一下关键词login 随便点进去看到有个可疑的值 uname和pass

![ask3](/img/user/images/tryhackme-wireshark小tips/traffic/number6.png)


把uname值拖到搜索栏搜一下 就能找到六个账户密码的条数

![ask3](/img/user/images/tryhackme-wireshark小tips/traffic/6.png)

### 第四问
问一个账户的密码 我们就在上一题的地方找到Client986这个账户 在里面就能翻到密码clientnothere!

![ask4](/img/user/images/tryhackme-wireshark小tips/traffic/client986.png)

### 第五问
我们在上一题找到Client986 我们把这个值拖到搜索栏 把Client986改成Client354 就能搜到与Client354有关的数据包 进去后在周围翻翻就能找到密码

![ask5](/img/user/images/tryhackme-wireshark小tips/traffic/nice354.png)

在周围翻翻就能找到密码

## task4
![task4](/img/user/images/tryhackme-wireshark小tips/traffic/task4.png)

### 第一问
问主机盖乐世a30的mac地址 那我们搜搜看有没有数据包有a30这个字段的

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/a30.png)

找到之后往上面找找地址 destination地址是cisco思科的企业协议 源地址估计就是我们想要的

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/a301.png)

### 第二问
问工作站的netbios注册请求

![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/lival.png)

```
nbns.name contains "LIVALJM" && nbns.flags opcode == 5 
```

nbns.flags opcode == 5 表示nbns的注册请求

### 第三问
问哪个主机请求的ip是172.16.13.85

我们先搜一下dhcp 看一下数据包先 在最底下发现一行requested ip address 把这个值拖到搜索栏和dhcp一起搜索
记得加上我们题目说的ip

![ask3](/img/user/images/tryhackme-wireshark小tips/traffic/a12.png)

```
(dhcp) && (dhcp.option.requested_ip_address == 172.16.13.85)
```

一搜一个准 找到主机名 盖乐世的a12

![ask3](/img/user/images/tryhackme-wireshark小tips/traffic/a121.png)

### 第四问
找u5这个用户的ip 

CNameString在kerberos中就算是用户名 把这个参数拉到搜索栏 搜u5就行了

![ask4](/img/user/images/tryhackme-wireshark小tips/traffic/u5.png)

![ask4](/img/user/images/tryhackme-wireshark小tips/traffic/u51.png)

我这算很繁琐的步骤 用下面这个命令就一步到位了 知道ip是10.1.12.2 诶 要记住不能只填ip 他说是defanged ip

```
kerberos.CNameString contains "u5"
```

科普一下
Defanged IP address”（去毒化的 IP 地址）是一种安全处理技术，于修改 IP 地址的显示格式，使其在文档、日志或安全报告中不被自动识别为可点击的链接，从而防止意外访问或自动化攻击。

把.换成[.]就行了

### 第五问
要查主机名 在kerberos中主机名和用户名有点小区别

![ask5](/img/user/images/tryhackme-wireshark小tips/traffic/user.png)

就得在后面加个$ 所以我们就查CNameString带$的就行

```
kerberos.CNameString contains "$" 
```

![ask5](/img/user/images/tryhackme-wireshark小tips/traffic/xp1.png)

查到了一个叫xp1$的用户名

## task5
![task5](/img/user/images/tryhackme-wireshark小tips/traffic/task5.png)

### 第一问
问什么协议
这题其实我来说没多大灵感 就搜icmp然后往下翻翻 看看有哪个包的大小异常吗 结果发现还真有 点开看看 发现开头就发现了dh协议 ssh敲定了
![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/ssh.png)

### 第二问
开始就是搜索dns看看有什么大小异常的包吗 还是出乎意料 真有一个

![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/data.png)

底下发下一个网站 把网址中的.转化成[.] 输进去发现成了

这两个纯纯狗屎运
## task6
![task2](/img/user/images/tryhackme-wireshark小tips/traffic/task6.png)

### 第一问
问有几次登录失败

```
ftp.response.code == 530
```

530：未登录，密码无效 刚好适合错误的登录尝试

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/error737.png)

### 第二问
问的是文件大小 应该是和文件状态有关
```
ftp.response.code == 213
```
213：文件状态

输完命令之后我们点开追踪流检查一下

![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/213.png)

发现有个39424大小的文件

![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/2131.png)

### 第三问 & 第四问
问文件名和升级文件权限的命令。
同样在这个追踪流里面可以看到。
![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/chmod.png)

文件和命令都看见了。

## task7
![task2](/img/user/images/tryhackme-wireshark小tips/traffic/task7.png)

### 第一问&第二问
这一问纯玄学 问异常的类型
![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/jizhong.png)

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/jizhong2.png)

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/jizhong4.png)

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/jizhong5.png)

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/jizhong6.png)

最后一个是mozilla和mozllla差了个字

第二题刚好问了这个有差别的包的编号是什么 第52个

### 第三问&第四问
第几个包发动log4j攻击的

问了问度娘
Log4j 是一个广泛使用的 Java 日志记录库，jndi（Java 命名和目录接口）是 Java 的一部分，允许 Java 应用程序查找和访问命名服务。

输入命令搜一下 出现的第一个是编号444的包 
```
ip contains "jndi"
```

![aks3](/img/user/images/tryhackme-wireshark小tips/traffic/jndi.png)

点进去发现一个惊喜 应该是base64编码 拉到cyberchief里面解码
![aks3](/img/user/images/tryhackme-wireshark小tips/traffic/jndi1.png)

会发现在用wegt下载 设置权限 运行脚本 把ip转成defang的ip 就是第四题的答案
![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/jizhong3.png)

## task8
![task2](/img/user/images/tryhackme-wireshark小tips/traffic/task8.png)

### 第一问
问发送到“accounts.google.com”的“Client Hello”消息的框架编号是多少？

Client Hello：tls.handshake.type == 1
server Hello： tls.handshake.type == 2  

我们先收集一下所有的Client Hello 随便点一个点开他的server name 拉到搜索栏 server name改成‘accounts.google.com’
![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/client.png)

```
(tls.handshake.type == 1)  && (tls.handshake.extensions_server_name == "accounts.google.com")
```

一下就搜出来了

![ask1](/img/user/images/tryhackme-wireshark小tips/traffic/16.png)

### 第二问
导入秘钥 直接搜http2就出来了 共115个

![ask2](/img/user/images/tryhackme-wireshark小tips/traffic/http2.png)

### 第三问
去322编号的数据包

直接点开跟踪流 把网站的.弄成[.]

![ask3](/img/user/images/tryhackme-wireshark小tips/traffic/322.png)


### 第四问
找这种数据包里的文件有个很diao的方法 我们导出文件

![ask4](/img/user/images/tryhackme-wireshark小tips/traffic/flag0.png)

点进去就能找到疑似flag的包

![ask4](/img/user/images/tryhackme-wireshark小tips/traffic/flag.png)

保存到桌面 查看一下就行了

![ask4](/img/user/images/tryhackme-wireshark小tips/traffic/flag1.png)

## task9
![task2](/img/user/images/tryhackme-wireshark小tips/traffic/task9.png)
### 第一问&第二问
明文凭证 这一问都是在这里面完成

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/cred.png)

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/cred1.png)
第一问的数据包编号就是237

第二个就得看看数据包先 

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/ftp1.png)

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/ftp2.png)

数据包基本就分这两类 user和pass 还有别的无关紧要的 要求是说空密码 就把 request command：pass那行拉去搜索

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/ftp.png)

发现第170个包的pass空空的 没错了

## task10
![task2](/img/user/images/tryhackme-wireshark小tips/traffic/task10.png)


照着题目点击对应的数据包 点开防火墙规则设置

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/fire0.png)

把这两个规则复制粘贴就行 第二问要把deny改成allow 因为题目问的是允许

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/fire.png)

![ask](/img/user/images/tryhackme-wireshark小tips/traffic/fire2.png)


wireshark三个部分总算肝完 其实也不算肝 就是内容小多 内容也干得要死 全是干货 过段时间更一下wireshark的实战 试试水
