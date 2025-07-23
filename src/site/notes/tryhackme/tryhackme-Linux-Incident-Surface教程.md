---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-Linux-Incident-Surface教程/","title":"Linux Incident Surface教程","tags":["linux","#tryhackme","#应急响应"]}
---

ok啊 因为下学期要来一个linux的课 我打算先把tryhackme的linux先搞一遍 学习学习linux的命令 方便下学期学习

本次教程用了ps lsof journalctl dpkg命令 查了auth.log和dpkg.log 这些log中都得学会用grep命令 这个是真的好用

![开头](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 110940.png)

我们用ssh先连接一下机子
```
ssh tryhackme@ip
```

task3是一个看的文章 我们直接跳过

## task4

![task4](/img/user/images/Linux Incident Surface教程/task4.png)

### 第一步

我们按照文章所说 先运行一个简单程序
但首先记得先sudo su 

![第一步](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 160531.png)

```
gcc simple.c -o /tmp/simple
/tmp/simple
```
![第1步](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 160920.png)


这里的gcc是c的编译器

### 第二步
我们使用ps aux查看运行的进程 
-aux 显示所有包含其他使用者的进程

```
ps aux | grep simple
搜索进程是simple这个名的
```

![第二步](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 161218.png)

### 第三步

这一步要用到我们刚才查询到的进程的pid

```
lsof -p pid
用来搜索对应pid的进程
```
![第三步](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 161542.png)

lsof下面显示的东西等让我们对文件了解多一点

### 第四步 
我们先回到/home/activities/processes 启动一下netcom(./netcom) 这一步能让我们连接到远程ip的

其实到这一步 我们已找到了第一题的答案

```
lsof -l -P-n
查找我们刚才的netcom进程
```

![第四步](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 162402.png)

进入osquery 

```
osqueryi
SELECT pid, fd, socket, local_address, remote_address,remote_port  FROM process_open_sockets WHERE pid =你的pid;
```
![第四步](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 163204.png)

一下子把端口和ip都搞到手了

### 第五步
开始之前 先运行这三个代码

```
sudo useradd attacker -G sudo
sudo passwd attacker
echo "attacker ALL=(ALL:ALL) ALL" | sudo tee -a /etc/sudoers
```

![第五步](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 165656.png)

假如我们cat一下auth.log的useradd字眼

![cat](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 165656.png)

到etc的passwd里面查找
![passwd](/img/user/images/Linux Incident Surface教程/屏幕截图 2025-01-17 171452.png)

你就能发现刚才操作的痕迹

## task5

![task](/img/user/images/Linux Incident Surface教程/task5.png)

先创建一个配置文件
```
sudo nano /etc/systemd/system/suspicious.service
```
将以下复制粘贴进去

```
[Unit]
Description=Suspicious_Service
After=network.target

[Service]
ExecStart=/home/activities/processes/suspicious
Restart=on-failure
User=nobody
Group=nogroup

[Install]
WantedBy=multi-user.target
```

然后重启一下服务
```
重新加载 systemd 管理器配置以识别新服务：
命令：sudo systemctl daemon-reload

启用服务在启动时运行：
命令：sudo systemctl enable suspicious.service

立即开始服务。
命令：sudo systemctl start suspicious.service
```

![第五](/img/user/images/Linux Incident Surface教程/3个sys.png)

我们能看到运行成功

![第五](/img/user/images/Linux Incident Surface教程/running.png)

运行成功之后到/etc/systemd/system 中查找
你会发现一个叫benign.service的东西 cat一下就会发现他和我们写的那个脚本差不多

![ben](/img/user/images/Linux Incident Surface教程/bengin.png)

其中的execstart的benign就是题目需要的进程

最后一题的话 就是搜索日记 一个一个数 直接输入journalctl可能要找一下九月11的记录 我们可以根据日志进行筛选 能看出有七个

```
journalctl -u benign.service --since "2024-09-11"
```

![jour](/img/user/images/Linux Incident Surface教程/journalctl.png)

## task6

![task6](/img/user/images/Linux Incident Surface教程/task6.png)

创建一个目录 然后创建control文件和恶意脚本

![control](/img/user/images/Linux Incident Surface教程/control.png)

把这一行复制到control里面
```
Package: malicious-package
Version: 1.0
Section: base
Priority: optional
Architecture: all
Maintainer: attacker@test.com
Description: This is a malicious Package
```

然后vim创建一个名字叫postinst的脚本

```
#!/bin/bash
# Malicious post-installation script
# It will run this script after installation

# Print a suspicious message - for demonstration
echo "something suspicious"
```
给脚本赋权

```
chmod 755 malicious-package/DEBIAN/postinst(记得你的路径要搞对)
```

退回到malicious-package的根目录 构建安装包

![goujian](/img/user/images/Linux Incident Surface教程/goujian.png)

```
dpkg -i malicious-package.deb
```
搞完这一一步
使用dpkg进行检查已经安装的包

```
dpkg -l | grep malicious
```
![jiancha](/img/user/images/Linux Incident Surface教程/jianchaanz.png)


接着我们查看dpgk.log
```
grep " install " /var/log/dpkg.log
```
![jiancha](/img/user/images/Linux Incident Surface教程/chackanzhuang.png)

你会发现第二题的答案出来了 c2omm

## task7
![task7](/img/user/images/Linux Incident Surface教程/题目7.png)

其实看到题目一直被困在auth中 我以为只有一个 进入到/var/log中 

```
ll auth*

```
其实会发现三个auth的日志

![auth](/img/user/images/Linux Incident Surface教程/auth.png)

```
cat auth.log | grep ssh
cat auth.log.1 | grep ssh
```
![auth](/img/user/images/Linux Incident Surface教程/auth.png)

题目说是ssh 我们就搜关键词 一个一个检查

```
cat auth.log | grep ssh
```
![check](/img/user/images/Linux Incident Surface教程/authlog2.png)

```
cat auth.log.1 | grep ssh
```

![check](/img/user/images/Linux Incident Surface教程/authcheck.png)

到此所有任务就算完成了 只能说是做的过程波澜曲折了 可能也是我比较菜吧 继续加油


