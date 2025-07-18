---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-密码学基础/","title":"tryhackme 密码学基础","tags":["密码学"]}
---


话说回来 来到了密码学专区了

![title](/img/user/images/tryhackme-密码学基础/title1.png)

密码学在生活中无处不在

# cyptography basics
来看看密码学的一些术语
- 明文(Plaintext)是加密前的原始可读消息或数据。它可以是文档、图像、多媒体文件或任何其他二进制数据。
  
- 密文(Ciphertext)是加密后的消息的混乱且不可读的版本。理想情况下，我们无法获得有关原始明文的任何信息，除了其大致大小。
  
- 密码(Cipher)是一种将明文转换为密文并再转换回明文的算法或方法。密码通常由数学家开发。
  
- 密钥(Key )是密码用于加密或解密数据的一串位。一般来说，使用的密码是公开的；但是，除非密钥是非对称加密中的公钥，否则它必须保密。我们将在后面的任务中讨论非对称加密。
  
- 加密(Encryption)是使用密码和密钥将明文转换为密文的过程。与密钥不同，密码的选择是公开的。
  
- 解密(Decryption)是加密的逆过程，使用密码和密钥将密文重新转换为明文。尽管密码是公开的，但在不知道密钥的情况下恢复明文应该是不可能的（不可行）

来看看加密和解密的图解

此为加密
![task](/img/user/images/tryhackme-密码学基础/en.png)


此为解密
![task](/img/user/images/tryhackme-密码学基础/de.png)

## task3

看两个问题

![task3](/img/user/images/tryhackme-密码学基础/task3.png)

第一个 毋庸置疑 密文(ciphertext)

第二个 转化为明文就是解密

## task4
凯撒密码 顾名思义就是凯撒搞出来的加密方法 将每个字母移动一定数字来加密消息

分为明文 偏移量 密码 
举例：
- 假设明文为ikun
- 偏移量为3(像右3个字母)
- 密码为lnxq

假设知道密文和偏移量 也能倒推出明文

来试试看吧

![task4](/img/user/images/tryhackme-密码学基础/task4.png)

可以试试这个[网站](https://www.dcode.fr/caesar-cipher) 相信很多师傅都有这种密码破解的 一下子梭出来了

![task](/img/user/images/tryhackme-密码学基础/depy.png)

第一个就是答案

## 加密类型
分为对称加密和非对称加密

对称加密，使用相同的密钥来加密和解密数据，

特点：
- 密钥相同：发送和接收双方共享一个密钥，因此加密和解密过程都依赖于相同的密钥。
- 速度较快：对称加密算法通常计算速度较快，适用于加密大量数据。
- 密钥管理问题：因为密钥是共享的，密钥的分发和管理需要非常小心，密钥一旦泄漏，所有加密的数据都可能被破解

常用的对称加密DES 3DES AES RC4

![task](/img/user/images/tryhackme-密码学基础/duicheng.png)

非对称加密
又称公钥加密
非对称加密使用一对密钥：公钥（Public Key）和 私钥（Private Key）。公钥用于加密，私钥用于解密。加密和解密使用不同的密钥。

- 加密：发送方使用接收方的公钥对数据进行加密。
- 解密：接收方使用自己的私钥对数据进行解密。

特点：
- 密钥不同：加密和解密使用不同的密钥，其中公钥可以公开，而私钥必须保密。
- 安全性高：即使公钥被泄露，也无法通过公钥解密数据，只有私钥能解密。
- 速度较慢：由于非对称加密算法的计算复杂度较高，因此相对于对称加密，非对称加密速度较慢，通常用于加密少量数据，如传输密钥。

常见的非对称加密有RSA DSA Diffie-Hellman ecc

## 基础计算
异或计算

XOR是“异或”的缩写，是二进制算术中的逻辑运算，在各种计算和加密应用中起着至关重要的作用。在二进制中，XOR比较两个位，如果位不同则返回 1，如果位相同则返回0，


我们要对二进制数 1010 和 1100 进行异或运算。在这种情况下，我们逐位执行操作：1 ⊕ 1 = 0、0 ⊕ 1 = 1、1 ⊕ 0 = 1 和 0 ⊕ 0 = 0，结果为 0110。

模运算 

步骤：
- 除法运算：将 a 除以 b。
- 求余数：计算除法后的余数。

例子 2：15 % 4
除法运算：15 ÷ 4 = 3，商是 3。
求余数：15 - (4 × 3) = 15 - 12 = 3，所以 15 % 4 = 3。

![task6](/img/user/images/tryhackme-密码学基础/task6.png)

异或计算就是每个相对应的位数 分别计算 相同为0 不同为1

1⊕1=0 
0⊕0=0
0⊕1=1
1⊕0=1

第一问答案就是0011

第二问可以到[这里](https://www.wolframalpha.com/)计算

第三问 因为60可以与12整除 余数为0

# Public Key Cryptography Basics
![task](/img/user/images/tryhackme-密码学基础/public key/public key.png)

网络通信中要注意
- 身份验证：确保与正确的人进行交流，而不是与其他人假装的交流。
- 真实性：可以验证信息是否来自所声称的来源。
- 完整性：必须确保没有人更改您交换的数据。
- 保密性：想防止未经授权的一方窃听您的谈话。

这个章节主要是讲公钥加密 也就是非对称加密

## task2
非对称加密其实就有点像你放了一个信到一个上锁的信箱里面。每个人都可以放信进去 但是只有你才能打开信箱的锁拿出信来看内容

公钥就是信箱的投递口
私钥就是取信的钥匙

## task3
来认识认识rsa

rsa的计算过程
1. Bob 选择了两个素数：p  = 157和q  = 199。他计算出n  =  p  ×  q  = 31243。
2. 假设ϕ ( n ) =  n  −  p  −  q  + 1 = 31243 − 157 − 199 + 1 = 30888，Bob 选择e  = 163 ，使得e与ϕ ( n )互质；此外，他选择d  = 379，其中e  ×  d  = 1 mod ϕ ( n )，即e  ×  d  = 163 × 379 = 61777且61777 mod 30888 = 1。公钥为( n , e )，即(31243,163)，私钥为 $(n,d) ，即(31243,379)。
3. 假设他们想要加密的值是x  = 13，那么 Alice 将计算并发送y  =  x e mod n  = 13 163 mod 31243 = 16341。
4. Bob 将通过计算x  =  y d mod n  = 16341 379 mod 31243 = 13来解密收到的值。这样，Bob 就恢复了 Alice 发送的值。



看看问题

![task3](/img/user/images/tryhackme-密码学基础/public key/task3.png)

也是醉了 出了个寂寞

第一问就是4391*6659=29239669

第二问就是29239669-4391-6659+1=29228620

分别是第一步和第二步的开头


## task4 
Diffie-Hellman Key Exchange 简称就叫dh协议 密钥交换

密钥交换旨在在双方之间建立共享秘密。这是一种允许双方在非安全通信信道上建立共享秘密的方法，无需预先存在共享秘密，观察者也无法获取此密钥。因此，此共享密钥可用于后续通信中的对称加密。

研究一下具体的过程。

步骤 1: 公共材料
Alice 和 Bob 确定一个公共的素数p 和一个公共的生成器g，这两者对双方都是公开的。假设我们选择：
p = 29（一个素数）
g = 3（生成器）
这些值是公开的，任何人都可以知道它们。

步骤 2: Alice 和 Bob 各自选择私钥
Alice 选择一个私钥 a，并且不能告诉别人。假设 Alice 选择了私钥 a = 13。
Bob 选择一个私钥 b，并且也不会告诉别人。假设 Bob 选择了私钥 b = 15。
这些私钥是保密的，只由 Alice 和 Bob 知道。

步骤 3: 计算公钥
根据 Diffie-Hellman 的规则，每个人使用自己的私钥来计算公钥：

Alice 的公钥：A = g^a mod p
A = 3^13 mod 29
计算 3^13 mod 29：
3^13 = 1594323，
然后 A mod p 
1594323 mod 29 = 19
所以 Alice 的公钥是 A = 19。

Bob 的公钥：B = g^b mod p
B = 3^15 mod 29
计算 3^15 mod 29：
3^15 = 14348907，
然后 B mod p 
 14348907 mod 29 = 26
所以 Bob 的公钥是 B = 26。

步骤 4: Alice 和 Bob 交换公钥
Alice 发送自己的公钥 A = 19 给 Bob。
Bob 发送自己的公钥 B = 26 给 Alice。
这时，Alice 和 Bob 都拥有对方的公钥。

步骤 5: 计算共享密钥
Alice 和 Bob 使用自己私有的密钥 的公钥来计算共享密钥：

Alice 的计算：
Alice 收到 Bob 的公钥 B = 26，然    算 B^a mod p：
B^a mod p = 26^13 mod 29
计算 26^13 mod 29：
26^13 = 10460353203，然后 104603    mod 29 = 10
所以 Alice 得到的共享密钥是 10。    

Bob 的计算：
Bob 收到 Alice 的公钥 A = 19，然    算 A^b mod p：
A^b mod p = 19^15 mod 29
计算 19^15 mod 29：
19^15 = 41821194261，然后 418211    mod 29 = 10
所以 Bob 得到的共享密钥是 10。

步骤 6: 完成
最终，Alice 和 Bob 都得到了相同 密钥 10，并且可以用它来进行后续的加密通信。

看看图解

![dh](/img/user/images/tryhackme-密码学基础/public key/dh.png)


来练练手

![dh](/img/user/images/tryhackme-密码学基础/public key/task4.png)

三四问可以在[这里](https://www.wolframalpha.com/)进行计算

第一问
p=29
g=5
a=12

A = g^a mod p
A = 5^12 mod 29
A = 244140625 mod 29 = 7

第二问 一样
b=17 
其他都一样
B = g^b mod p
B = 5^17 mod 29 = 9

第三问
已知p  = 29，a  = 12，并且从第二个问题得到B，那么 Bob 计算出的密钥是什么？ 

key = B^a mod p 
key = 9^ 12 mod 29 =24

第四问
已知p  = 29，b  = 17，并且从第一个问题得到A，那么 Alice 计算出的密钥是什么？

key = A^b mod p
key = 7^17 mod 29 =24


## task5
讲的是ssh秘钥


![task5](/img/user/images/tryhackme-密码学基础/public key/rsa.png)

![task5](/img/user/images/tryhackme-密码学基础/public key/task5.png)

## task6
数字签名和证书
数字签名提供了一种验证数字消息或文档真实性和完整性的方法。数字签名和物理签名​​具有相同的法律效力。

证书是公钥加密技术的一个重要应用，它们也与数字签名相关。它们最常用于 HTTPS

证书有一个信任链，从根 CA（证书颁发机构）开始。

问题就跳过了 文章都是可以找到的

## task7
PGP和GPG
PGP代表 Pretty Good Privacy。它是一种实现加密的软件，用于加密文件、执行数字签名等。GnuPG或GPG是 OpenPGP 标准的开源实现。

看看问题

![task77](/img/user/images/tryhackme-密码学基础/public key/task77.png)

需要我们导入密钥 然后解密就可以了

![task7](/img/user/images/tryhackme-密码学基础/public key/task7.png)

```
gpg --import tryhackme.key
gpg --decrypt message.gpg
```

到此第二部分也简单介绍到这里

# Hashing Basics
哈希值是由哈希函数计算的固定大小的字符串或字符。哈希函数接受任意大小的输入并返回固定长度的输出，即哈希值。

我们也能通过对比hash值来判断下载的副本是否与原始文件完全相同

## task2
### 什么是哈希函数

哈希函数不同于加密。它没有密钥，并且不可能（或计算上不切实际）从输出返回到输入。输出具有固定大小。

### 什么是哈希碰撞

哈希碰撞是指两个不同的输入产生相同的输出。

### 题
来看看题

![task2](/img/user/images/tryhackme-密码学基础/hash/task2.png)

第一问问照片的sha256值

```
sha256sum passport.jpg
```
来获得照片的sha256的值

第二问
md5能输出一个128位(16字节)的散列值 答案就是16

第三问
八位hash输出 可以当成2^8=256

## task3
身份验证的密码存储不安全

密码方面的三种不安全做法：
- 以明文形式存储密码
- 使用已弃用的加密方式存储密码
- 使用不安全的哈希算法存储密码

这个问就是找密码

![task3](/img/user/images/tryhackme-密码学基础/hash/task3.png)

问第20个密码是啥 直接head 查到第二十个就行了

```
head -n 20 rockyou.txt
```

![task3](/img/user/images/tryhackme-密码学基础/hash/mima.png)

## task4
使用哈希存储密码

Rainbow Table 彩虹表(彩虹表是哈希到明文的查找表) 因此您可以从哈希中快速找出用户刚刚拥有的密码。

[crackstation](https://crackstation.net/) 和[hashes.com](https://hashes.com/en/decrypt/hash)能给我们帮助
内部使用大量彩虹表来提供快速破解无盐哈希密码的功能。

这个时候要提一嘴了 什么是加盐

加盐是指在加密过程中，向原始数据（通常是密码）中添加一个随机生成的字符串（即“盐”），然后再进行加密。

这个随机字符串通常是唯一的，并且与原始数据一起存储。 加盐的目的是增加加密数据的复杂性，使得即使两个相同的密码，其加密结果也会不同。

![task4](/img/user/images/tryhackme-密码学基础/hash/task4.png)

问题一
我们能在上面的表上查到

![task4](/img/user/images/tryhackme-密码学基础/hash/caihong.png)

问题二

我们把hash值复制到[hashes.com](https://hashes.com/en/decrypt/hash)能破解出结果

![task4](/img/user/images/tryhackme-密码学基础/hash/tryhackme.png)


## task5
识别密码哈希值

在Linux上，密码哈希存储在 中/etc/shadow，通常只有 root 才能读取。它们过去存储在 中/etc/passwd，每个人都可以读取。

该shadow文件包含密码信息。每行包含九个字段，以冒号 ( :) 分隔。前两个字段是登录名和加密密码。

加密密码字段包含哈希密码，该密码由四个部分组成：前缀（算法 ID）、选项（参数）、盐和哈希。

![task5](/img/user/images/tryhackme-密码学基础/hash/task5.png)

第一问遇事不决直接百度 能查出是256的大小

第二问和第三问 我们在[Hashcat 示例](https://hashcat.net/wiki/doku.php?id=example_hashes)能找到我们要的答案

第二问

![task5](/img/user/images/tryhackme-密码学基础/hash/2410.png)

![task5](/img/user/images/tryhackme-密码学基础/hash/scrypt.png)

## task6
密码破解

可以在[hashcat](https://hashcat.net/wiki/doku.php?id=example_hashes)和[John the Ripper](https://www.openwall.com/john/)这两个工具上

第一个最好在主机上搞 榨干全部性能

[哈希分析](https://www.tunnelsup.com/hash-analyzer/)

来试试手

![task6](/img/user/images/tryhackme-密码学基础/hash/task6.png)

第一问
我们用john来破解 
$2a$就是bcrypt
```
john hash.txt -format=bcrypt -wordlist=/usr/share/wordlists
```
![task6](/img/user/images/tryhackme-密码学基础/hash/850.png)

能破解出85208520

第二 三 四问 我们都可以在[这里](https://hashes.com/en/decrypt/hash)去破解

主要是不知道为啥这个机子的john很抽象 就只能在网站那破解

![task6](/img/user/images/tryhackme-密码学基础/hash/holl.png)

![task6](/img/user/images/tryhackme-密码学基础/hash/spaceman.png)

![task6](/img/user/images/tryhackme-密码学基础/hash/funforyou.png)

## task7
![task7](/img/user/images/tryhackme-密码学基础/hash/task7.png)

第一问
```
sha256sum 
```
![task7](/img/user/images/tryhackme-密码学基础/hash/2561.png)

第二问
![task7](/img/user/images/tryhackme-密码学基础/hash/512.png)


## task8
使用base64解码RU5jb2RlREVjb2RlCg==

把这串复制到[cyberchef](https://gchq.github.io/CyberChef/)里面能得出解码：ENcodeDEcode

第三部分就这样结束了 后面还会补充多点内容 