---
{"dg-publish":true,"permalink":"/思/GoldenEye-v1/","tags":["oscp","vulnhub","靶场"]}
---



1.nmap扫描端口 访问发现就只有一个登录页面 

2.上文件路径爆破 爆破出一个js文件 访问发现告诉我们一个密码 加上文件中给的用户名 登录进去网站

3.告诉我们有个pop3服务器 结合js文件中的命令进行爆破 爆破出账号密码 访问 检查邮件

4.邮件中有一个账户密码和一个路径

5.访问那个路径 会发现是个网站 找的过程中找到评论 又发现用户名 

6.接着爆破出这个用户名的pop3的账户密码 从中找到了这个用户的网站账户密码 

7.登录进去能找到一个路径和照片 照片分离不出文件 exiftool查看一下 发现描述是个base64编码 解密得出一个类似密码的东西 

8.在主页找到用户名叫admin 用这个密码来试试 能成功

9.登录进去admin 需要找到执行反弹shell的地方 

10.发现能改环境的路径 长得也蛮像反弹shell 我们修改一下 改成反弹shell

11.又找到个地方 本来的拼写检查 里面有个shell的选项 把他换上

12.新建blog执行shell 反弹成功

13.内核提权 注意这里没有gcc需要用cc编译 里面的gcc也要改一下