---
{"dg-publish":true,"permalink":"/tryhackme/tryhackme-Burp-Suite：基础知识/","title":"tryhackme-Burp Suite：基础知识","tags":["linux","tryhackme","渗透工具"]}
---


这次介绍如何使用 Burp Suite 进行 Web 应用程序渗透测试。

重点将围绕以下关键方面：

- 对Burp Suite的详细介绍。
- 全面概述框架内可用的各种工具。
- 有关在您的系统上安装Burp Suite的过程的详细指导。
- 导航和配置Burp Suite。


## task2

**Burp Suite**是一个基于 Java 的框架，旨在作为进行 Web 应用程序渗透测试的综合解决方案。它已成为对 Web 和移动应用程序（包括依赖应用程序编程接口( API)的应用程序）进行实际安全评估的行业标准工具。

Burp Suite可以捕获并允许操纵浏览器和 Web 服务器之间的所有HTTP /HTTPS 流量。这一基本功能构成了框架的支柱。通过拦截请求，用户可以灵活地将它们路由到Burp Suite框架内的各个组件

Burp Suite Professional是Burp Suite Community的不受限制版本。它具有以下功能：

- 自动化漏洞扫描程序。
- 不受速率限制的模糊测试器/暴力破解器。
- 保存项目以供将来使用和生成报告。
- 内置API允许与其他工具集成。
- 不受限制地访问以添加新的扩展以实现更强大的功能。
- 访问Burp Suite Collaborator（有效地提供一个自托管或- 在 Portswigger 拥有的服务器上运行的唯一请求捕获器）。

Burp Suite Enterprise与社区版和专业版不同，主要用于持续扫描。它具有一个自动扫描器，可定期扫描 Web 应用程序是否存在漏洞，类似于 Nessus 等工具执行自动基础设施扫描的方式。与其他允许从本地机器进行手动攻击的版本不同，Burp Suite Enterprise 驻留在服务器上并不断扫描目标 Web 应用程序是否存在潜在漏洞。

看看task2

![task2](/img/user/images/tryhackme-Burp-Suite：基础知识/task2.png)

Burp Suite Enterprise提供持续扫描的功能

Burp Suite可以用于攻击web应用和移动应用程序

## task3

Burp Suite主要功能：

Proxy代理：Burp代理是Burp Suite最著名的功能。它能够在与 Web 应用程序交互时拦截和修改请求和响应。

- Repeater(中继器也叫重放)：另一个众所周知的功能。 中继器允许多次捕获、修改和重新发送相同的请求。当通过反复试验（例如，在SQLi - 结构化查询语言注入中）制作有效负载或测试端点功能是否存在漏洞时，此功能特别有用。

- Intruder(入侵者)：尽管Burp Suite社区有速率限制 ，但入侵者仍允许向端点发送请求。它通常用于暴力攻击或模糊端点。

- Decoder(解码器)：解码器为数据转换提供了有价值的服务。它可以解码捕获的信息或编码有效载荷，然后再将其发送到目标。虽然存在用于此目的的替代服务，但利用 Burp Suite中的解码器可以非常高效。

- Comparer(比较器)：顾名思义，比较器可以在字或字节级别比较两段数据。虽然这并非Burp Suite独有的功能，但使用单个键盘快捷键将可能很大的数据段直接发送到比较工具的功能可以显著加快该过程。

- Sequencer(序列器)：序列器通常用于评估令牌的随机性，例如会话 cookie 值或其他据称随机生成的数据。如果用于生成这些值的算法缺乏安全随机性，则可能会暴露毁灭性攻击的途径。

![task3](/img/user/images/tryhackme-Burp-Suite：基础知识/task3.png)

用proxy来拦截请求

用Intruder来强行破解

## task4
这部分是安装 这个就略过了

## task5

![task3](/img/user/images/tryhackme-Burp-Suite：基础知识/task5.png)

来看问题

![task3](/img/user/images/tryhackme-Burp-Suite：基础知识/task51.png)

event log 事件日志可以查看操作信息

## task6

讲的是导航栏

![daoh](/img/user/images/tryhackme-Burp-Suite：基础知识/daoh.png)

快捷键
Ctrl + Shift + D	 仪表板

Ctrl + Shift + T	 目标选项卡

Ctrl + Shift + P	 代理选项卡

Ctrl + Shift + I	 入侵者标签

Ctrl + Shift + R	 中继器选项卡

## task7
先来了解一下配置Burp Suite的可用选项。有两种类型的设置：全局设置（也称为用户设置）和项目设置。

- 全局设置：这些设置会影响整个Burp Suite安装，并在每次启动应用程序时应用。它们为您的Burp Suite 环境提供基准配置。

- 项目设置：这些设置特定于当前项目，仅在会话期间应用。但是，请注意，Burp Suite社区版不支持保存项目，因此关闭 Burp 时任何特定于项目的选项都将丢失。

要访问设置，请点击顶部导航栏中的“设置”按钮。这将打开一个单独的设置窗口。

在“设置”窗口中，您会在左侧看到一个菜单。此菜单允许您在不同类型的设置之间切换，包括：

- 搜索：使用关键字搜索特定设置。
- 类型过滤器：过滤用户和项目选项的设置 。
- 用户设置：显示影响整个Burp Suite安装的设置。
- 项目设置：显示特定于当前项目的设置。
- 类别：允许按类别选择设置。

![task7](/img/user/images/tryhackme-Burp-Suite：基础知识/task7.png)

第一问 直接在设置里面搜索cookie jar

![task7](/img/user/images/tryhackme-Burp-Suite：基础知识/session.png)

第二问 搜索update

![task7](/img/user/images/tryhackme-Burp-Suite：基础知识/update.png)

第三问

![hotkey](/img/user/images/tryhackme-Burp-Suite：基础知识/hotkeys.png)

第四问毋庸置疑肯定是yea

## task8
 Burp代理的关键点
 
拦截请求：当通过 Burp代理发出请求时，这些请求会被拦截并阻止到达目标服务器。请求显示在“代理”选项卡中，允许进一步执行操作，例如转发、删除、编辑或将其发送到其他 Burp 模块。 要禁用拦截并允许请求不间断地通过代理，请单击按钮。 Intercept is on 

控制：拦截请求的能力使测试人员能够完全控制网络流量，这对于测试网络应用程序非常有用。

Capture and Logging（捕获和记录）： Burp Suite默认捕获并记录通过代理发出的请求，即使拦截已关闭。此记录功能有助于以后分析和审查以前的请求。

WebSocket Support（WebSocket支持）： Burp Suite还能捕获并记录WebSocket 通信，在分析 Web 应用程序时提供额外的帮助。

日志和历史记录：可以在HTTP历史记录和WebSockets 历史记录子选项卡中查看捕获的请求 。

代理设置中的一些值得注意的功能

响应拦截：默认情况下，代理 不会拦截服务器响应，除非每个请求都明确要求。“根据以下规则拦截响应”复选框以及定义的规则允许更灵活的响应拦截。

匹配和替换：代理设置中的“匹配和替换”部分允许使用正则表达式 (regex) 修改传入和传出请求。此功能允许进行动态更改，例如修改用户代理或操纵 cookie。

## task9
就是配置代理（proxy）然后用Burp Suite来抓包

## task10
讲的是target选项卡
由三个子选项卡组成：

Site map（站点地图）：此子选项卡允许我们以树形结构绘制我们定位的 Web 应用程序。代理处于活动状态时访问的每个页面都将显示在站点地图上。此功能使我们能够通过简单地浏览 Web 应用程序来自动生成站点地图。

Issue definitions（问题定义）：提供的完整漏洞扫描功能，但我们仍然可以访问扫描器查找的所有漏洞的列表。

Scope settings（范围设置）：此设置允许控制 Burp Suite中的目标范围。它使我们能够包含或排除特定域/ IP来定义测试范围。
## task11
讲的是The Burp Suite Browser

用这个浏览器的话 抓包啥的就比较方便

## task12
可以定义Burp Suite中代理和记录的内容。可以限制Burp Suite仅针对我们要测试的特定 Web 应用程序。

最简单的方法是切换到选项Target卡，右键单击左侧列表中的目标，然后选择 Add To Scope。Burp 会提示我们选择是否要停止记录不在范围内的任何内容，在大多数情况下，要选择yes。

## task13
代理https 我们可以导入ca证书来启用 TLS 的站点

## task14
这显示的是个xxs漏洞