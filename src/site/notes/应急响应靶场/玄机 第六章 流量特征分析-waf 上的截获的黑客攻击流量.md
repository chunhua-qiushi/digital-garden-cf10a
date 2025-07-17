---
{"dg-publish":true,"permalink":"/应急响应靶场/玄机 第六章 流量特征分析-waf 上的截获的黑客攻击流量/","tags":["打靶","应急响应"]}
---

![image.png](https://s2.loli.net/2025/06/02/M2GYmVTakeDUK1b.png)



![image.png](https://s2.loli.net/2025/06/02/8uePdHb2O9fBVrv.png)

就是找 post 包中提交的密码，从最后一个往前试，成了。

登录成功之后，会发现攻击者在上传一句话木马

![image.png](https://s2.loli.net/2025/06/02/bvizPFkIU6Amo5B.png)

只能慢慢筛选了

http. request. method == "POST"

但是太过于麻烦。需要一直输入这个搜素式

换一个方案

http. request. uri == "/images/article/a.php"

将与这个有关的所有流量保存在另一个流量包中

![image.png](https://s2.loli.net/2025/06/02/PfA5vaEQWTiX3G2.png)


http contains "dbpass"

找了很久，可以找流量包中包含 dbpass 字样的内容

![image.png](https://s2.loli.net/2025/06/02/CGO75hTnj638cSX.png)



