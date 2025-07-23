---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme Active-Directory-Basics教程/","title":"Active Directory Basics教程","tags":["#靶场","ad域","tryhackme"]}
---


Active Directory Basics教程
这个只能说没遇到过 在未来工作管理中还蛮适合的 就是可以管理用户和计算机的好工具 

![ad](/img/user/images/Active-Directory-Basics教程/title.png)

**Active Directory**（活动目录）是微软Windows操作系统中用于存储网络资源信息和管理网络用户权限的一种目录服务。它在企业级网络环境中广泛使用，主要用于集中管理用户账户、计算机、组织单位、组策略等网络资源。

Windows 计算机网络的通用组件的管理集中在一个名为**Active Directory  ( AD )** 的存储库中。运行 Active Directory 服务的服务器称为**Domain Controller 域控制器 ( DC )。**

## Active Directory作用
- 1.**集中管理用户和计算机**：Active Directory可以集中管理网络中的用户账户和计算机。管理员可以通过它创建、修改、删除用户账户和计算机账户，统一管理用户的登录权限和计算机的访问权限。
<br>  
- 2.**组织单位管理**：可以创建组织单位（OU），将用户和计算机按部门、地理位置等进行分类管理，方便对不同组织单位应用不同的策略和管理操作。
<br>
- 3.**组策略应用**：通过Active Directory可以将组策略应用到用户和计算机上。组策略可以控制用户的桌面环境、软件安装、安全设置等，实现对网络环境的统一配置和管理。
<br>
- 4.**资源访问控制**：可以设置用户对网络资源（如文件服务器、打印机等）的访问权限，确保只有授权用户可以访问特定资源。
<br>
- 5.**身份验证和授权**：Active Directory提供身份验证和授权机制，确保用户在登录网络时的身份合法性，并根据用户的权限进行授权操作。


科普完之后咱开始看题
## task2

![task2](/img/user/images/Active-Directory-Basics教程/task2.png)

这一题就是科普题

1.在Windows域中，凭据被存储在一个的集中式存储库 存储库叫什么名字？

Active Directory

2.负责运行Active Directory服务的服务器被称为？

Domain Controller


## task3

先看看怎么打开ad吧 在电脑中找了一波 就是没发现ad在哪。在问过大模型之后 发现AD是运行在Windows Server上的服务。我们电脑应该应该没 真的要这个功能需要下载 搞很多东西 这里就不搞了

这个是ad的默认组
- **域管理员(Domain Admins)**	该组的用户拥有整个域的管理权限。默认情况下，他们可以管理域中的任何计算机，包括 DC。
- **务器操作员(Server Operators)**	此组中的用户可以管理域控制器。他们无法更改任何管理组成员身份。
- **份操作员(Backup Operators)**	此组中的用户可以访问任何文件，而不受其权限的限制。它们用于执行计算机上的数据备份。
- **账户操作员(Account Operators)**	该组中的用户可以创建或修改域中的其他帐户。
- **域用户(Domain Users)**	包括域中所有现有的用户帐户。
- **域计算机(Domain Computers)**	包括域中所有现有的计算机。
- **域控制器(Domain Controllers)**	包括域中所有现有的 DC。

识别机器帐户相对容易。它们遵循特定的命名方案。机器帐户名称是计算机的名称后跟美元符号。例如，名为的机器DC01将有一个名为的机器帐户**DC01$**。

![task](/img/user/images/Active-Directory-Basics教程/task3.png)

我们接着看看task3的题目 依旧科普题

1.**Domain Admins** 该组的用户拥有整个域的管理权限 一般可以直接管理所有计算机

2.**"tom-pc$"**  命名这些机器账户就是在名字后面加个美元符号

第三题就是 organizational units

## task4
![task4](/img/user/images/Active-Directory-Basics教程/task4.png)

我们来看第一问 我们现在windows的搜索框搜一下这个

![task4](/img/user/images/Active-Directory-Basics教程/group.png)

进去之后 我们随便点一下文件 出现以下的框框 点击第一个

![task4](/img/user/images/Active-Directory-Basics教程/ad1.png)

我们点击add 添加philip这个用户(题目提供了这个账户给我们) 给他给别的用户重设密码的权限

![task4](/img/user/images/Active-Directory-Basics教程/ad2.png)

![task4](/img/user/images/Active-Directory-Basics教程/ad3.png)

改完之后我们远程登录phillip ip就是tryhackme提供的ip

![task4](/img/user/images/Active-Directory-Basics教程/rd.png)

![task4](/img/user/images/Active-Directory-Basics教程/login.png)

登录之后我们用powershell来重设shphile的账户 **记住密码要符合一定的复杂程度！**

```
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```


```
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
```

![task4](/img/user/images/Active-Directory-Basics教程/setpasswd.png)

一切就绪 我们开始远程登录

![login](/img/user/images/Active-Directory-Basics教程/sophlogin.png)

进入桌面就能看到flag

![login](/img/user/images/Active-Directory-Basics教程/flag.png)

第二问 向用户授予某些 OU 或其他 AD 对象的权限的过程称为
**delegation**

## task5

![task5](/img/user/images/Active-Directory-Basics教程/task5.png)

第一问挺有意思的 

只能说我一开始是没搞明白啥意思

我们在Windows的搜索栏搜一下users

![task5](/img/user/images/Active-Directory-Basics教程/group.png)

点进去照着图这么设置 显示出所有的东西

![task5](/img/user/images/Active-Directory-Basics教程/fix.png)

找到computer这个组

![task5](/img/user/images/Active-Directory-Basics教程/fix2.png)

将这里的勾去掉

![task5](/img/user/images/Active-Directory-Basics教程/fix3.png)

然后点击thm.local 新建两个文件一个是servers另一个是workstations

![task5](/img/user/images/Active-Directory-Basics教程/fixxx.png)

恭喜~~你被我恭喜到了~~以上是让你熟悉熟悉操作的 打开computer那个组 把s开头的那几个减掉 就是第一题的答案

第二题当然是yay

## task6
Windows 通过**组策略对象** ( GPO )管理此类策略。GPO 只是可应用于 **OU**的设置集合。GPO 可以包含针对用户或计算机的策略，

GPO 通过名为 的网络共享分发到网络**SYSVOL**，该共享存储在 DC 中。域中的所有用户通常都应能够通过网络访问此共享，以定期同步其 GPO。SYSVOL 共享默认指向C:\Windows\SYSVOL\sysvol\我们网络中每个 DC 上的目录。

```
gpupdate /force
``` 

我们看一下题目

![](/img/user/images/Active-Directory-Basics教程/task6.png)

第一题是通过sysvol将gpo分发到域机器

第二题 gpo可以将设置应用在用户和计算机

## task7

身份验证方法

Windows 域中的网络身份验证可以使用两种协议：
**Kerberos**：任何较新版本的 Windows 均使用此协议。这是任何较新域中的默认协议。

**NetNTLM**：为了兼容性目的而保留的旧式身份验证协议。

### Kerberos身份验证

Kerberos身份验证是 Windows 最新版本的默认身份验证协议。使用Kerberos登录服务的用户将被分配票证。将票证视为先前身份验证的证明。拥有票证的用户可以将其出示给服务，以证明他们之前已通过网络身份验证，因此可以使用该服务。

当使用Kerberos进行身份验证时，会发生以下过​​程：

#### 第一步
用户将其用户名和使用从其密码派生的密钥加密的**时间戳**发送到**密钥分发中心 (KDC)**，kdc负责在网络上创建**Kerberos票证**。

**KDC**将创建并返回一个票证授予**票证 ( TGT )**，这将允许用户请求更多票证以访问特定服务。需要票证才能获得更多票证听起来可能有点奇怪，但它允许用户在每次想要连接到服务时请求服务票证而无需传递其凭据。除了 TGT 之外，还会向用户提供会话密钥，他们将需要该密钥来生成以下请求。


#### 第二步

当用户想要连接到网络上的服务（如共享、网站或数据库）时，他们将使用其TGT向 KDC 请求**票证授予服务 (TGS)**。TGS 是仅允许连接到为其创建的特定服务的票证。要请求 TGS，用户将发送使用会话密钥加密的用户名和时间戳，以及 TGT和服务主体名称 (SPN)，后者指示我们打算访问的服务和服务器名称。

因此，KDC 将向我们发送 TGS 以及服务会话密钥，我们需要该密钥来验证我们要访问的服务。TGS 使用从 服务所有者哈希派生的密钥进行加密。服务所有者是服务在其下运行的用户或机器帐户。TGS 在其加密内容中包含服务会话密钥的副本，以便服务所有者可以通过解密 TGS 来访问它。


#### 第三步
然后，TGS 可以发送到所需的服务以进行身份​​验证并建立连接。该服务将使用其配置的帐户的密码哈希来解密 TGS 并验证服务会话密钥。


### NetNTLM 身份验证

NetNTLM 采用质询-响应机制。 整个过程如下：

1.客户端向其想要访问的服务器发送身份验证请求。
2.服务器生成一个随机数并将其作为挑战发送给客户端。
3.客户端将其NTLM密码哈希与质询（和其他已知数据）相结合，以生成对质询的响应并将其发送回服务器进行验证。
4.服务器将质询和响应转发给域控制器进行验证。
5.域控制器使用该质询重新计算响应，并将其与客户端发送的原始响应进行比较。 如果两者匹配，则客户端通过身份验证； 否则，访问被拒绝。 身份验证结果将发送回服务器。
6.服务器将认证结果转发给客户端。

请注意，出于安全考虑，用户的密码（或哈希值）绝不会通过网络传输。

---
任务七的题目在这

![task7](/img/user/images/Active-Directory-Basics教程/task7.png)

第一题 Windows是默认使用**Kerberos** 所以填nay

第二题 当用户想要连接到网络上的服务（如共享、网站或数据库）时，他们将使用其**TGT**向 KDC 请求票证授予服务 (TGS) tgt就是答案

第三题 密码不会通过网络传输 填nay

## task8

trees forests and trusts
### trees

Active Directory 支持集成多个域，这样就可以将网络划分为可以独立管理的单元。如果您有两个共享**相同命名空间**的域，则可以将这些域合并为一个**tree(树)**。

![tree](/img/user/images/Active-Directory-Basics教程/tree.png)

### forests

将具有**不同命名空间**的多个树合并到**同一个网络**中称为**forest森林**。

![forest](/img/user/images/Active-Directory-Basics教程/forest.png)

### trusts

以树和森林形式组织的多个域可让您在管理和资源方面拥有良好的分区网络。但在某个时候，alice的用户可能需要访问bob 服务器之一中的共享文件。为此，以树和森林形式组织的域通过信任关系连接在一起。

还可以建立双向信任关系，以允许两个域相互授权对方的用户。默认情况下，将多个域加入一棵树或一个森林下将形成双向信任关系。

![task8](/img/user/images/Active-Directory-Basics教程/8.png)

第一题就是tree
第二题是a trust relationship   

也算是做完这个了 就是熟悉活动目录 以及对用户和组的一些管理 了解了解一些功能。太太太粪怪了 没多大意思