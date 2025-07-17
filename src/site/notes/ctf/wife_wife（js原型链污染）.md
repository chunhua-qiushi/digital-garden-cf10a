---
{"dg-publish":true,"permalink":"/ctf/wife_wife（js原型链污染）/"}
---

![image.png](https://s2.loli.net/2025/06/23/tAcn54VNFHI9erK.png)


抓包发现是有个 isadmin 的值，改这个值的思路也是没毛病的。

![image.png](https://s2.loli.net/2025/06/23/kvMqhXVz3f295lR.png)

这里要用到 js原型链污染
```
这个是payload
{"__proto__":{"isAdmin":true}
```
![image.png](https://s2.loli.net/2025/06/23/Y4hKTPfmdM17kQq.png)

获得 flag

![image.png](https://s2.loli.net/2025/06/23/Jyd8coRCVrBbDYA.png)
