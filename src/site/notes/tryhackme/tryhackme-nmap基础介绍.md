---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-nmap基础介绍/","title":"tryhackme nmap基础介绍","tags":["linux","#tryhackme","#渗透工具"]}
---


![title](/img/user/images/tryhackme-nmap基础介绍/title.png)



提到Nmap使用多种方式来指定其目标：

- IP 范围使用-：如果要扫描从 192.168.0.1 到 192.168.0.10 的所有 IP 地址，就写成192.168.0.1-10
  
- IP 子网使用/：如果要扫描子网，可以将其表示为192.168.0.1/24，这相当于192.168.0.0-255
- 主机名：您还可以通过主机名指定目标，例如，example.thm

更好地控制Nmap如何通过给定端口发现实时主机（例如-PS[portlist]、-PA[portlist]），-PU[portlist]以进行TCP SYN、TCP ACK 和UDP发现。


- -sL 此扫描仅列出要扫描的目标，而不会实际扫描它们

- -sn 目的是发现活动主机而不尝试发现其上运行的服务。

- -sT 连接扫描  (扫描需要完成tcp三次握手)

- -sS SYN扫描(仅完成第一步)

- -sU UDP扫描 (UDP扫描)

- -F 代表快速模式，扫描 100 个最常见的端口

- -p[range] 允许您指定要扫描的端口范围。例如，-p10-1024扫描从端口 10 到端口 1024，

- -p-25 扫描 1 到 25 之间的所有端口。
- -p- 扫描所有端口和相当于-p1-65535

- -O 启用操作系统检测。

- -sV 启用版本检测。这非常方便您用更少的按键收集有关目标的更多信息。发现了几个开放端口，并想知道哪些服务正在监听它们

- -A 有-O，-sV并且还有更多功能，此选项可实现操作系统检测、版本扫描和跟踪路由等功能。

- -Pn 能将所有主机视为在线并对每个主机进行端口扫描，包括在主机发现阶段未响应的主机。

- -T 可以按名称或编号选择计时模板。举例: -T0(或-T 0) 或-T paranoid(paranoid就是计时模板的名称)以选择最慢的计时。

并行探测的数量可以通过--min-parallelism <numprobes>和 来控制--max-parallelism <numprobes>。这些选项可用于设置主机组中同时活动的 TCP 和 UDP 端口探测数量的最小值和最大值。

类似有用的选项是--min-rate <number>和--max-rate <number>。顾名思义，它们可以控制nmap发送数据包的最小和最大速率。

- --host-timeout <time>。此选项指定您愿意等待的最大时间，适用于速度较慢的主机或网络连接较慢的主机。

- -v 启用详细输出

- -d 使用调试级别输出 最高级别是-d9 您可以通过添加一个或多个“d”或直接指定调试级别来提高调试级别

- oN <filename>- 正常输出
- oX <filename>- XML输出
- oG <filename>- -able 输出 (对和grep有用)grepawk
- oA <basename>- 以所有主要格式输出

随便看看thm的问题
## task2
![task1](/img/user/images/tryhackme-nmap基础介绍/task2.png)

```
nmap -sL ip/27
```
用-sL列出所有要扫描的目标 找到最后一个就行
![task](/img/user/images/tryhackme-nmap基础介绍/sl ip.png)


## task3
![task1](/img/user/images/tryhackme-nmap基础介绍/task3.png)

```
nmap -sT ip
```
就能列出所有tcp端口 在这些端口中能发现一个http的端口8008 ip加端口就能获得第二问的flag

![task2](/img/user/images/tryhackme-nmap基础介绍/tcpport端口.png)

![task2](/img/user/images/tryhackme-nmap基础介绍/8008.png)

## task4
![task1](/img/user/images/tryhackme-nmap基础介绍/task4.png)

```
nmap -A ip
```
用了-A把服务器名称 版本等详细信息都查出来了 慢慢找就行
![task4](/img/user/images/tryhackme-nmap基础介绍/web.png)

## task5

下面就是直接输入就行的 -T的用法

![task1](/img/user/images/tryhackme-nmap基础介绍/task5.png)

## task6
启用调试是-d
![task1](/img/user/images/tryhackme-nmap基础介绍/task6.png)


## task7
在没有root权限 是本地用户的时候只能使用-sT的tcp连接扫描(connect scan)
![task1](/img/user/images/tryhackme-nmap基础介绍/task7.png)

解决了nmap的基础问题