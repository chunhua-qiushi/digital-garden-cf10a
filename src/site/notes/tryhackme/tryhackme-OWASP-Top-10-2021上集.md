---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-OWASP-Top-10-2021上集/","title":"tryhackme OWASP Top 10 - 2021上集","tags":["oaasp","漏洞","tryhackme"]}
---

tryhackme的OWASP Top 10 的题目和记录

OWASP Top 10 

访问控制失效
加密失败
注入
不安全的设计
安全配置错误
易受攻击和过时的组件
识别和认证失败
软件和数据完整性故障
安全日志和监控失败
服务端请求伪造（SSRF）

## 1.访问控制失效

攻击者可能通过绕过访问控制机制，获取未授权的资源或执行未授权的操作。

IDOR 或不安全的直接对象引用是指一种访问控制漏洞，利用该漏洞您可以访问通常无法看到的资源。当程序员公开直接对象引用时就会发生这种情况，它只是指向服务器内特定对象的标识符。我们所说的对象可能是指文件、用户、银行应用程序中的银行账户或任何其他对象。

![task1](/img/user/images/tryhackme-OWASP-Top-10-2021/task1.png)

我们进入题目要求的网站 用给的账户登录一下 、


登进去后会就是显示普通的文本 但观察网址会发现后面有个note_id=1这个字样 从零开始输 看看有没有可能出flag
第一个就出flag了

![task1](/img/user/images/tryhackme-OWASP-Top-10-2021/flag.png)


## 2 加密失败

加密故障是指因误用（或未使用）用于保护敏感信息的加密算法而产生的任何漏洞。Web 应用程序需要加密技术来为其用户提供多层次的机密性。

加密失败的第二部分 我们简单学习了一下下载数据库和查找数据库的有用信息 还有一个等下用得着的网站[crackstation](https://crackstation.net/)

![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/task2.png)

进入网站查看有什么问题吗

![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/web.png)

看看网站源码
![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/webdb0.png)

发现有很多都是来自assets 在网址后面加个/assets试试看

![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/webdb.png)

看到最后一个数据库文件 把他下过来。然后查看一下数据库里有什么重要文件

![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/webdb0.5.png)


![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/webdb1.png)

能找到admin的密码的hash值 拉到上面说的网站里面解密能知道明文密码

![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/webdb3.png)

在网站中登录admin账户就能发现flag

![task2](/img/user/images/tryhackme-OWASP-Top-10-2021/login.png)


## 3 注入
包括sql注入 命令注入

![task3](/img/user/images/tryhackme-OWASP-Top-10-2021/task3.png)

这个thm讲的是命令注入

先输入$(ls)试试水

![task3](/img/user/images/tryhackme-OWASP-Top-10-2021/drpepper.png)


试试看$(cat /etc/passwd | grep "usr")查一下第二题说的用户

![task3](/img/user/images/tryhackme-OWASP-Top-10-2021/shell.png)

但发现都是在/sbin中 都属于root用户 所以说是零个用户 用户shell就是/sbin/shell

查应用程序以什么用户身份运行？直接whoami

![task3](/img/user/images/tryhackme-OWASP-Top-10-2021/whoami.png)

发现是apache用户

查版本就 cat /etc/os-release

![task3](/img/user/images/tryhackme-OWASP-Top-10-2021/os-rease.png)

是3.16.0版本

## 4 不安全的设计
不安全的设计是指应用程序架构固有的漏洞。这些漏洞不是由于糟糕的实现或配置而导致的，而是整个应用程序（或其中的一部分）背后的理念从一开始就存在缺陷。

![task4](/img/user/images/tryhackme-OWASP-Top-10-2021/task4.png)

进入网站 直接开始重置账户 选中间那个最喜欢的颜色
红黄蓝绿一直输 发现是绿色 重置成功。登录进去就能发现flag

![task4](/img/user/images/tryhackme-OWASP-Top-10-2021/green.png)

![task4](/img/user/images/tryhackme-OWASP-Top-10-2021/greenflag.png)

![task4](/img/user/images/tryhackme-OWASP-Top-10-2021/greenflag1.png)


## 5 安全配置错误
安全配置错误与其他十大漏洞不同，因为它们发生在安全配置本来可以正确配置但实际上却没有配置的情况下。即使您下载了最新的软件，错误的配置也可能使您的安装变得脆弱。


先进入网站尝尝~~咸蛋~~咸淡
![task5](/img/user/images/tryhackme-OWASP-Top-10-2021/console.png)

我们直接复制thm给出来的代码 在这里面跑跑

![task5](/img/user/images/tryhackme-OWASP-Top-10-2021/tododb.png)

能发现数据库名字是todo.db

根据thm的提示 我们稍微改改他给出来的命令 就能查出flag

```
import os; print(os.popen("cat app.py | grep 'flag'").read())
```

![task5](/img/user/images/tryhackme-OWASP-Top-10-2021/py.png)


## 6 易受攻击和过时的组件
版本不及时更新的话容易遇到一下漏洞攻击

进入网站看看 诶呦是个线上书店 直接把cse bookstore去谷歌搜索一下看有什么漏洞吗

挺好 找到一个

![task](/img/user/images/tryhackme-OWASP-Top-10-2021/looudong.png)

那他下载过来 直接跑一遍 漏洞利用成功，cat一下/opt/flag.txt 就能就能获得flag

![task](/img/user/images/tryhackme-OWASP-Top-10-2021/books.png)

## 7 身份识别和认证失败

身份验证和会话管理是现代 Web 应用程序的核心组件。身份验证允许用户通过验证其身份来访问 Web 应用程序。最常见的身份验证形式是使用用户名和密码机制。用户输入这些凭据，然后服务器验证它们。如果它们正确，服务器将向用户的浏览器提供会话 cookie 。需要会话 cookie 是因为 Web 服务器使用HTTP (S) 进行通信，而 HTTP (S) 是无状态的。附加会话 cookie 意味着服务器将知道谁在发送什么数据。然后服务器可以跟踪用户的操作。

身份验证机制中的一些常见缺陷包括：

- 暴力攻击
- 使用弱凭证
- 弱会话 Cookie。 

看看题目

![task7](/img/user/images/tryhackme-OWASP-Top-10-2021/task7.png)

进入网站看看 说是要先注册一个darren的账户 

你会发现注册darren的话 会说明账户已存在 

在注册账户的时候 在darren的d的前面加一个空格再来照常注册 发现注册成功 

用刚才的账户密码就能登录darren账户找到flag

![task7](/img/user/images/tryhackme-OWASP-Top-10-2021/login1.png)

arthur也是一样的操作

![task7](/img/user/images/tryhackme-OWASP-Top-10-2021/arthur.png)


![task7](/img/user/images/tryhackme-OWASP-Top-10-2021/arthur1.png)


## 软件和数据完整性故障

此漏洞源自使用未使用任何完整性检查的软件或数据的代码或基础架构。由于未进行完整性验证，攻击者可能会修改传递给应用程序的软件或数据，从而导致意外后果。此类别中主要有两种类型的漏洞：

- 软件完整性故障
- 数据完整性故障

### 软件完整性故障
来看看thm的题目 这里讲的是 jQuery
如果你愿意，你可以直接从他们的服务器将 jQuery 包含在你的网站中，而无需实际下载它，只需在你网站的 HTML 代码中包含以下行：
```
<script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>
```
当用户导航到您的网站时，其浏览器将读取其 HTML 代码并从指定的外部源下载 jQuery。

如果攻击者以某种方式入侵 jQuery 官方存储库，他们可以更改内容``https://code.jquery.com/jquery-3.6.1.min.js`` 以注入恶意代码。

题目

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/task8.0.png)

将https://code.jquery.com/jquery-1.12.4.min.js转成SHA-256的形式

[来这里](https://www.srihash.org/)

```
https://code.jquery.com/jquery-1.12.4.min.js
```

进入网站就能转化成SHA-256 这个就答案

### 数据完整性故障

通常，当用户登录应用程序时，他们会被分配某种会话令牌，该令牌需要在会话持续期间保存在浏览器中。此令牌将在每个后续请求中重复，以便 Web 应用程序知道我们是谁。这些会话令牌可以有多种形式，但通常通过 Cookie 分配。Cookie是Web 应用程序将存储在用户浏览器中的键值对，并且将在每次向发出它们的网站发出请求时自动重复。

如果您正在创建一个 Web 邮件应用程序，您可以在登录后为每个用户分配一个包含其用户名的 Cookie。在后续请求中，您的浏览器将始终在 Cookie 中发送您的用户名，以便您的 Web 应用程序知道哪个用户正在连接。

因此如果用户篡改 Cookie 并更改用户名，他们可能会冒充其他人并阅读他们的电子邮件！此应用程序将遭受数据完整性故障，因为它信任攻击者可以篡改的数据。

解决此问题的一个方法是使用某种完整性机制来保证 cookie 未被用户更改。为了避免重复工作，我们可以使用一些允许您执行此操作的令牌实现，并处理所有加密以提供完整性证明，而无需您为此操心。JSON Web 令牌 (JWT)就是这样一种实现。

看题目

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/task8.png)

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/task8.1.png)

进入网站 guest随便输入一次 报错就知道密码了

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/cookie.png)

进去之后f12找出jwt

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/cookieflag.png)

把hs256改成none 用户名改成admin

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/jwt.png)

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/jwt1.png)

改好的这两个参数重新转化为base64

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/jwt2.png)

![task8](/img/user/images/tryhackme-OWASP-Top-10-2021/jwt3.png)

把这个粘贴在一起 用.间隔。最后面也要用.结尾

把jwt代替之前的jwt 刷新一下就有flag了

![flag](/img/user/images/tryhackme-OWASP-Top-10-2021/cookieflag1.png)


## 9 安全日志和监控失败
设置 Web 应用程序时，应记录用户执行的每个操作。记录很重要，因为一旦发生事件，攻击者的活动就会被追踪。一旦追踪到他们的行为，就可以确定他们的风险和影响。如果没有记录，就无法知道攻击者在获得特定 Web 应用程序的访问权限后执行了哪些操作。


日志中存储的信息应包括以下内容：
- HTTP状态代码
- 时间戳
- 用户名
- API端点/页面位置
- IP 地址

下载日志看看

![task9](/img/user/images/tryhackme-OWASP-Top-10-2021/task9.png)

![task9](/img/user/images/tryhackme-OWASP-Top-10-2021/log.png)

看到连续几个相同的ip盲猜就是攻击者ip

这样看的话就是暴力破解

## 10 服务器端请求伪造（SSRF）

这种类型的漏洞发生在攻击者可以强迫 Web 应用程序代表他们向任意目的地发送请求，同时控制请求本身的内容时。SSRF漏洞通常出现在我们的 Web 应用程序需要使用第三方服务的实现中。

![task10](/img/user/images/tryhackme-OWASP-Top-10-2021/task10.png)

依旧进入网站 发现是个类似博客的东西 周围找找看有什么可以做的吗

![task10](/img/user/images/tryhackme-OWASP-Top-10-2021/admin.png)

点进去发现 要localhost才能进去

![task](/img/user/images/tryhackme-OWASP-Top-10-2021/localhost.png)

第二问 问点击下载按钮。服务器参数指向哪里？

直接f12 找到下载按钮 发现了下载路径

![task](/img/user/images/tryhackme-OWASP-Top-10-2021/down.png)

这个路径也有大作用 正常来说我们下载那个pdf使用这个路径

```
http://10.10.234.129:8087/download?server=secure-file-storage.com:8087&id=75482342
```

我们要把后面的server=的值改成attackbox或者是你虚拟机连openvpn的ip 其他都不动

然后打开终端 设置监听 

```
nc -lvnp 8087
```

启动完就把改好的网址在浏览器那搜索一下 终端就能找到flag

![ask](/img/user/images/tryhackme-OWASP-Top-10-2021/api1.png)

于是乎 把thm的owasp top-10简单介绍一下了 过段时间继续学习更新owasp top-1o！！！

























































































































































































