---
{"dg-publish":true,"permalink":"/思/Weasel/","tags":["靶场","tryhackme","oscp"]}
---


有意思的一点就是smb服务找到所有文件 从中找到token 登录到jupyter 利用python反弹shell

在获得linux的shell后提权 复制/bin/bash到能利用的变量中 从而获得root的shell

#注意 这里遇到一个mnt挂载的知识 就是我们最开始是获得了wsl的权限 在wsl中挂载c盘