---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-blue/","title":"tryhackme-blue","tags":["linux","tryhackme","oscp","靶场"]}
---


这个是个蛮简单的虚拟机的测试 就当随便玩玩

## task1

![task](/img/user/images/tryhackme-blue/task.png)

下载完虚拟机之后 我是用nmap扫描有啥端口 在后面的时候了解到了个新的参数 **--scrip=vuln** 在nmap中扫描漏洞

![nmap](/img/user/images/tryhackme-blue/nmap.png)
  
![task1](/img/user/images/tryhackme-blue/vuln.png)

1000以下的开放端口有三个 本来是有四个的 其中有一个状态并不是开放的

我们加上参数--scrip=vuln扫描之后会发现 扫出来了一个ms17-010漏洞


## task2
来看看任务2

![task](/img/user/images/tryhackme-blue/task2.png)

task2的话 问题就是基础的问题 

第二问 我们到msfconsole中搜索ms17-010 第一个就是我们需要的漏洞 exploit/windows/smb/ms17_010_eternalblue

第三问的话 肯定要去配置rhost的 就是你的攻击目标的ip

我有时候会不成功 最好加set payload windows/x64/shell/reverse_tcp进去 这样子攻进去就能直接获得Windows的权限

## task3

要使用的模块是post/multi/manage/shell_to_meterpreter 

使用这些模块 要先设置session参数

## task4

![task4](/img/user/images/tryhackme-blue/task4.png)

这个要在Meterpreter中进行操作 用hashdump来爆出用户和密码

![task4](/img/user/images/tryhackme-blue/hashdump.png)
[[tips和一些工具/linux工具#^3d6d9f\|linux工具#^3d6d9f]]

除了administrator和guest用户还有一个jon 这个就是非默认用户

把这一串全部复制到一个文件中 用john来破解 一开始还在想把单独一串拿来破解 结果一直没效果。之后想到这个是Windows用户 john的--format参数要用nt

![task4](/img/user/images/tryhackme-blue/pojie.png)

能破解出密码是alqfna22

[[tryhackme/tryhackme-John-the-Ripper-The-Basics#^4b4fca\|tryhackme-John-the-Ripper-The-Basics#^4b4fca]]]
## task5

![task5](/img/user/images/tryhackme-blue/task5.png)

其实知道密码可以登进Windows去找 或者在meterpreter中用命令搜索 

![task](/img/user/images/tryhackme-blue/flag1.png)

flag2的提示说了可以在 Windows 中存储密码的位置找到此标志。

加密后的密码哈希值（密文）通常存储在`C:\Windows\System32\config\SAM`文件中。 这个文件保存了Windows系统中所有用户的密码哈希值。 

我们可以到C:\Windows\System32\config去找找

![task](/img/user/images/tryhackme-blue/flag2.png)

结果发现真有flag2

第三个其实可以直接在meterpreter中 用dir命令来搜索

![task](/img/user/images/tryhackme-blue/flag3.png)

blue完成



