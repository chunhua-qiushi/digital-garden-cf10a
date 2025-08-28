---
{"dg-publish":true,"permalink":"/渗透靶场/玄机靶场/玄机 渗透靶场 configconfig/"}
---


## 端口扫描
![image.png](https://s2.loli.net/2025/08/28/V8HSaTghInfp59A.png)

找到 8081 的 web 端口和 2222 ssh 端口。这个是题目说的

## 目录扫描
![image.png](https://s2.loli.net/2025/08/28/LIsi41cw5jEr9D2.png)

但是 config 还是重定向到原页面
![image.png](https://s2.loli.net/2025/08/28/iT8CxstwyrcMLof.png)

要考虑目录遍历了，试了/../和../都是没有反应。我只能看一下 wp

发现是/config../config. txt
![image.png](https://s2.loli.net/2025/08/28/Gm2sec8fjTowY14.png)

问了 ai
**URL 中的拼接效果​**​：
当Nginx配置使用 `root` 指令时（如 `root /var/www;`），请求 `/config../config.txt` 会被解析为：`/var/www/config../config.txt` → 实际指向 `/var/www/config.txt`（因为 `../` 回退到 `/var/www/`）

Nginx 默认会对请求路径进行标准化处理，将 `../`和 `./`转换为实际路径。例如：
`/config../config.txt`→ 标准化为 `/config.txt`（假设 `config`是目录）


说明一个问题 nginx 的路径校验不严格，而且还有敏感文件泄露。

![image.png](https://s2.loli.net/2025/08/28/SbfDUTYplyrm9Zu.png)

登录系统成功

## 提权
输入！/bin/bash 逃逸
![image.png](https://s2.loli.net/2025/08/28/CWZ9rljyPkoQ8Kf.png)

逃逸成功

获得 flag
flag{user-530773 d 6-5951-11 f 0-89 d 9-836 ccaf 94 d 6 b}
![image.png](https://s2.loli.net/2025/08/28/viYrjgFB1Pc2XAC.png)

sudo -l 查看权限
![image.png](https://s2.loli.net/2025/08/28/wE3OlsxQt4BY82R.png)

![image.png](https://s2.loli.net/2025/08/28/9PCQaB8t652nJsq.png)


![image.png](https://s2.loli.net/2025/08/28/sI1u7p8nYUZdoCy.png)

发现占用了，改个端口

![image.png](https://s2.loli.net/2025/08/28/V9CdPjeqoY8mlgw.png)

浏览器中查看

![image.png](https://s2.loli.net/2025/08/28/6S1GMvBqDdN53gI.png)

找到 root 的 flag
flag{root-bf 116 e 68-5953-11 f 0-b 06 c-63 e 27 ce 93 d 04}

主要是看 nginx 的路径遍历加上敏感文件泄露。ssh 连接进去之后 bash 逃逸成功，又发现有 root 权限的文件可以执行