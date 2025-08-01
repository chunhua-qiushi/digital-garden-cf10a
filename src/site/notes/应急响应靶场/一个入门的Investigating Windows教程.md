---
{"dg-publish":true,"permalink":"/应急响应靶场/一个入门的Investigating Windows教程/","title":"一个入门的Investigating Windows教程","tags":["靶场","tryhackme"]}
---


最近也是开始放寒假了，从网上知道了try hack me这个网址 刚好本人的技术又不咋地 于是决定好好提升一下技术 从最简单的基础部分开始。

![主题](/img/user/images/一个入门教程/屏幕截图 2025-01-14 201646.png)

这次使用了cmd powershell 注册表 计算机管理 任务计划程序 事件查看器 防火墙 c盘的system32的文件 ~~(省流）~~都是蛮基础的windows工具

---
我们来看看第一道题

![第一题](/img/user/images/一个入门教程/屏幕截图 2025-01-14 201913.png)
大致意思就是这个windows机器的版本和年份是什么

进入主机之后 直接点击设置 在其中查看操作系统的版本号为windows server 2016

![第一题解释](/img/user/images/一个入门教程/屏幕截图 2025-01-14 145316.png)

第二种办法就是在cmd中输入**systeminfo** 查看版本信息

![第一题解释](/img/user/images/一个入门教程/屏幕截图 2025-01-14 202928.png)

[[tips和一些工具/Windows的命令 1#^d31370\|Windows的命令 1#^d31370]]
---
下一题 题目大意谁是最晚登录的

![第二题](/img/user/images/一个入门教程/屏幕截图 2025-01-14 203100.png)

我们现在cmd里面输入net user 先查看有哪些用户

![第二题解释1](/img/user/images/一个入门教程/屏幕截图 2025-01-14 203406.png)

发现有五个用户 我们用net user 用户名 来一个一个排查 对比last logon的时间可以知道最后一个是administraor

![第二题解释1](/img/user/images/一个入门教程/屏幕截图 2025-01-14 203524.png)

[[tips和一些工具/Windows的命令 1#^2f1f3e\|Windows的命令 1#^2f1f3e]]

当然 这是个土办法 我们还有一招 打开powershell 输入以下指令
```
get-localuser |select name,lastlogon
```
![第二题解释2](/img/user/images/一个入门教程/屏幕截图 2025-01-15 090059.png)

[[tips和一些工具/Windows的命令 1#^709495\|Windows的命令 1#^709495]]

很简单明了的可以看出谁是最后一个登录的用户
~~其实顺便下面一道题 把john的做最后登录时间查出来了~~

---
下面一题

![第三题](/img/user/images/一个入门教程/屏幕截图 2025-01-14 203706.png)

这题我们使用net user john来查看时间 

![第三题解释](/img/user/images/一个入门教程/屏幕截图 2025-01-14 203826.png)

[[tips和一些工具/Windows的命令 1#^2f1f3e\|Windows的命令 1#^2f1f3e]]

---
接着下一题

![第四题](/img/user/images/一个入门教程/屏幕截图 2025-01-14 204914.png)

看这个ip的话有一个很nt的方法 在你打开这个机的时候 前几分钟会弹出一个终端 那上面写着ip

![第四题解释1](/img/user/images/一个入门教程/屏幕截图 2025-01-14 150219.png)

正经办法还有一个 就是在注册表里面查找那个程序 **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run** 你就会发现这个程序的ip

![第四题解释2](/img/user/images/一个入门教程/屏幕截图 2025-01-15 090843.png)

---
接下来是哪几个用户是属于administraor组

![第五题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 091809.png)

直接在计算机管理里面查看administraor组里面有谁就可以了

![第五题解释](/img/user/images/一个入门教程/屏幕截图 2025-01-15 091600.png)

---
下面几个题 可以一起解决了

![第六题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 092017.png)

其实我们可以大致猜出他是个恶意程序 可以到任务计划程序中查找

![第六题解释1](/img/user/images/一个入门教程/屏幕截图 2025-01-15 092603.png) 

[[tips和一些工具/Windows的命令 1#^f213c0\|Windows的命令 1#^f213c0]]

我们可以发现这个明显和别的任务有区别 我们点击action进行查看详细的信息

![第六题解释2](/img/user/images/一个入门教程/屏幕截图 2025-01-15 092427.png) 

可以知道运行的文件为nc.ps1 端口是1348

---
下一题 jenny最后一次登录是啥时候

![第七题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 092902.png) 

直接net user jenny 发现人家压根就没登陆过

![第七题解释1](/img/user/images/一个入门教程/屏幕截图 2025-01-15 093023.png)

---
这些东西是怎么来的

![第八题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 093156.png)

打开c盘文件夹 发现一个突兀的文件夹tmp 第六感告诉我这个文件夹绝壁有问题 直接打开 查看里面的文件的时间 19年3月2日 

![第八题解释](/img/user/images/一个入门教程/屏幕截图 2025-01-15 093508.png)

---

一次的特殊权限的新登录（原谅我的瞎寄吧翻译）

![第九题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 093542.png)

这题的话蛮有意思的 我是在事件发生器里面的security筛选4672（问了度娘 这个就是题目需要的Special privileges assigned to new logon.）

结果死活找不到需要的。后来我去观摩了一下大佬的做法，上面一道题不是知道最早登录的时间嘛 19年3月2日 用筛选器选出这一天的4672的所有记录 再根据提示 秒是49，就在里面找到了一个记录。

![第九题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 094502.png)

[[tips和一些工具/Windows的命令 1#^534268\|Windows的命令 1#^534268]]

啥工具搞密码

![第十题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 094900.png)

这题挺好的 mimikatz直接秒了（点开这个机子 时不时就有个mimikatz弹窗） 还有一招 点开tmp文件的min-out文件 里面可以看到

![第十题](/img/user/images/一个入门教程/屏幕截图 2025-01-15 094950.png)

最后一波问题

![最后一波](/img/user/images/一个入门教程/屏幕截图 2025-01-15 095638.png)

打开c盘的C:\Windows\System32\drivers\etc的host文件 

![最后一波](/img/user/images/一个入门教程/屏幕截图 2025-01-15 095508.png)

这两个谷歌的ip感觉不太对劲哦 最后一题就可以直接输入google.com 第一题ip就是76.32.97.132

---
第二题 题目刚好涉及到了网站服务器 windows就是iis服务 我们去c盘找一下有啥线索吗

inetpub文件夹是iis服务端的一个核心部分 网站目录啥的都存储在那 我们可以进去找找有什么线索吗

![最后一波](/img/user/images/一个入门教程/屏幕截图 2025-01-15 101030.png)

---
第三题端口 可以在防火墙里面查看 发现是1337

![最后一波](/img/user/images/一个入门教程/屏幕截图 2025-01-15 100137.png)

就这样 完成了第一个room。其实这个还算是基础的room 来给我们熟悉一下 我们遇到问题要去哪个区域查找我们想要的数据





