---
{"dg-publish":true,"permalink":"/应急响应靶场/玄机 哥斯拉ekp版本流量分析/","tags":["应急响应","靶场"]}
---


![image.png](https://s2.loli.net/2025/06/03/DwJ2zxh7VZ8fHae.png)


## 第一问
![image.png](https://s2.loli.net/2025/06/03/NsihyGw8XFruz9t.png)

在导出对象中发现. index. jsp。这个就是隐藏的 jsp 木马

flag{. index. jsp}


## 第二问
ssh 登录之后，搜索. index. jsp

![image.png](https://s2.loli.net/2025/06/03/VwD2UMAGPHlRtnW.png)


发现密码和解密密钥

flag{mypass}

flag{9adbe0b3033881f8}
