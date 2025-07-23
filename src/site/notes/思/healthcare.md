---
{"dg-publish":true,"permalink":"/思/healthcare/","tags":["靶场","vulnhub","oscp"]}
---

枚举子目录就废了大半时间，接着是利用漏洞获得用户 hash 值，登录 openemr，进行反弹 shell。其实玩了这么多，感觉 php 反弹的地方差不多都是这样，反弹成功之后，就是环境变量劫持，利用脚本中的命令，给他替换掉

##note



