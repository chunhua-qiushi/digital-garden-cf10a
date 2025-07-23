---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme- Bricks-Heist-教程/","title":"Bricks Heist 教程","tags":["linux","#oscp","#tryhackme","靶场"]}
---

### 祝师傅们2025越来越好 技术越来越牛 

# Bricks Heist教程
我们看一下Bricks Heist 这个简单挑战
![title](/img/user/images/Bricks-Heist-教程/title.png)

这个是题目

![title](/img/user/images/Bricks-Heist-教程/ask.png)

我们先把自己的机子ip和bricks.thm绑到/etc/hosts里面

![hosts](/img/user/images/Bricks-Heist-教程/hosts.png)

连接上openvpn之后咱正式开搞

## 第一步
先nmap开扫 可以看到他们开放了22，80和443端口

![nmap](/img/user/images/Bricks-Heist-教程/nmap.png)

![nmap](/img/user/images/Bricks-Heist-教程/duankou.png)

[[tips和一些工具/linux工具#^8da186\|linux工具#^8da186]]
[[tips和一些工具/linux工具#^077f81\|linux工具#^077f81]]
## 第二步
我们在浏览器那搜索ip地址和域名 看一下有什么结果吗

直接输ip的话是啥也没有

![ip](/img/user/images/Bricks-Heist-教程/ip.png)

我们换成域名试一试 发现成功了

![brick](/img/user/images/Bricks-Heist-教程/brick.png)


## 第三步
我们把这个拉到wpscan里面扫描一下

```
wpscan --url https://bricks.thm/ --disable-ssl-checks
```

为什么要加--disable-ssl-checks这个参数捏 不加的话他会报错 说目标网站的 SSL/TLS 证书不正确或无法验证

![scan](/img/user/images/Bricks-Heist-教程/scan1.png)

<strong>--disable-ssl-checks</strong> 参数可以让你在面对无效证书（如自签名证书、过期证书等）时继续进行扫描，跳过 SSL/TLS 证书验证检查。

扫描结果我们可以发现在robots.txt中有两个可疑的路径

![scan](/img/user/images/Bricks-Heist-教程/wpscan.png)

[[tips和一些工具/linux工具#wpscan\|linux工具#wpscan]]

第二个呢 就是wordpess的主题 bricks和主题版本1.9.5

![scan](/img/user/images/Bricks-Heist-教程/wpscan3.png)

结合这两个信息 咱可以在浏览器搜搜有什么漏洞吗 成功发现一个 把他的python下载到机子里面

![loudong](/img/user/images/Bricks-Heist-教程/ludong.png)

## 第四步
我们跑一下这个文件 就可以获得这个网站的shell 然后查看题目需要的文件是否存在

```
python3 CVE-2024-25600.py -u https://bricks.thm

```
**-u** / --url：指定目标 URL。用于设置你要扫描的 Web 地址。

![loudong](/img/user/images/Bricks-Heist-教程/flag.png)

刚进去就发现一个可疑文件 直接cat查看一下 发现第一问解出来了 flag到手

第二题看可疑进程和进程的服务名

我们先**ps aux**查看一下 但发现太多了 我们进一步筛选 查找一下正在运行中的服务

```
systemctl | grep running
```

![run](/img/user/images/Bricks-Heist-教程/jingcheng.png)

我们在里面发现了一个可疑进程 然后systemctl查一下具体信息

![run](/img/user/images/Bricks-Heist-教程/jingcheng1.png)

![run](/img/user/images/Bricks-Heist-教程/jiingcheng3.png)

就发现了可疑进程是nm-inet-dialog 服务名叫做ubuntu.service

## 第五步
要看日志文件叫什么名字 在刚才的查询中发现这个服务在/lib/networkmanager的inet.conf

```
head /lib/networkmanager/inet.conf
```

![head](/img/user/images/Bricks-Heist-教程/inet.png)

发现id是个复杂的加密 把这串id复制到cyberchef中 这个id进行了十六进制转换 再来两次base64的转换

![cyber](/img/user/images/Bricks-Heist-教程/bitcoiin.png)

得出来的结果分成一般就是我们需要的钱包地址

## 最后一步

我们知道比特币地址之后 在www.blockchain.com中搜索一下地址 查询一下他的交易流程 发现一个地址频繁出现 我们把这个地址复制 搜索一下 

![bit](/img/user/images/Bricks-Heist-教程/bit.png)

搜完之后 随便点了一个链接 得知是lockbit组织

![bit](/img/user/images/Bricks-Heist-教程/lockbit.png)


这个练习挺好玩的 用到了wpscan nmap和反向提权 其实在python的那一步 反向提权一下会更方便一点 可能我不太熟练吧 就直接这么查找需要的东西了













