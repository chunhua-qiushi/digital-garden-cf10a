---
{"dg-publish":true,"permalink":"/应急响应靶场/应急响应-vulntarget-k-01/","tags":["应急响应"]}
---

## 第一问
![image.png](https://s2.loli.net/2025/07/15/NglE5LA3QVyR6z4.png)

我们用 fscan 扫描一下端口
![image.png](https://s2.loli.net/2025/07/15/DIyHevR84CfXwdq.png)

会发现有个 8081 和 9999，分别试一下，发现是 9999 端口攻击进去的

访问 9999 端口会发现是个xxl-job，github 找一下有无利用的漏洞
![image.png](https://s2.loli.net/2025/07/15/QfEFnK9CHmeARSB.png)


用 xxl-job 的漏洞检测工具能找到可以利用的漏洞
![image.png](https://s2.loli.net/2025/07/15/AH52bUW73NDPQFv.png)




