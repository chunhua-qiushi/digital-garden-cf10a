---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-Gobuster：基础知识/","title":"tryhackme-Gobuster：基础知识","tags":["linux","tryhackme"]}
---


![titile](/img/user/images/tryhackme-Gobuster：基础知识/title.png)

Gobuster是一款用 Golang 编写的开源攻击工具。它使用特定的单词列表并处理传入的响应，通过暴力枚举 Web 目录、DNS子域、虚拟主机、Amazon S3存储桶和 Google Cloud Storage。

学习目标
了解枚举的基础知识
如何使用Gobuster枚举 Web 目录和文件
如何使用Gobuster枚举子域名
如何使用Gobuster枚举虚拟主机
如何使用单词表

首先先按照task2进行配置环境

```
需要更改/etc/resolv-dnsmasq文件：
在 AttackBox 上打开一个终端并输入命令：sudo nano /etc/resolv-dnsmasq。

插入nameserver 10.10.15.13为第一行。

按 CTRL+O，然后按 ENTER 保存文件，然后按 CTRL+X 退出编辑器。

输入命令/etc/init.d/dnsmasq restart重新启动Dnsmasq服务。
```

## task3

重点介绍dir、dns和vhost命令。将在以下任务中介绍每个命令。

flags：这些是我们可以配置以自定义命令的特定选项。让我们看看我们在这个房间中经常使用的标志：

| short flags  | long flags       | 描述 |
|------|-----------|------|
| -t   | --threads  | 此标志配置用于扫描的线程数。每个线程都会发出请求，但会略有延迟。默认线程数为 10。使用大型单词表时，此数量可能会很慢。您可以根据可用的系统资源增加或减少线程数。 |
| -w   | --wordlist | 该标志配置了用于迭代的单词表。每个单词表条目都附加到您在命令中包含的 URL。 |
|      | --delay    | 此标志定义发送请求之间等待的时间。一些 Web 服务器包含通过查看在特定时间段内收到的请求数量来检测枚举的机制。我们可以增加后续请求之间的延迟，使其看起来像正常的 Web 流量。 |
|      | --debug    | 当我们的命令出现意外错误时，此标志可以帮助我们排除故障。 |
| -o   | --output   | 该标志将枚举结果写入我们选择的文件中。 |

eg:
让我们看一个例子，了解如何使用这些命令和标志来枚举 Web 目录：
```
gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64
```

gobuster dir表示我们将使用目录和文件枚举模式。
-u "http://www.example.thm/"告诉Gobuster目标 URL是http : //example.thm/ 。

-w /usr/share/wordlists/dirb/small.txt指示Gobuster使用small.txt单词表对网络目录进行暴力破解。Gobuster将使用单词表中的每个条目形成一个新的 URL，并向该 URL 发送 GET 请求。如果单词表的第一个条目是图像，Gobuster将向http://example. thm /images/发送 GET 请求。

-t 64将Gobuster将使用的线程数设置为 64。这大大提高了性能。

## task4
目录和文件枚举

gobuster dir命令。逐一介绍超出了范围，但在下表中，我们列出了涵盖大多数场景的标志：

| short flags | long flags               | 描述                                                                                 |
| ----------- | ------------------------ | ---------------------------------------------------------------------------------- |
| -c          | --cookies                | 此标志配置一个 cookie 来传递每个请求，例如会话 ID。                                                    |
| -x          | --extensions             | 此标志指定要扫描的文件扩展名。例如，.php、.js。                                                        |
| -H          | --headers                | 此标志配置与每个请求一起传递的整个标头。                                                               |
| -k          | --no-tls-validation      | 此标志在使用 https 时跳过检查证书的过程。在 CTF 活动或测试室（如 THM 上的测试室）中，通常会使用自签名证书。这会导致 TLS 检查期间出现错误。   |
| -n          | --no-status              | 如果不想看到收到的每个响应的状态代码，可以设置此标志。这有助于保持屏幕上的输出清晰。                                         |
| -P          | password                 | 可以将此标志与 --username 标志一起设置以执行经过身份验证的请求。当您从用户那里获得凭据时，这很方便。                           |
| -s          | --status-codes           | 通过此标志，可以配置想要显示的收到的响应的状态代码，例如 200，或者 300-400 这样的范围。                                 |
| -b          | --status-codes-blacklist | 此标志允许配置您不想显示的收到的响应的状态代码。配置此标志将覆盖 -s 标志。                                            |
| -U          | --username               | 可以将此标志与 --password 执行经过身份验证的请求的标志一起设置。当您从用户那里获得凭据时，这很方便。                           |
| -r          | --followredirect         | 此标志将 Gobuster 配置为遵循其收到的作为对发送请求的响应的重定向。HTTP 重定向状态代码（例如 301 或 302）用于将客户端重定向到不同的 URL。 |

运行Gobuster dir，请使用以下命令格式

该命令还包含标志-u和-w。这两个标志是 Gobuster 目录枚举工作所必需的。
```
gobuster dir -u "http://www.example.thm" -w /path/to/wordlist
```

此命令使用单词列表directory-list-2.3-medium.txt扫描位于www.example.thm的所有目录。让我们仔细看看命令的每个部分：

```
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r
```

- gobuster dir：配置Gobuster使用目录和文件枚举模式。
-u http://www.example.thm：
<br>
- URL 将是 Gobuster 开始查找的基本路径。因此，上面的 URL 使用的是根 Web 目录。可以将其视为http://www.example.thm/path/to/folder。
<br>
- URL 必须包含使用的协议，在本例中为HTTP。这很重要且必需。如果传递了错误的协议，扫描将失败。
<br>
- 在 URL 的主机部分，可以填写 IP 或 HOSTNAME。但是，需要注意的是，使用 IP 时，可能会指向与预期不同的网站。Web 服务器可以使用一个 IP 托管多个网站（此技术也称为虚拟托管）。如果想确保万无一失，请使用 HOSTNAME。
<br>
- Gobuster 不会递归枚举。因此，如果结果显示您感兴趣的目录路径，则必须枚举该特定目录。
<br>
- -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt配置Gobuster使用directory-list-2.3-medium.txt单词表进行枚举。单词表的每个条目都附加到配置的 URL。
<br>
- -r 配置Gobuster遵循从发送的请求收到的重定向响应。如果收到状态代码 301，Gobuster将导航到响应中包含的重定向 URL。

-x标志来指定我们要枚举的文件类型：
```
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js
```
此命令将使用单词列表directory-list-2.3-medium.txt查找位于http://example. thm 的目录。除了目录列表之外，此命令还列出所有具有 . php或 .js 扩展名的文件。



![task4](/img/user/images/tryhackme-Gobuster：基础知识/task44.png)


第一问就是用--no-tls-validation来跳过

第二问 就是枚举这个网站的目录
```
gobuster dir -u www.offensivetools.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r
```

![task4](/img/user/images/tryhackme-Gobuster：基础知识/task4.png)

第三问也是一样 枚举js文件 用-x参数

```
gobuster dir -u http://www.offensivetools.thm/secret -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .js
```
![task4](/img/user/images/tryhackme-Gobuster：基础知识/flag.png)

![task4](/img/user/images/tryhackme-Gobuster：基础知识/flag1.png)

## task5
dns模式。此模式允许Gobuster暴力破解子域名。在渗透测试期间，检查目标顶级域名的子域名至关重要。

| short flags  | long flags           | 描述 |
|------|--------------|------|
| -c   | --show-cname | 显示 CNAME 记录（不能与标志一起使用 -i）。 |
| -i   | --show-ips   | 包括此标志显示域和子域解析到的 IP 地址。 |
| -r   | --resolver   | 此标志配置用于解析的自定义 DNS 服务器。 |
| -d   | --domain     | 此标志配置您想要枚举的域。 |

如何使用dns模式
要在dns模式下运行Gobuster，请使用以下命令语法：
```
gobuster dns -d example.thm -w /path/to/wordlist
```
请注意，除了关键字之外，该命令还包含标志-d和-w。这两个标志是Gobuster 子域名枚举工作所必需的。

看一个如何使用Gobuster dns模式枚举子域名的示例：
```
gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```
gobuster dns枚举配置域上的子域。

-d example.thm将目标设置为example.thm域。

-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt将单词列表设置为 s ubdomains-top1million-5000.txt。Gobuster使用此列表的每个条目构建新的DNS查询。如果此列表的第一个条目是“all”，则查询将是all.example. thm。

来练练手

![task5](/img/user/images/tryhackme-Gobuster：基础知识/task5.png)

第一问 还要加一个-d参数

第二问

```
gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```
其实在这里有一个重复的 一直没发现 总共四个子域

![task5](/img/user/images/tryhackme-Gobuster：基础知识/dns.png)

## task6
最后是vhost模式。此模式允许 Gobuster 暴力破解虚拟主机。虚拟主机是同一台机器上的不同网站。有时，它们看起来像子域名，但不要被欺骗！虚拟主机基于 IP 并在同一台服务器上运行。子域名在 DNS 中设置。vhost和dns模式之间的区别在于 Gobuster 扫描的方式：

- vhost模式将导航到由配置的 HOSTNAME（-u 标志）与单词列表条目组合创建的 URL。
  
- dns模式将对通过将配置的域名（-d 标志）与单词列表的条目相结合而创建的 FQDN进行DNS查找
  
| short flags  | long flags              | 描述 |
|------|------------------|------|
| -u   | --url           | 指定强制虚拟主机名的基本 URL（目标域）。 |
|      | --append-domain | 将基本域附加到单词表中的每个单词（例如，word.example.com）。 |
| -m   | --method        | 指定用于请求的 HTTP 方法（例如 GET、POST）。 |
|      | --domain        | 将域名附加到每个单词列表条目以形成有效的主机名（如果没有明确提供则很有用）。 |
|      | --exclude-length | 根据响应主体的长度排除结果（有助于过滤掉不需要的响应）。 |
| -r   | --follow-redirect | 遵循 HTTP 重定向（对于子域名可能重定向的情况很有用）。 |

如何使用vhost模式
要以模式运行Gobustervhost，请输入以下命令：

gobuster vhost -u "http://example.thm" -w /path/to/wordlist

请注意，除了关键字之外，该命令还包含标志-u和-w。这两个标志是 Gobuster vhost 枚举工作所必需的。让我们看一个如何使用 Gobuster模式枚举虚拟主机的实际示例：

```
gobuster vhost -u "http://10.10.15.13" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 
```

此命令比基本命令语法复杂得多。它包含更多配置标志。在实际测试中，情况通常如此，具体取决于要测试的域的基础设施是如何设置的。

Gobuster将发送多个请求，每次都会更改Host:请求的一部分。Host:此示例中的值为www.example.thm。我们可以将其分为三个部分 ：

www：这是子域名。这是 Gobuster 将用配置的单词表的每个条目填充的部分。
.example：这是二级域名，可以通过--domainflag来配置（需要和顶级域名一起配置）。
.thm：这个是顶级域名，可以通过--domainflag来配置（需要和二级域名一起配置）

---
Gobuster如何发送请求，让我们分解命令并更仔细地检查每个标志：

gobuster vhost指示Gobuster枚举虚拟主机。
-u "http://10.10.15.13"将要浏览的 URL 设置为 10.10.15.13。

-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt配置 Gobuster 使用subdomains-top1million-5000.txt单词表。Gobuster 将单词表中的每个条目附加到配置的域。如果没有使用标志明确配置域--domain，Gobuster将从 URL 中提取它。例如，test.example.thm、help.example.thm等。如果发现任何子域，Gobuster将在终端中向您报告它们。

--domain example.thmHostname:将请求部分中的顶级域名和二级域名设置为example.thm。

--append-domain将配置的域附加到单词表中的每个条目。如果未配置此标志，则设置的主机名将为www、blog等。这将导致命令无法正常工作并显示误报。

--exclude-length过滤我们从发送的 Web 请求中获得的响应。使用此标志，我们可以过滤掉误报。如果您在不使用此标志的情况下运行命令，会注意到您会得到很多误报，例如“找到：Orion.example.thm状态：404 [大小：279]”或“找到：pm.example.thm状态：404 [大小：276]”。这些误报通常具有相似的响应大小，因此我们可以使用它来过滤掉大多数误报。

![task6](/img/user/images/tryhackme-Gobuster：基础知识/task6.png)

```
gobuster vhost -u "http://10.10.15.13"--domain offensivetools.thm -w /usr/sharewordlists/SecLists/Discovery/DNSsubdomains-top1million-5000.txt--append-domain --exclude-length 250-320 
```
域名加ip防止gobuster找错地方 

![task6](/img/user/images/tryhackme-Gobuster：基础知识/vhost.png)

gobuster简单介绍到这