---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-John-the-Ripper-The-Basics/","title":"tryhackme-John the Ripper: The Basics","tags":["#tryhackme","#渗透工具"]}
---

时隔多年 总算继续我们的tryhackme之旅了 我们来到剪刀手约翰 john the ripper这个程序

这个专题我们要：
- 破解 Windows 身份验证哈希
{ #4b4fca}

- 破解/etc/shadow哈希
- 破解受密码保护的 Zip 文件
- 破解受密码保护的 RAR 文件
- 破解SSH密钥


## task2
来介绍一下hash 哈希

哈希是一种任意长度的数据转化为另一种固定数据的方式。哈希值是通过对原始数据进行哈希算法运算获得的。目前存在许多流行的哈希算法，例如 MD4、MD5、SHA1 

我们将ikun转化为hash就会变成"cd71184f9989c66b41df91c756ec5312" 而我们把zhendeshiniya转化为hash就会变成"7aa58fbe618489683b951bd6f78ffdc2" 同样都是32位的MD5hash 即使他们原本是不同的长度

hash是个单向函数,有密码的哈希版本，并且知道哈希算法，则可以使用该哈希算法对大量单词（称为字典）进行哈希处理。然后，将这些哈希与要破解的哈希进行比较，看看它们是否匹配。如果匹配，就知道哪个单词与该哈希相对应。这称之为字典攻击

而john就是一个对各种类型的hash破解的工具

task2的问题 john的最受欢迎拓展版本是jumbo john

## task3
在/usr/share/wordlists中能找到很多的单词本

问题
在rockyou.com中泄露出来的rock.txt

## task4
### 用法

1. john option file path
- option:选项

- file path:要破解的hash文件路径


2. john --wordlist=[path to wordlist] path to file
- --wordlist:选用的单词模式
- path to file:正在使用的单词列表的路径

eg:
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

3. john --format=[format] --wordlist=[path to wordlist] [path to file]

特定格式破解
一旦确定了要处理的哈希值，就可以告诉 John 在使用以下语法破解提供的哈希值时使用它：

- --format=：告诉john一个特定格式的哈希值，并使用以下格式来破解它
  
- [format]：哈希的格式

eg：

```
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

### 识别hash
thm中推荐了[hash-identifier](https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master)

启动hash-identifier

```
 python3 hash-id.py
```

来看看task4的问题

![task4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task4ask.png)

进入task4文件夹之后分别cat查看一下hash1.txt到hash4.txt

![taask4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task4.png)

```
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt

#format=raw-md5 这个是指定哈希算法为md5
```

![taask4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task41.png)

剩下三个其实原理差不多 都是找出hash算法 然后用john去破解

hash2

![taask4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task4 hash2.png)

```
 john --format=Raw-SHA1 --wordlist=/usr/share/wordlists/rockyou.txt hash2.txt
```

![taask4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/kangeroo.png)


hash3

![taask4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/sha256.png)

```
 john --format=Raw-SHA256 --wordlist=/usr/share/wordlists/rockyou.txt hash3.txt
```

![taask4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/mircophone.png)

hash4

```
john --list=formats | grep -iF "whirlpool"

john --format=whirlpool --wordlist=/usr/share/wordlists/rockyou.txt hash4.txt
```

![task4](/img/user/images/tryhackme-John-the-Ripper-The-Basics/hash4po.png)

## task5
破解Windows身份验证哈希

```
john --format=nt --wordlist=/usr/share/wordlist/rockyou.txt ntlm.txt

# --format=nt 是Windows的哈希算法
```

![task5](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task5.png)

## task6
破解 /etc/shadow 哈希

破解这个需要把密码和/etc/passwd文件结合起来 要用到一个工具 unshadow

unshadow [path to passwd] [path to shadow]

eg:

```
unshadow local_passwd local_shadow > unshadowed.txt
```

来看看task6的问题来练练手

![task6](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task6.png)

```
unshadow local_passwd local_shadow > unshadowed.txt

# unshadowed.txt 这个可以你自己取名字

john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt

# unshadowed.txt 就是上文设置的名字
```

![task5](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task6 root.png)

## task7
单一破解模式

在此模式下，John 仅使用用户名中提供的信息，通过稍微更改用户名中包含的字母和数字来尝试启发式地找出可能的密码。

eg：

- Markus1, Markus2, Markus3 
- MArkus, MARkus, MARKus 
- Markus!, Markus$, Markus

john --single --format=[format] [path to file]

--single：此标志让 John 知道你想使用单一哈希破解模式
--format=[format]：一如既往，确定正确的格式至关重要。
eg：

```
john --single --format=raw-sha256 hashes.txt
```

看看task7的题目

![task7](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task7.png)

```
vim hash7.txt

joker:7bf6d9bb82bed1302f331fc6b816aada

# 后面一串hash值是本来在task07里面的文件 ca就能查看到hash值

#把这个写入hash7.txt

john --single --format=raw-md5 hash7.txt
```

![task7](/img/user/images/tryhackme-John-the-Ripper-The-Basics/joker.png)

## task8
自定义修饰
修饰符：

- Az：获取单词并将其与你定义的字符附加在一起
- A0：获取单词并在其前面添加您定义的字符
- c：按位置将字符大写

也遵循双引号内的修饰符模式" "。以下是一些常见示例：

- [0-9]：将包括数字 0-9
- [0]：仅包含数字 0
- [A-z]：将包括大写和小写
- [A-Z]：仅包含大写字母
- [a-z]：仅包含小写字母

这个是task8的问题和答案

![task8](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task8.png)

## task9
破解受密码保护的 Zip 文件

zip2john工具将 Zip 文件转换为 John 可以理解并有望破解的哈希格式。主要用法如下：

```
zip2john [options] [zip file] > [output file]

# [option]：允许您传递特定的校验和选项zip2john；这通常不是必要的
# [zip file]：你希望获取哈希值的 Zip 文件的路径
# >：将此命令的输出重定向到另一个文件
# [output file]：这是存储输出的文件
```

eg:
```
zip2john zipfile.zip > zip_hash.txt
```

来练练手
![task9](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task9a.png)

```
zip2john secure.zip > zip.txt

# 将zip转成john能理解的形式

john --wordlist=/usr/share/wordlists/rockyou.txt zip.txt

# 用john破解
```

![taks9](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task9.png)

然后找到找到破解完的文件就能查看到flag

## task10
破解受密码保护的RAR文件

Rar2John
与该工具几乎相同zip2john，将使用该rar2john工具将RAR文件转换为 John 可以理解的哈希格式。

语法:
```
rar2john [rar file] > [output file]

# rar2john：调用rar2john工具
# [rar file]：你希望获取哈希值的 RAR 文件的路径
# >：将此命令的输出重定向到另一个文件
# [output file]：这是存储命令输出的文件
```

eg:
```
/opt/john/rar2john rarfile.rar > rar_hash.txt

# 表示rar2john工具存放在/opt/john/目录下
```

来看看task10的问题

![task10](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task10a.png)

```
rar2john secure.rar > file.txt

john --wordlist=/usr/share/wordlists/rockyou.txt file.txt
```

![task10](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task10.png)
## task11
john破解ssh秘钥

SSH2John用于登录 SSH 会话的私钥ssh2john转换为 John 可以使用的哈希格式。
eg:
```
ssh2john [id_rsa private key file] > [output file]

# ssh2john：调用ssh2john工具
# [id_rsa private key file]：你希望获取哈希值的 id_rsa 文件的路径
# >：这是输出目录。我们用它将这个命令的输出重定向到另一个文件。
# [output file]：这是将存储输出的文件
```
示例
```
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```

来练练手
![task11](/img/user/images/tryhackme-John-the-Ripper-The-Basics/task11.png)

```
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt

john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```
![task11](/img/user/images/tryhackme-John-the-Ripper-The-Basics/mango.png)

john的简单用法就介绍到这里 后面还会有更多补充