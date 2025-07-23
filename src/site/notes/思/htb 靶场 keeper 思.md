---
{"dg-publish":true,"permalink":"/思/htb 靶场 keeper 思/","tags":["靶场","hackthebox","oscp"]}
---


keeper 用了一个是 putty 和 keepassxc 的工具来破解。先是脚本提取出. dmp 文件中的密码，再用这个密码到 keepassxc 中解密数据库，找到 ssh 私钥，puttygen 工具做成私钥。登录就完事了
工具是个很奇怪的问题

##note

