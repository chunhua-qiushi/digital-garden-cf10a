---
{"dg-publish":true,"permalink":"/思/Silver Platter/","tags":["tryhackme","靶场","oscp"]}
---


这个靶场考验的是信息收集 和漏洞利用 会不会去找poc来利用 

信息收集就是在找到网页的登录页面 用目录扫描也只能扫到一些没用的 在blog中刚好提到了一嘴文件路径 这个有点考究

漏洞利用 登录界面有个漏洞用法就是bp抓包 改他的验证 从而登录到admin用户的账户

在检查邮件也是用到poc了

这个靶场主题是服务器 在/var/log/auth.log中有可能会有账户密码 这个是我没想到的