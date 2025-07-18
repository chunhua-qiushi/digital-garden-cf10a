---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-Web应用程序基础/","title":"tryhackme-Web应用程序基础","tags":["tryhackme"]}
---


来重温和拓展一下web应用程序基础

通过完成这个房间，你将：

- 了解什么是 Web 应用程序以及它如何在 Web 浏览器中运行。
- 分解 URL 的组成部分并了解它如何帮助访问网络资源。
- 了解HTTP请求和响应的工作原理。
- 熟悉不同类型的HTTP请求方法。
- 了解不同HTTP响应代码的含义。
- 检查HTTP标头的工作原理以及它们对安全性的重要性。

## task2

web应用程序分为前段和后端

在这个专题thm用一颗行星来做例子

### 前端
前端 可以被视为类似于地球表面。 Web应用程序可以让用户与其交互，用HTML、CSS和JavaScript等多种技术来实现这一点

HTML（超文本标记语言）是网络应用程序的基础。它是一组指令或代码，用于指示网络浏览器显示什么以及如何显示。

CSS（层叠样式表）描述了标准外观，例如某些颜色、文本类型和布局。

JS  (JavaScript) 是 Web 应用程序前端的一部分，它支持 Web 浏览器中更复杂的活动。

### 后端
数据库 是可以存储、修改和检索信息的地方。

## task3
这个部分讲的是url

![url](/img/user/images/tryhackme-Web应用程序基础/yurl.png)

url有很多部分组成

Scheme
是用于访问网站的协议。最常见的是HTTP （超文本传输​​协议）和HTTPS（超文本传输​​安全协议）。

User
对于需要身份验证的网站，某些 URL 可能包含用户的登录详细信息（通常是用户名）

现在是比较少见的 因为这个有可能泄露一些信息

Host/Domain
主机或域名是 URL 中最重要的部分，因为它会告诉你正在访问哪个网站。

从安全角度来看，请寻找那些看起来与真实域名几乎相同但略有差异的域名（这称为域名抢注（Typosquatting））。这些虚假域名通常用于网络钓鱼攻击，诱骗人们泄露敏感信息。

港口

Port
端口号有助于将浏览器引导至 Web 服务器上的正确服务。

Path
路径指向你尝试访问的服务器上的特定文件或页面。

Query String
查询字符串是 URL 的一部分，以问号 (?) 开头。它通常用于搜索词或表单输入等内容。

Fragment
片段以井号 (#) 开头，有助于指向网页的特定部分，例如直接跳转到特定标题或表格。

这俩个都容易被修改 要多加防范

来看看问题 这些都是在上面能找到的

![task3](/img/user/images/tryhackme-Web应用程序基础/task3.png)

## task4
这个部分讲的是http信息 所谓http消息就是用户（客户端）和 Web 服务器之间交换的数据包。

一般来说有两种类型的信息：

- HTTP请求(HTTP Requests)：由用户发送，以触发 Web 应用程序上的操作。

- HTTP响应(HTTP Responses)：服务器发送以响应用户的请求。

一个信息中会有以下的部分:
Start Line
是消息的介绍。它告诉您正在发送什么类型的消息 - 是来自用户的请求还是来自服务器的响应。此行还提供了有关如何处理消息的重要详细信息。

Headers
由键值对组成，提供有关HTTP消息的额外信息。它们向处理请求或响应的客户端和服务器发出指令。这些标头涵盖各种内容，例如安全性、内容类型等，确保通信一切顺利。

Empty Line
是分隔标头和正文的一条小分隔线。它很重要，因为它显示了标头的结束位置以及消息的实际内容的开始位置。如果没有这个空行，消息可能会变得混乱，客户端或服务器可能会误解它，从而导致错误。

body
是实际数据存储的地方。在请求中，主体可能包含用户想要发送到服务器的数据（如表单数据）。在响应中，主体是服务器放置用户请求的内容（如网页或API数据）的地方。

![task4](/img/user/images/tryhackme-Web应用程序基础/task4.png)

## task5
讲的是HTTP request 

请求行是http请求的第一部分 

用于告知服务器正在处理的请求类型。它主要包含三个部分：HTTP  method、URL 路径和HTTP版本。

eg： METHOD /path HTTP/version

### http method
GET 
从服务器获取数据而不做更改

POST
向服务器发送数据 用于创建或更新内容

PUT
替换或更新服务器上的某些内容。提醒！在接受请求之前，请确保用户有权进行更改。

DELETE
从服务器中删除某些内容。提醒！与 PUT 一样，确保只有授权用户才能删除资源。

除了这些常用方法外，还有一些在特定情况下使用的方法：

PATCH
更新资源的一部分。它适用于进行小改动而无需替换整个资源，但始终要验证数据以避免不一致。

HEAD
工作原理与 GET 类似，但只检索标头，而不是完整内容。它对于检查元数据而不下载完整响应非常方便。

OPTIONS
告诉您特定资源可用的方法，帮助客户端了解他们可以对服务器做什么。

TRACE
与 OPTIONS 类似，它显示允许哪些方法，通常用于调试。许多服务器出于安全原因禁用它。

CONNECT
用于创建安全连接，如 HTTPS。它并不常见，但对于加密通信至关重要。

### url path
URL 路径告诉服务器在哪里可以找到用户请求的资源

对于url path至关重要的是：

- 验证 URL 路径以防止未经授权的访问
- 净化路径以避免注入攻击
- 通过进行隐私和风险评估来保护敏感数据

### http版本
尽管HTTP /2 和HTTP /3 提供了更好的速度和安全性，但许多系统仍在使用HTTP /1.1，因为它得到了良好的支持并且适用于大多数现有设置。但是，随着越来越多的系统采用 HTTP /2 或 HTTP /3，升级到HTTP /2 或HTTP /3 可以显著提高性能和安全性。

![task5](/img/user/images/tryhackme-Web应用程序基础/task5.png)

## task6
Request Headers

| Request Header  | Example                                             | Description                                                  |
|----------------|-----------------------------------------------------|--------------------------------------------------------------|
| **Host**       | `Host: tryhackme.com`                               | 指定请求所针对的 Web 服务器的名称。     |
| **User-Agent** | `User-Agent: Mozilla/5.0`                          | 共享有关请求来自的 Web 浏览器的信息。 |
| **Referer**    | `Referer: https://www.google.com/`                 | 指示请求来自的 URL。        |
| **Cookie**     | `Cookie: user_type=student; room=introtowebapplication; room_status=in_progress` | Web 服务器先前要求 Web 浏览器存储的信息保存在 cookie 中。 |
| **Content-Type** | `Content-Type: application/json`                  | 描述请求中的数据的类型或格式。  |

请求正文
在HTTP请求（例如 POST 和 PUT）中，数据被发送到 Web 服务器，而不是从 Web 服务器请求，数据位于HTTP请求正文中。一些常见的格式是URL Encoded、Form Data、JSON或XML。

URL 编码 (application/x-www-form-urlencoded)
一种数据结构为键和值对的格式

eg:
```
POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US
```
表单数据 (multipart/form-data)
允许发送多个数据块，每个数据块由边界字符串分隔。边界字符串是请求本身定义的标头。这种格式可用于发送二进制数据，例如将文件或图像上传到 Web 服务器时。

eg:
```
POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```

JSON (application/ json )在此格式中，可以使用 JSON
(JavaScript 对象表示法) 结构 发送数据。数据以名称：值对的形式格式化。多个对以逗号分隔，所有对都包含在花括号 { } 中。示例

eg:
```
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62

{
    "name": "Aleksandra",
    "age": 27,
    "country": "US"
}
```

XML (application/ xml )
在 XML 格式中，数据结构化在标签内，标签有开头和结尾。这些标签可以相互嵌套。您可以在下面的示例中看到标签的开头和结尾，以发送有关名为 Aleksandra 的用户的详细信息。

eg:
```
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124

<user>
    <name>Aleksandra</name>
    <age>27</age>
    <country>US</country>
</user>
```

![task6](/img/user/images/tryhackme-Web应用程序基础/task6.png)

## task7
状态行和状态代码

状态行
每个HTTP响应的第一行称为状态行。它为您提供三条关键信息：

HTTP版本：这告诉您正在使用哪个版本的HTTP 。
状态代码：一个三位数字，显示您的请求的结果。
原因短语：以可读的术语解释状态代码的简短消息。  

状态代码和原因短语:

信息响应 (100-199)
这些代码表示服务器已收到部分请求并正在等待其余部分。这是一个“继续”信号。

成功响应（200-299）
这些代码表示一切正常。服务器处理了请求并发回了请求的数据。

重定向消息（300-399）
这些代码告诉您请求的资源已移动到其他位置，通常提供新的 URL。

客户端错误响应（400-499）
这些代码表示请求存在问题。可能是 URL 错误，或者您缺少一些必需的信息，例如身份验证。

服务器错误响应（500-599）
这些代码表示服务器在尝试执行请求时遇到错误。这些通常是服务器端问题，而不是客户端的错误。

常见的 HTTP 状态码：

- 1xx（信息性状态码）：表示接收的请求正在处理。
- 2xx（成功状态码）：表示请求正常处理完毕。
- 3xx（重定向状态码）：需要后续操作才能完成这一请求。
- 4xx（客户端错误状态码）：表示请求包含语法错误或无法完成。
- 5xx（服务器错误状态码）：服务器在处理请求的过程中发生了错误。

![task7](/img/user/images/tryhackme-Web应用程序基础/task7.png)

## task8
这个部分讲的是Response Headers响应标头

![task8](/img/user/images/tryhackme-Web应用程序基础/xiangying.png)

主要是包含一些重要信息

date：
示例：Date: Fri, 23 Aug 2024 10:43:21 GMT
此标题显示服务器生成响应的确切日期和时间。

Content-Type：
示例：Content-Type: text/html; charset=utf-8
它告诉客户端正在获取什么类型的内容，例如它是 HTML、JSON还是其他内容。

server：
示例：Server: nginx
此标头显示哪种服务器软件正在处理请求。

还有其他常见的标头，它们为客户端或浏览器提供额外指令，并帮助控制如何处理响应。

Set-Cookie：
示例：Set-Cookie: sessionId=38af1337es7a8
此方法将 Cookie 从服务器发送到客户端，然后客户端会存储这些 Cookie 并在以后的请求中将其发回。为了确保安全，请确保使用HttpOnly标志（这样 JavaScript 就无法访问它们）和Secure标志（这样它们只能通过 HTTPS 发送）设置 Cookie。

Cache-Control：
示例：Cache-Control: max-age=600
此标头告知客户端在再次与服务器核对之前可以缓存响应多长时间。如果需要，它还可以防止缓存敏感信息（使用no-cache）。

Location：
示例：Location: /index.html
此字段用于重定向 (3xx) 响应。如果资源已移动，它会告诉客户端下一步该去哪里。如果用户可以在请求期间修改此标头，请小心验证和清理它 — 否则，您可能会面临开放的重定向漏洞，攻击者可以将用户重定向到有害网站。

![task8](/img/user/images/tryhackme-Web应用程序基础/task8.png)

## task9
这个部分是讲安全标头Security Headers

将深入研究以下安全标头：

- 内容安全策略（CSP）
- 严格传输安全 (HSTS)
- X-Content-Type 选项
- 引荐来源政策

1.内容安全策略（CSP）
CSP标头是一个额外的安全层，可以帮助缓解跨站点脚本 ( XSS ) 等常见攻击。恶意代码可以托管在单独的网站或域上，并注入易受攻击的网站。CSP为管理员提供了一种方法来说明哪些域或来源被视为安全，并为此类攻击提供了一层缓解措施。

查看CSP标头的示例：

Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'

我们看到了以下用法：

default-src
- 指定自身的默认策略，即仅限当前网站。

script-src
- 指定从何处加载脚本的策略，它本身以及托管在https://cdn.tryhackme.com

style-src
- 指定从当前网站 (自身) 加载样式 CSS 样式表的策略

2.严格传输安全 (HSTS)
HSTS 标头可确保 Web 浏览器始终通过 HTTPS 进行连接。让我们看一个 HSTS 示例：
```
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```
以下是按指令细分的示例 HSTS 标头：

max-age
- 这是此设置的到期时间（以秒为单位）

includeSubDomains
- 一个可选设置，指示浏览器将此设置也应用于所有子域。

preload
- 此可选设置允许将网站包含在预加载列表中。浏览器甚至可以在首次访问网站之前使用预加载列表来强制执行 HSTS。
  
3.X-Content-Type 选项

X-Content-Type-Options 标头可用于指示浏览器不要猜测资源的MIME时间，而仅使用 Content-Type 标头。下面是示例：
```
X-Content-Type-Options: nosniff
```
下面是 X-Content-Type-Options 标头按指令的细分：

nosniff——
- 该指令指示浏览器不要嗅探或猜测MIME类型。

4.引荐来源政策
此标头控制当用户从源 Web 服务器重定向时（例如当他们单击超链接时）发送到目标 Web 服务器的信息量。此标头可用于允许 Web 管理员控制共享哪些信息。以下是 Referrer-Policy 的一些示例：
```
Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
```
以下是根据指令对 Referrer-Policy 标头进行的细分：

no-referrer-
这将完全禁用有关引荐来源的任何信息

same-origin
- 此策略仅在目标属于同一来源时发送引荐来源信息。当超链接位于同一网站内但不在外部网站外时，如果您希望传递引荐来源信息，此策略非常有用。

strict-origin
- 此策略仅在协议保持不变时发送引荐来源作为来源。因此，当 HTTPS 连接转到另一个 HTTPS 连接时，会发送引荐来源。

strict-origin-when-cross-origin
- 这与 strict-origin 类似，但对于同源请求除外，它会在源标头中发送完整的 URL 路径。

![task9](/img/user/images/tryhackme-Web应用程序基础/task9.png)

## task10
小游戏随便玩玩

![task10](/img/user/images/tryhackme-Web应用程序基础/task10.png)

第一问直接输入就能得到flag

![task10](/img/user/images/tryhackme-Web应用程序基础/flag1.png)

点击页面的齿轮 设置一下值 country改成us 

![task10](/img/user/images/tryhackme-Web应用程序基础/flag21.png)

就能获得flag

![task10](/img/user/images/tryhackme-Web应用程序基础/flag2.png)

delete 输入/api/user/1 go一下就能得到flag

![task10](/img/user/images/tryhackme-Web应用程序基础/flag3.png)

thm的web程序应用就简单介绍到此