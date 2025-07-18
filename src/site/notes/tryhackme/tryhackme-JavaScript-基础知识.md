---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-JavaScript-基础知识/","title":"tryhackme-JavaScript 基础知识","tags":["js","tryhackme"]}
---


话说回来 这还是第一次碰碰js 一开始对js的最大印象就是那些高端网站的动效等等。代码审计比这个更难更难 就当是入坑的第一站了

## task2
基本概念

### 变量Variables
变量是允许您在其中存储数据值的容器。与任何其他语言一样，JS 中的变量类似于存储数据的容器。

在 JS 中，每个变量都有一个名称；当我们在变量中存储某个值时，我们会为其分配一个名称以便以后引用它。 var、let 和 const 类型的图像在 JS 中有三种声明变量的方式：var、let和const。

### 数据类型Data Types

在 JS 中，数据类型定义变量可以保存的值的类型。示例包括string(text)、、number( booleantrue/false)、null、undefined 和 object（适用于数组或对象等更复杂的数据）。

### 功能function

函数表示用于执行特定任务的代码块。在函数内部，您可以将需要执行类似任务的代码分组。

### 循环loop

只要条件为true，循环允许多次运行代码块。JS 中常见的循环是for、while,和do...while，它们用于重复任务

![task](/img/user/images/tryhackme-JavaScript-基础知识/task2.png)

loop就是循环 能多次运行代码块

## task3
JavaScript 概述

JS 是一种解释型语言，这意味着代码无需事先编译即可在浏览器中直接执行。

根据文章 我们在浏览器写写代码 试试看

![task](/img/user/images/tryhackme-JavaScript-基础知识/20.png)

![task3](/img/user/images/tryhackme-JavaScript-基础知识/task3.png)

刚才的提升就给出答案了 

## task4

### 内部 JavaScript

内部 JS 是指将 JS 代码直接嵌入 HTML 文档中。

脚本插入在script标签之间。这些标签可以放在head部分内，通常用于需要在呈现页面内容之前加载的脚本，也可以放在body部分内，其中脚本可用于在网页上加载元素时与元素进行交互。

根据文章 我们打开internal.html

![task4](/img/user/images/tryhackme-JavaScript-基础知识/15.png)

![task4](/img/user/images/tryhackme-JavaScript-基础知识/nei.png)

里面的格式是这样的

JS 通过选择一个元素（带有 id="result" 的 <p>）并使用 document.getElementById("result").innerHTML更新其内容来与 HTML 交互

### 外部 JavaScript

外部 JS 涉及在以文件扩展名结尾的单独文件中创建和存储 JS 代码 .js 。此方法可帮助开发人员保持 HTML 文档的整洁有序。外部 JS 文件可以存储或托管在与 HTML 文档相同的 Web 服务器上，也可以存储在外部 Web 服务器（例如云）上。

我们打开文件夹中的script.js和external.html

![task](/img/user/images/tryhackme-JavaScript-基础知识/waibu.png)

所做的不同之处在于使用 script标记中的 src 属性从外部文件加载 JS。当浏览器加载页面时，它会查找 script.js 文件并将其内容加载到 HTML 文档中。这种方法使我们能够将 JS 代码与 HTML 分开，使代码更有条理且更易于维护，尤其是在处理较大的项目时。

### 验证内部或外部 JS

在对 Web 应用程序进行渗透测试时，检查网站使用的是内部 JS 还是外部 JS 非常重要。通过查看页面的源代码可以轻松验证这一点。为此，请打开external_test.html 位于exercise文件夹中的页面Chrome，右键单击页面上的任意位置，然后选择View Page Source。

这将显示渲染页面的 HTML 代码。在源代码中，任何直接在页面上编写的 JS 都将出现在不带属性的script标记之间src。如果您看到带有src属性的script标记，则表示页面正在从单独的文件加载外部 JS。

![task4](/img/user/images/tryhackme-JavaScript-基础知识/task4.png)

内部js就是集成在html文档中

外部js适合跨多个文件重用js

打开这个文档查看源码 就能发现这个是用外部js调用thm_external.js

![task4](/img/user/images/tryhackme-JavaScript-基础知识/thmjs.png)

用src标签来链接外部js文件

## task5
Abusing Dialogue Functions 滥用对话功能
JS 的主要目标之一是提供对话框以便与用户交互并动态更新网页内容。

Alert
alert 函数在带有“ ”按钮的对话框中显示一条消息OK，通常用于向用户传达信息或警告。例如，如果我们想Hello THM向用户显示“ ”，我们将使用 alert("HelloTHM");。要试用它，请打开 Chrome 控制台，输入alert("Hello THM")，然后按 Enter。屏幕上将出现一个带有消息的对话

Prompt
prompt 函数显示一个对话框，要求用户输入。当用户单击“ OK”时，它返回输入的值；如果用户单击“ Cancel”，则返回 null。例如，要询问用户的姓名，我们将使用prompt("What is your name?");。

Confirm
确认函数显示一个对话框，其中包含一条消息和两个按钮：“ OK”和“ Cancel”。如果用户点击“ OK”，则返回 true，如果用户点击“ Cancel”，则返回 false。例如，要要求用户确认，我们将使用confirm("Are you sure?");。要尝试此操作，请打开 Chrome 控制台，输入confirm("Do you want to proceed?")，然后按Enter。 

![task5](/img/user/images/tryhackme-JavaScript-基础知识/task5.png)

打开源码的话会发现

![task5](/img/user/images/tryhackme-JavaScript-基础知识/yuan.png)

超过五次就没有这个提示 那就是警报五次

prompt这个函数用来与用户交互

输入tesla的话 就是存储了tesla的值

## task6
绕过控制流语句

JS 提供了多种控制流结构，例如if-else，switch语句用于做出决策，以及循环（例如for，，while和）do...while用于重复操作。

条件语句的实际应用

最常用的条件语句之一是if-else语句，它允许您根据条件的计算结果if-else 是否（true false）为来执行不同的代码块。

![task6](/img/user/images/tryhackme-JavaScript-基础知识/task6.png)

第一题打开age.html 输入一个比18小的数字就行 就能出提示

![task6](/img/user/images/tryhackme-JavaScript-基础知识/17.png)

第二题就是打开login.html 打开源码就能找到密码

![task6](/img/user/images/tryhackme-JavaScript-基础知识/admin.png)

## task7
探索最小化文件

JS 中的压缩是通过删除所有不必要的字符（例如空格、换行符、注释，甚至缩短变量名）来压缩 JS 文件的过程。这有助于减小文件大小并缩短网页的加载时间，尤其是在生产环境中。压缩后的文件使代码更紧凑，更难被人类阅读，但它们的功能仍然完全相同。

混淆（Obfuscation）通常用于通过添加不需要的代码、将变量和函数重命名为无意义的名称，甚至插入虚拟代码来使 JS 更难理解。

![hello](/img/user/images/tryhackme-JavaScript-基础知识/hello1.png)

这个就是混淆

我们可以通过[网站](https://obf-io.deobfuscate.io/)来进行混淆和反混淆（Deobfuscation）的操作

![task7](/img/user/images/tryhackme-JavaScript-基础知识/task7.png)

第一问 打开hello.html就能看到警报

![task7](/img/user/images/tryhackme-JavaScript-基础知识/welcome.png)

第二问 拿到网站就能反混淆

![task7](/img/user/images/tryhackme-JavaScript-基础知识/21.png)

## task8
避免仅依赖客户端验证
JS 的主要功能之一是执行客户端验证。开发人员有时会使用它来验证表单并完全依赖它，这不是一个好的做法。由于用户可以在客户端禁用/操纵JS，因此在服务器端执行验证也是必不可少的。

避免添加不受信任的库
如前面的任务中所述，JS 允许您使用src标签内的属性包含任何其他 JS 脚本script。但您是否考虑过我们包含的库是否来自可信来源？不良行为者已将一组名称与合法库相似的库上传到互联网上。因此，如果您盲目地包含恶意库，您的 Web 应用程序将面临威胁。

避免硬编码
切勿将API密钥、访问令牌或凭据等敏感数据硬编码到 JS 代码中，因为用户可以轻松检查源代码并获取密码。

压缩并混淆你的 JavaScript 代码
压缩和混淆 JS 代码可减少其大小、缩短加载时间，并使攻击者更难理解代码的逻辑。因此，在生产中使用代码时，请务必 压缩 和 混淆 代码。

简单介绍就到这里