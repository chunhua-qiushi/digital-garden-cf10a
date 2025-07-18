---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme Linux-System-Hardening教程/","title":"Linux System Hardening教程","tags":["linux","安全","管理"]}
---

第一次遇到困难难度的题 有点意思 怕不会是个难关哦 

![title](/img/user/images/Linux System Hardening/linux.png)

本房间涵盖了与Linux强化相关的各种主题。在本房间结束时，将通过以下方式了解有关如何提高Linux系统安全性的更多信息：

- **物理安全**
- **文件系统加密**
- **防火墙配置**
- **远程访问**
- **软件和服务**
- **更新和升级**
- **日志**

## task2
这个是讲物理安全

我们可以根据要保护的Linux系统考虑添加GRUB密码。许多工具都有助于实现这一点。其中一个工具是，它会提示您输入两次密码并为您生成哈希值。生成的哈希值应根据Linux发行版（例如：Fedora和Ubuntu）添加到相应的配置文件中。此配置将阻止未经授权的用户重置您的 root 密码。它将要求用户提供密码才能通过GRUB访问高级启动配置，包括以 root 访问权限登录。**grub2-mkpasswd-pbkdf2**

我们能在文章中发现第一题的答案
```
grub2-mkpasswd-pbkdf2
```
grub2-mkpasswd-pbkdf2 是一个命令行工具，用于生成加密的密码哈希。

生成的密码哈希使用 **PBKDF2**（Password-Based Key Derivation Function 2）算法，这是一种安全的密码哈希算法，可以有效防止彩虹表攻击和暴力破解。

第二题也得出答案了 **Password-Based Key Derivation Function 2**

## task3

这个讲的是文件系统分区与加密

luks(全称是**Linux Unified Key Setup** 是Linux系统中广泛使用的磁盘加密规范，它提供了一种强大且用户友好的标准化加密机制，通过对物理和逻辑磁盘分区内的数据进行加密来保护数据，确保只有授权用户才能访问加密分区的内容。)

luks结构

![disk](/img/user/images/Linux System Hardening/disk.png)

- LUKS phdr：它代表**LUKS分区头**。LUKS phdr存储有关UUID（通用唯一标识符）、使用的密码、密码模式、密钥长度和主密钥的校验和的信息。
  <br>
- KM：KM 代表**密钥材料**，其中有 KM1、KM2、...、KM8。每个密钥材料部分都与一个密钥槽相关联，该密钥槽可以在LUKS phdr 中指示为活动状态。
  换句话说，我们可能使用第一个用户的密码加密主密钥并保存在 KM1 中，使用第二个用户的密码加密主密钥并保存在 KM2 中，依此类推。
  <br>
- 批量数据：这是指**使用主密钥加密的数据**。主密钥保存在密钥材料部分中，并使用用户的密码进行加密。

我们看看问题

![task3](/img/user/images/Linux System Hardening/task3.png)

第一问 luks全称叫**Linux Unified Key Setup**

第二问真的有难度 看看提示 把提示的命令粘贴进去

![3](/img/user/images/Linux System Hardening/3.png)

然后到myvault里面就能看到答案

![3](/img/user/images/Linux System Hardening/3 1.png)


```
sudo cryptsetup open --type luks secretvault.img myvault && sudo mount /dev/mapper/myvault myvault/
```

### 命令解释

 **sudo cryptsetup open --type luks secretvault.img  myvault**

 - **sudo**：以超级用户权限运行命令，因为解密和挂载操作通常 需要管理员权限。
 - **cryptsetup**：用于管理加密设备的命令行工具。
 - **open**：cryptsetup的子命令，用于打开一个加密设备。
 - **--type luks**：指定加密设备的类型为LUKS（Linux  Unified Key Setup）。
 - **secretvault.img**：加密图像文件的路径。
 - **myvault**：解密后的设备映射器名称，用于在/dev/ mapper/下创建一个设备节点。
 这个命令的作用是将**secretvault.img**文件解密，并创建一个 设备映射器/dev/mapper/myvault，以便后续可以挂载这个设备。
 
 ---
**sudo mount /dev/mapper/myvault myvault/**

- **sudo** ：以超级用户权限运行命令。
- **mount**：用于挂载文件系统的命令。
- **/dev/mapper/myvault**：解密后的设备映射器路径。
- <strong>*myvault/</strong>：挂载点目录，解密后的文件系统将挂载到这个目录。
  
这个命令的作用是将解密后的设备/dev/mapper/myvault挂载到myvault/目录，使得你可以访问其中的文件

## task4
这一个部分讲的是防火墙

设置防火墙有很多安全优势。首先，防火墙规则可以精细控制哪些数据包可以离开系统以及哪些数据包可以进入系统。因此，防火墙规则通过控制设备之间的网络流量来帮助减轻各种安全风险。

更重要的是，可以制定防火墙规则来确保没有客户端可以充当服务器。换句话说，攻击者无法在目标计算机上启动可访问的侦听端口；漏洞利用者可以启动侦听端口，但防火墙将阻止所有传入的连接尝试。

  防火墙有两个主要功能：

- 1.什么可以进入？允许或拒绝数据包进入系统。
- 2.什么可以离开？允许或拒绝数据包离开系统。

进入防火墙规则的 IP 标头字段包括：

- 源 IP 地址
- 目标 IP 地址
  
防火墙规则主要关注的TCP / UDP标头字段是：

- 源TCP / UDP端口
- 目标TCP / UDP端口

netfilter 为Linux内核 2.4.x 及更高版本提供数据包过滤软件。netfilter 需要(例如**iptables**或 **nftables**来管理。)

说有点多了 我们直接看题

![task4](/img/user/images/Linux System Hardening/firewall.png)

前面提到**iptables** 我们就直接用iptables列出防火墙的规则 用grep过滤出我们想要的

```
iptables --list | grep tcp
iptables --list | grep ucp
```
![task4](/img/user/images/Linux System Hardening/tcp.png)

![task4](/img/user/images/Linux System Hardening/udp.png)


## task5
远程访问
常见的攻击包括：
- 密码嗅探
- 密码猜测和暴力破解
- 利用监听服务


![task5](/img/user/images/Linux System Hardening/task5.png)

查看sshd_config的隐藏 flag

直接进入/etc/ssh/sshd_config查看就行了

![sshd](/img/user/images/Linux System Hardening/sshd.png)


## task6
讲的是用户账户安全

### 使用 sudo
为了避免以身份登录root，更好的方法是将为管理目的创建的帐户添加到**sudoers** （即可以使用该sudo命令的组）。sudo代表超级用户执行，它应该在任何需要root特权的命令之前。

据Linux发行版，我们可以通过以下方式将用户添加到 sudoers 组。某些发行版（例如 Debian 和 Ubuntu）将 sudoers 称为组**sudo**

```
usermod -aG sudo username
```

其他发行版（例如 RedHat 和 Fedora）将 sudoers 组称为**wheel**。

```
usermod -aG wheel username  
```

### 禁用未使用的用户
一种简单的方法是编辑<strong>/etc/passwd</strong>文件并将我们要禁用的用户帐户的 shell 设置为/sbin/nologin。

### 禁用 root
一旦创建了用于管理目的的帐户并将其添加到**sudo/wheel**组，您可能会考虑禁用该root帐户。
一种直接的方法是修改<strong>/etc/passwd</strong> 将 shell 更改root为<strong>/sbin/nologin</strong>。换句话说，编辑/etc/passwd并将行更改**bin/bash** 为 **/sbin/nologin**。

我们看看题目

![task6](/img/user/images/Linux System Hardening/task6.png)

第一题/sbin/nologin

第二题很显而易见 wheel

第三题sudo

第四题 问sudo组里面还有谁 我们之间grep一下/etc/group就可以知道1

```
cat group | grep sudo
```

![group](/img/user/images/Linux System Hardening/group.png)

## task7

软件和服务

系统上安装的每款软件都会增加潜在漏洞的数量。换句话说，安装额外的软件包和新服务会增加攻击者可利用的漏洞，从而访问系统，并最终访问网络上的其他系统。

- 禁用不必要的服务
改善安全状况的最简单方法之一是删除或禁用不需要的服务和软件包。每个软件包都有一定的风险，而且我们无法知道何时会发现相关的漏洞。

- 阻止不需要的网络端口
删除所有不需要的软件包并禁用可能不会被删除的预安装服务后，相应地设置防火墙规则至关重要。

- 避免使用遗留协议
过去，Telnet 曾是远程访问系统的主要协议；TFTP 协议通常用于传输文件。随着安全替代方案的发布，不应再允许使用此类协议。
- eg：
SSH协议现已广泛使用，取代了 Telnet 。例如，安全文件传输议 (**SFTP**) 协议为 TFTP 协议提供了很好的替代方案。关键在选择和使用安全的替代方案。

- 删除识别字符串
每当你连接到远程服务器时，它通常会回复其版本号。此信息会向攻击者透露各种信息，例如服务器/程序的名称、版本号和主机操作系统。

根据上面的文字 能知道 task7的答案是sftp(替代了tftp)

## task8
升级和更新策略
以下两个命令更新基于 Debian 的发行版，例如 Ubuntu：

```
apt update从配置的源下载包信息
apt upgrade从配置的源安装所有软件包的可用升级
```

以下命令更新 RedHat 或 Fedora 系统：

```
dnf update在较新版本上（Red Hat Enterprise Linux 8 及更高版本）
yum update在旧版本上（Red Hat Enterprise Linux 7 及更早版本）
```
这个是题目

![task8](/img/user/images/Linux System Hardening/task8.png)

前面几题其实就是基础知识 百度啥的都可以知道 直接复制粘贴就完了

最后一题的话 进/etc/apt/source.list就可以看到flag

![source](/img/user/images/Linux System Hardening/source.png)

## task9

![task9](/img/user/images/Linux System Hardening/task9.png)

这两天就是看你的**tail**和**grep**命令的使用了

```
tail -n 15 kern.log
grep denied source
```
结束这场战斗了 算比较深一点点了解linux在安全上的各种措施。要把这些用在实际的地方在是关键