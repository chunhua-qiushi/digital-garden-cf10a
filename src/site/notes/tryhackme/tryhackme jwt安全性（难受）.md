---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme jwt安全性（难受）/"}
---


基于令牌的会话管理是一个相对较新的概念。它没有使用浏览器的自动 cookie 管理功能，而是依赖于客户端代码来完成该过程。身份验证后，Web 应用程序会在请求正文中提供令牌。然后使用客户端 JavaScript 代码，此令牌将存储在浏览器的 LocalStorage 中。

它通过 Authorization： Bearer 标头传递

## api的例子
```
对于身份验证，可以发出以下 cURL 请求：

curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "passwordX" }' http://10.10.248.106/api/v1.0/exampleX
```

```
对于用户验证，可以发出以下 cURL 请求：

curl -H 'Authorization: Bearer [JWT token]' http://10.10.248.106/api/v1.0/example2?username=Y

[JWT token] 组件必须替换为从第一个请求收到的 JWT。在这种情况下，Y 可以是 user 或 admin，具体取决于您的权限。
```

## JWT Structure  JWT 结构
JWT 由三个组件组成，每个组件都由 Base64Url 编码并由点分隔：
### 标头 
标头通常指示令牌的类型（即 JWT）以及使用的签名算法。
    
### 有效负载 
有效负载是包含声明的令牌的正文。索赔是为特定实体提供的一条信息。在 JWT 中，有已注册的声明，这些声明是由 JWT 标准预定义的声明以及公共或私有声明。public 和 private 声明是由开发人员定义的声明。了解公共索赔和私人索赔之间的区别是值得的，但不是为了安全目的，因此这不会是我们在这个房间里的重点。
    
### 签名 
签名是令牌的一部分，它提供验证令牌真实性的方法。签名是使用 JWT 标头中指定的算法创建的。让我们深入了解一下主要的签名算法。

## 签名算法
### 无 
无算法表示不用于签名的算法。实际上，这是一个没有签名的 JWT，这意味着无法通过签名验证 JWT 中提供的声明的验证。

### 对称签名 
对称签名算法（如 HS265）通过在生成哈希值之前将密钥值附加到 JWT 的标头和正文来创建签名。签名验证可以由任何知道密钥的系统执行。

### 非对称签名
非对称签名算法（如 RS256）通过使用私钥对 JWT 的标头和正文进行签名来创建签名。这是通过生成哈希值，然后使用私钥加密哈希值来创建的。签名验证可以由任何系统执行，只要系统知道与用于创建签名的私钥关联的公钥。

## JWT 可以加密（称为 JWE）
## 敏感信息泄露
### 练习1
让我们看一个实际示例。让使用以下 cURL 请求对 API 进行身份验证：

```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password1" }' http://10.10.248.106/api/v1.0/example1
```

![image.png](https://s2.loli.net/2025/05/12/vj3H5iy4wdTrNfB.png)


![image.png](https://s2.loli.net/2025/05/12/AyYv3BKjOcV9eM5.png)

### 修复
不应将密码或标志等值添加为声明，因为 JWT 将发送到客户端。

## 签名验证错误
### 未验证签名
签名验证的第一个问题是没有签名验证。如果服务器不验证 JWT 的签名，则可以将 JWT 中的声明修改为您希望的任何值。虽然不执行签名验证的 API 并不常见，但 API 中的单个端点可能省略了签名验证。根据终端节点的敏感度，这可能会对业务产生重大影响。

### 练习2
让我们对 API 进行身份验证：

```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password2" }' http://10.10.248.106/api/v1.0/example2
```

通过身份验证后，让我们验证我们的用户：

```
curl -H 'Authorization: Bearer [JWT Token]' http://10.10.248.106/api/v1.0/example2?username=user
```

但是，让我们尝试在没有签名的情况下验证我们的用户，删除 JWT 的第三部分（只留下点）并再次发出请求。这意味着签名未被验证。将有效负载中的 admin 声明修改为 1，并尝试以 admin 用户身份进行验证以检索您的标志。


![image.png](https://s2.loli.net/2025/05/16/4gQLBG25DhMP9Nk.png)


![image.png](https://s2.loli.net/2025/05/16/QpY4GFgdjerN2wv.png)


#### 修复
降级为无

一个常见问题是签名算法降级。JWT 支持 None 签名算法，这实际上意味着 JWT 不使用签名。虽然这听起来很愚蠢，但标准中这背后的想法是用于服务器到服务器的通信，其中 JWT 的签名在上游进程中得到验证。因此，不需要第二个服务器来验证签名。但是，假设开发人员不锁定签名算法，或者至少拒绝 None 算法。在这种情况下，您只需将 JWT 中指定的算法更改为 None，这将导致用于签名验证的库始终返回 true，从而允许您再次伪造令牌中的任何声明。

JWT 支持 None 算法，该算法跳过签名验证。如果开发人员没有限制算法，攻击者可以将 JWT 标头中的 alg 声明更改为 None，从而完全绕过签名检查

### 练习三
向 API 进行身份验证以接收您的 JWT，然后验证您的用户。要执行此攻击，您需要手动将标头中的 alg 声明更改为 None。您可以使用 CyberChef 使用 URL 编码的 Base 64 选项。再次提交 JWT 以验证它是否仍被接受，即使签名不再有效，因为已进行更改。然后，您可以更改 admin 声明以恢复标志

![image.png](https://s2.loli.net/2025/05/16/2TFejqg7AudpmNv.png)

![image.png](https://s2.loli.net/2025/05/16/6SsfOP5jncxipJ8.png)


![image.png](https://s2.loli.net/2025/05/16/6dbyNAieLmghZvC.png)


如果应支持多个签名算法，则应将支持的算法作为数组列表提供给 decode 函数
![image.png](https://s2.loli.net/2025/05/16/vLf3gSYZxeM9XWU.png)


### 练习四
弱对称密钥
当使用弱 JWT 密钥时，会出现此问题。当开发人员匆忙或从示例中复制代码时，通常会发生这种情况。
![image.png](https://s2.loli.net/2025/05/16/y1Dxfzqr52pRoiW.png)

![image.png](https://s2.loli.net/2025/05/16/KFYHzUlGf2PBACL.png)

#### 修复
应选择 Secure secret 值。由于此值将在软件中使用，而不是由人类使用，因此应该使用一个长而随机的字符串作为密钥

## 练习五
JWT 的第二个常见错误是没有正确验证签名。如果签名未正确验证，威胁行为者可能能够伪造有效的 JWT 令牌以访问其他用户的帐户。
### 签名验证错误

签名验证的第一个问题是没有签名验证。如果服务器不验证 JWT 的签名，则可以将 JWT 中的声明修改为希望的任何值。虽然不执行签名验证的 API 并不常见，但 API 中的单个端点可能省略了签名验证。根据终端节点的敏感度，这可能会对业务产生重大影响。

![image.png](https://s2.loli.net/2025/07/16/EJnlWBDiuqtLPvj.png)

![image.png](https://s2.loli.net/2025/07/16/mdLXxWzBbpNIR9A.png)

我们要把 jwt 的第二个 payload 中的 0 改成 1，最后一个部分删掉，只保留一个.

![image.png](https://s2.loli.net/2025/07/16/jSylndw93iOXpqU.png)

```
用我们改好的token，先使用user用户进行测试，发现是无效请求
curl -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.ewogICJ1c2VybmFtZSI6ICJ1c2VyIiwKICAiYWRtaW4iOiAxCn0=.' http://10.10.114.65/api/v1.0/example2?username=user

用户改成admin发现成功获得flag
curl -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.ewogICJ1c2VybmFtZSI6ICJ1c2VyIiwKICAiYWRtaW4iOiAxCn0=.' http://10.10.114.65/api/v1.0/example2?username=admin

```

![](https://s2.loli.net/2025/07/16/X9e561YCciO4RfJ.png)

### 开发错误
在此示例中，未验证签名，如下所示：

```
payload = jwt.decode (token, options={'verify_signature': False})
```

虽然在普通 API 上很少看到这种情况，但它经常发生在服务器到服务器的 API 上。如果威胁行为者可以直接访问后端服务器，则可以伪造 JWT。

#### 修复

应始终验证 JWT，或者应使用其他身份验证因素（例如证书）进行服务器到服务器通信。可以通过提供密钥（或公钥）来验证 JWT，如以下示例所示：

```
payload = jwt.decode (token, self. secret, algorithms="HS 256")
```

### 降级为无
另一个常见问题是签名算法降级。JWT 支持 None 签名算法，这实际上意味着 JWT 不使用签名。虽然这听起来很愚蠢，但标准中这背后的想法是用于服务器到服务器的通信，其中 JWT 的签名在上游进程中得到验证。因此，不需要第二个服务器来验证签名。但是，假设开发人员不锁定签名算法，或者至少拒绝 None 算法。在这种情况下，只需将 JWT 中指定的算法更改为 None，这将导致用于签名验证的库始终返回 true，从而允许您再次伪造令牌中的任何声明。

向 API 进行身份验证以接 JWT，然后验证您的用户。要执行此攻击，需要手动将标头中的 alg 声明更改为  None。

![image.png](https://s2.loli.net/2025/07/16/uR5dMwPVm2CoXFr.png)

![image.png](https://s2.loli.net/2025/07/16/8fp54ewQdBGsUHa.png)

将这两个伪造的 payload 拼接到原来的 payload 处, 获得 flag
![image.png](https://s2.loli.net/2025/07/16/NPHtBADJgoFX6KO.png)

### 弱对称密钥
如果使用对称签名算法，则 JWT 的安全性取决于所用密钥的强度和熵。如果使用了弱密钥，则可以通过离线破解来恢复 secret。知道密钥值后，可以再次更改 JWT 中的声明，并使用密钥重新计算有效签名。

获得 token 之后用 hashcat 破解

![image.png](https://s2.loli.net/2025/07/16/76UaMWItgdx3fXe.png)

发现密钥是 secret, 用之前的 token 稍微改一下，把 0 改成 1
![image.png](https://s2.loli.net/2025/07/16/WOtAZufClqHM43I.png)

获得 flag
![image.png](https://s2.loli.net/2025/07/16/C6nzLlWawFZ2XNo.png)

### 签名算法混淆
签名验证的最后一个常见问题是何时可以执行算法混淆攻击。这类似于 None 降级攻击，但是，它特别发生在对称签名算法和非对称签名算法之间的混淆中。如果使用非对称签名算法（例如 RS 256），则可以将算法降级为 HS 256。在这些情况下，某些库将默认使用公钥作为对称签名算法的密钥。由于公钥是已知的，因此您以将 HS256 算法与公钥结合使用来伪造有效的签名。

第五问跑脚本就能解决


## JWT 生命周期
在验证 Token 的签名之前，需要计算 Token 的生命周期，确保 Token 没有过期。这通常是通过从令牌中读取 exp （expiration time） 声明并计算令牌是否仍然有效来执行的。

一个常见的问题是，如果 exp 值设置得太大（或根本没有设置），则令牌的有效期会太长，甚至可能永远不会过期。跟 cookies，则 cookie 可以在服务器端过期。但是，JWT 不会内置了相同的功能。如果我们想在 exp time，我们必须保留这些令牌的黑名单，打破使用同一身份验证服务器的去中心化应用程序的模式。因此，根据应用程序的功能，应注意选择正确的 exp 值。例如，邮件服务器和银行应用程序之间可能使用不同的 exp 值。

```
使用未过期的payload可以获得flasg

curl -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6MX0.ko7EQiATQQzrQPwRO8ZTY37pQWGLPZWEvdWH0tVDNPU' http://10.10.87.36/api/v1.0/example6?username=admin
```

![image.png](https://s2.loli.net/2025/07/16/z9ORDCmybK56fpU.png)


## 跨服务中继攻击
常见错误配置是 Cross-Service 错误配置。如前所述，JWT 通常用于具有为多个应用程序提供服务的集中式身份验证系统的系统。但是，在某些情况下，我们可能希望限制使用 JWT 访问哪些应用程序，尤其是当存在仅应对某些应用程序有效的声明时。这可以通过使用 audience 声明来完成。但是，如果受众声明未正确实施，则可以执行跨服务中继攻击来执行权限提升攻击

JWT 可以具有受众声明。在单个身份验证系统为多个应用程序提供服务的情况下，audience 声明可以指示 JWT 适用于哪个应用程序。但是，此受众声明的实施必须在应用程序本身上进行，而不是在身份验证服务器上进行。如果此声明未得到验证，因为 JWT 本身仍通过签名验证被视为有效。

如果用户在某个应用程序上具有管理员权限或更高角色。分配给用户的 JWT 通常具有指示此内容的声明，例如 “admin” ： true。但是，同一用户可能不是同一身份验证系统提供的不同应用程序上的 admin。如果第二个应用程序（也使用其 admin 声明）未验证 audience 声明，则服务器可能会错误地认为该用户具有 admin 权限。这称为跨服务中继攻击

```
获得A服务的 token
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password7", "application" : "appA"}' http://10.10.87.36/api/v1.0/example7

获得B服务的token
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password7", "application" : "appB"}' http://10.10.87.36/api/v1.0/example7
```

![image.png](https://s2.loli.net/2025/07/16/6WPzvKx9cVDl8rX.png)

![image.png](https://s2.loli.net/2025/07/16/KbQfPY2oJTEWjBN.png)

![image.png](https://s2.loli.net/2025/07/16/V8UI5TaEHdCtJBS.png)


![image.png](https://s2.loli.net/2025/07/16/pbcCJlG4jYHi7Kd.png)


我们利用 a 的 jwt的中间部分的 payload 拼接到 b 的 jwt 中
```
curl -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6InVzZXIiLCJhZG1pbiI6MSwiYXVkIjoiYXBwQiJ9.jrTcVTGY9VIo-a-tYq_hvRTfnB4dMi_7j98Xvm-xb6o' http://10.10.87.36/api/v1.0/example7_appA?username=admin
```


获得 flag
![image.png](https://s2.loli.net/2025/07/16/V6DAU34JX9ek27a.png)
