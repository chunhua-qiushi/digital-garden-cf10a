---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-ice/","title":"tryhackme-ice","tags":["linux","tryhackme"]}
---


今天来试试blue的下一个挑战 ice。

这个问题也蛮简单的 就是练练手了解一下流程

## task2
![task](/img/user/images/tryhackme-ice/task2.png)

第三问远程桌面是用3389端口

第四问在8000端口的服务是什么

![task](/img/user/images/tryhackme-ice/ice.png)

我们搜一下这个服务的简写是Icecast

在nmap的报告中 能看到主机名

![host](/img/user/images/tryhackme-ice/pcname.png)

## task3
![task](/img/user/images/tryhackme-ice/task3.png)

我们进网站去查看一下[漏洞](https://www.cvedetails.com/cve/CVE-2004-1561/) 

会发现这个漏洞的影响评分是6.4

![msf](/img/user/images/tryhackme-ice/6.4.png)

cve编号为CVE-2004-1561

在msf中搜索 search icecast 就能找到我们要利用的模块

![msf](/img/user/images/tryhackme-ice/search.png)

show options 需要配置一个rhost

## task4
![task](/img/user/images/tryhackme-ice/task4.png)

![task](/img/user/images/tryhackme-ice/task41.png)

我们刚打进去的时候是用的是meterpreter的shell

我们用whoami来查看正在使用icecast进程的用户

![uid](/img/user/images/tryhackme-ice/dark.png)

查看版本信息用sysinfo

![sys](/img/user/images/tryhackme-ice/7601.png)

就能看到Windows的版本和进程的构架

在msf中search eventvwr(这个看了一下提示) 第一个就是我们要的漏洞

![msf](/img/user/images/tryhackme-ice/searchev.png)

会话错误需要配置lhost 监听的ip

输入getprivs就可以找到权限 问题说的是拥有文件的所有权 SeTakeOwnershipPrivilege 这个就是我们要找的

![msf](/img/user/images/tryhackme-ice/ownership.png)

## task5
![task](/img/user/images/tryhackme-ice/task5.png)

在meterpreter的shell中输入ps就能查看进程 提示中说了一个spool的名字

![msf](/img/user/images/tryhackme-ice/spool.png)

服务名叫spoolsv.eve

用migrate -N PROCESS_NAME命令迁移进程

![uid](/img/user/images/tryhackme-ice/migrate.png)

用getuid命令查看我们现在是什么用户

![uid](/img/user/images/tryhackme-ice/getuid.png)

load kiwi之后我们help一下 查看kiwi能用啥命令

![kiwi](/img/user/images/tryhackme-ice/kiwi.png)

这个就是我们需要的

用hashdump来转储用户和密码hash 在下面就能找到密码

![hash](/img/user/images/tryhackme-ice/password.png)

## task6
![task](/img/user/images/tryhackme-ice/task6.png)

这个板块主要是给我们熟悉meterpreter的一些命令来的

第一问就是hashdump命令g

实时观看用户桌面是用screenshare命令
 
想连接系统的麦克风录音用record_mic命令

修改文件时间戳用timestomp命令

黄金票据是golden_ticket_create命令