---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-Metasploit-科普/","title":"tryhackme-Metasploit 科普","tags":["#tryhackme","#渗透工具"]}
---

好久没更新了 最近在搞obsidian的手机端电脑端的同步 就一直没更新。总算是忙完了。

来看看thm的Metasploit基础知识

# Metasploit：简介
Metasploit是最广泛使用的漏洞利用框架。Metasploit是一款功能强大的工具，可以支持渗透测试的所有阶段，从信息收集到后期漏洞利用。

Metasploit Framework的主要组件可以概括如下；

msfconsole：主命令行界面。

模块：支持模块，例如漏洞利用、扫描器、有效载荷等。

工具：独立工具，可帮助进行漏洞研究、漏洞评估或渗透测试。其中一些工具是 msfvenom、pattern_create 和 pattern_offset。将在本模块中介绍 

## Metasploit的主要部件

### task2
要介绍几个东西：

1. 漏洞（Exploit）:利用目标系统上存在的漏洞的一段代码。

2. 脆弱性（Vulnerability）: 影响目标系统的设计、编码或逻辑缺陷。漏洞的利用可能导致机密信息泄露或允许攻击者在目标系统上执行代码。

3. 有效载荷（Payload）: 漏洞利用会利用漏洞。但是，如果我们希望漏洞利用达到我们想要的结果（获得目标系统的访问权限、读取机密信息等），我们需要使用有效载荷。有效载荷是将在目标系统上运行的代码。

在msfconsole中有几个交互性的模块：

1. **Auxiliary** 辅助模块
- 任何支持模块，例如扫描仪、爬虫和模糊器，都可以在这里找到。

2. **encoders**
- 编码器将允许您对漏洞和有效负载进行编码

3. **Evasion**
- evasion模块，生成木马程序

4. **Exploits**
- 漏洞利用

5. **nop**
- 空指令不会对程序运行状态造成任何实质影响的空操作或者无关操作指令

6. **payload**
- 有效负载是在目标系统上运行的代码。
分类：
 - payload下看到四个不同的目录：
- **adapters** (适配器)
- **singles**（单一有效负载）
- **stagers**（阶段式Payload）
- **stages**（阶段二有效负载）

7. **Post**
- 后期模块将在上述渗透测试过程的最后阶段，即后期开发中发挥作用。

简单看看问题说了啥
![task2](/img/user/images/tryhackme-Metasploit-科普/Screenshot 2025-03-07 151900.png)

利用系统缺陷的代码是**exploit**

攻击者在目的系统上运行的代码是**payload**

自包含有效载荷的是**singles**

这个是属于单一有效载荷

## msfconsole
msfconsole有很多适用的命令 **set**命令 **history**命令 **use**命令 

**show options** 可以看需要设置和设置好的工作环境

我们在show options中常见的参数：

- **RHOSTS**： “远程主机”，目标系统的 IP 地址。可以设置单个 IP 地址或网络范围。这将支持 CIDR（无类域间路由）表示法（/24、/16 等）或网络范围（10.10.10.x – 10.10.10.y）。您还可以使用列出目标的文件，每行一个目标，使用 file:/path/of/the/target_file.txt

- **RPORT**： “远程端口”，存在漏洞的应用程序在目标系统上运行的端口。

- **PAYLOAD**：您将与漏洞利用一起使用的有效载荷。

- **LHOST**： “Localhost”，攻击机器 也就是你的 IP 地址。

- **LPORT**： “本地端口”，反向 shell 将使用该端口进行连接。这是攻击机器上的端口，您可以将其设置为任何其他应用程序未使用的端口。

- **session**：使用Metasploit与目标系统建立的每个连接都将有一个会话 ID。您将使用会话 ID 与后开发模块一起使用，这些模块将使用现有连接连接到目标系统。

**info**命令可以查看详细消息

**search**命令用来搜索我们需要的东西 假如我们要搜索特定的类型就可以设置type

eg：如果我们希望搜索结果仅包含辅助模块，我们可以将类型设置为辅助。

search type：auxiliary

**back**是退出到上一级

**setg** 命令设置一个全局值，该值将一直使用，直到您退出Metasploit或使用该 unsetg 命令清除它。

使用该**unset**命令清除任何参数值

命令启动模块**exploit**

### task3&task4

![task3](/img/user/images/tryhackme-Metasploit-科普/task3.png)

第二问就是info 后面加要查的东西

![task3](/img/user/images/tryhackme-Metasploit-科普/todb.png)

这个是task4

![task4](/img/user/images/tryhackme-Metasploit-科普/task4.png)

第一问

```
set lport 666
```

第二问设置全局值

```
setg rhosts 10.10.19.23
```

第三问

```
unset payload
```

第四问

```
exploit
```

# Metasploit：利用
将介绍数据库功能如何使管理范围更广的渗透测试工作变得更加容易。最后，将研究如何使用msfvenom生成有效载荷以及如何在大多数目标平台上启动Meterpreter会话。

我们将涵盖的主题包括：
- 如何使用Metasploit扫描目标系统。
- 如何使用Metasploit数据库功能。
- 如何使用Metasploit进行漏洞扫描。
- 如何使用Metasploit利用目标系统上的易受攻击的服务。
- 如何msfvenom 使用来创建有效载荷并在目标系统上获取 Meterpreter 会话。

## port scanning
**search portscan**命令列出可用的端口扫描模块。

use 一个适合的模块之后 要show options 设置一下关键的参数

有一些参数我们要了解一下

- CONCURRENCY: 并发 同时扫描的目标数量。
- PORTS: 要扫描的端口范围
- RHOSTS: 要扫描的目标或目标网络
- THREADS: 同时使用的线程数。线程越多，扫描速度越快。

还有很多端口扫描可以给我们选择

UDP服务识别
scanner/discovery/udp_sweep 模块将允许您快速识别通过UDP（用户数据报协议）运行的服务。

Metasploit提供了几个有用的辅助模块，使我们能够扫描特定服务。例如smb模块的smb_enumshares 和，smb_version

## 数据库
实际用会涉及很多个目标 就需要数据库来简化项目管理

启动 PostgreSQL 数据库，将使用以下命令： 

systemctl start postgresql


然需要使用命令初始化 msfdb init数据库。

数据库功能将允许您创建工作区来隔离不同的项目。首次启动时，处于默认工作区中。可以使用workspace 命令列出可用的工作区。 

添加工作区-a或使用参数删除工作区-d

workspace -h命令列出该workspace 命令的可用选项

运行这样的方式运行Nmap 
```
db_nmap
```
所有结果都将保存到数据库中。 

现在，您可以分别使用hosts和services命令获取与目标系统上运行的主机和服务相关的信息 

## Exploitation
show payloads命令列出可以与该特定漏洞一起使用的其他命令 在使用漏洞的时候可以用这个命令查看别的有效负载 

决定某个有效呼负载之后可以用set payload

sessions 命令将列出所有活动会话。支持许多选项，可帮助更好地管理会话。

看看task5

![task5](/img/user/images/tryhackme-Metasploit-科普/task5.png)

看提示的话 就是要使用/windows/smb/ms17_010_enteralblue这个漏洞

在里面设置一个payload /generic/shell_reverse_tcp

![task5](/img/user/images/tryhackme-Metasploit-科普/payload.png)

![task5](/img/user/images/tryhackme-Metasploit-科普/payload1.png)

![task5](/img/user/images/tryhackme-Metasploit-科普/payload3.png)


成功进入 用dir命令来搜一下我们需要的文件 再type一下就能查看文件

![task5](/img/user/images/tryhackme-Metasploit-科普/flag.png)

![task5](/img/user/images/tryhackme-Metasploit-科普/flag1.png)

第三问其实一开始就有点唐了 一直在Meterpreter那里 得先shell升级一下 用hashdump就能找到hash值

## Msfvenom
Msfvenom 取代了 Msfpayload 和 Msfencode，它允许您生成有效载荷

# Metasploit：Meterpreter
Meterpreter是一个Metasploit有效负载，它使用许多有价值的组件支持渗透测试过程。

getpid返回运行Meterpretergetpid 的进程 ID 

msfvenom --list payloads 命令并 grep 了“meterpreter”有效载荷（添加| grep meterpreter到命令行）

决定使用哪个版本的Meterpreter主要基于三个因素；

- 目标操作系统（目标操作系统是Linux还是 Windows？是 Mac 设备吗？是 Android 手机吗？等等）
- 目标系统上可用的组件（是否安装了 Python？这是一个PHP网站吗？等等）
- 目标系统可以拥有的网络连接类型（它们是否允许原始TCP连接？您只能拥有 HTTPS 反向连接吗？IPv6 地址是否不像 IPv4 地址那样受到严格监控？等等） 

Meterpreter将为您提供三类主要工具；

- 内置命令
- Meterpreter工具
- Meterpreter脚本

最后一个没啥好介绍的 上实战

![task6](/img/user/images/tryhackme-Metasploit-科普/task6.png)

介绍中说了
可以使用（使用 exploit/windows/ smb /psexec）

用户名：ballen

密码：Password1

设置好smbpass smbuser和rhost就能开打了

![task6](/img/user/images/tryhackme-Metasploit-科普/task51.png)

进去之后输入sysinfo查看信息

可以看到计算机名字和目标域的名字

计算机名字是ACME-TEST

目标域是FLASH

![task6](/img/user/images/tryhackme-Metasploit-科普/task52.png)

第三问可能创建的共享域名字

这个需要用到别的模块 先把我们的meterpreter弄到session里面保存先

![task6](/img/user/images/tryhackme-Metasploit-科普/task591.png)

![task6](/img/user/images/tryhackme-Metasploit-科普/task59.png)

看了一下提示 是说要用post/windows/gather/enum_shares这个模块的内容 设置session为我们保存过的那个 运行一次就能知道结果

![task6](/img/user/images/tryhackme-Metasploit-科普/task592.png)

我们看结果有一个是shares的文件 可能就是ta 把名字输进去还真是speedster

第四问是问用户的ntlm哈希值

这里是用hashdump抓取密码

![task6](/img/user/images/tryhackme-Metasploit-科普/task53.png)

将找到的值用在线破解工具试试

![task6](/img/user/images/tryhackme-Metasploit-科普/task54.png)

密码为Trustno1

第六问和第八问问的都是文件在哪里

在这里可以用search命令去搜索 或者是 dir /s /b命令就能找到他们的路径

知道路径之后可以用type 加文件绝对路径来查看文件内容 

![task6](/img/user/images/tryhackme-Metasploit-科普/task55.png)

![task6](/img/user/images/tryhackme-Metasploit-科普/task56.png)


![task6](/img/user/images/tryhackme-Metasploit-科普/task57.png)

![task6](/img/user/images/tryhackme-Metasploit-科普/task58.png)

有一说一 msf这个工具是确实好用 很多地方我还不是很熟悉 得漫漫了解