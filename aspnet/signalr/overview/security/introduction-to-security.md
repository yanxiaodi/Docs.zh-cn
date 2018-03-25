---
uid: signalr/overview/security/introduction-to-security
title: "SignalR 安全性简介 |Microsoft 文档"
author: pfletcher
description: "描述开发 SignalR 应用程序时必须考虑的安全问题。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 1cb9f15a958028822b50decf4b420c36596ce25e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-signalr-security"></a>SignalR 安全性简介
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍开发 SignalR 应用程序时必须考虑的安全问题。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>本主题中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 版本 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主题的早期版本
> 
> 有关 SignalR 的早期版本的信息，请参阅[SignalR 较旧版本](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>概述

本文档包含以下各节：

- [SignalR 安全性的基础概念](#concepts)

    - [身份验证和授权](#authentication)
    - [连接令牌](#connectiontoken)
    - [当重新连接时重新加入组](#rejoingroup)
- [SignalR 如何防止跨站点请求伪造](#csrf)
- [SignalR 安全建议](#recommendations)

    - [安全套接字层 (SSL) 协议](#ssl)
    - [不使用组作为一种安全机制](#groupsecurity)
    - [安全地处理来自客户端的输入](#input)
    - [协调活动连接的用户状态中的更改](#reconcile)
    - [自动生成的 JavaScript 代理文件](#autogen)
    - [异常](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>SignalR 安全性的基础概念

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>身份验证和授权

SignalR 不提供用户进行身份验证的任何功能。 相反，你将 SignalR 功能集成到应用程序的现有身份验证结构。 验证用户身份像处理通常在您的应用程序，并且可使用你 SignalR 的身份验证的结果代码。 例如，你可能对 ASP.NET 窗体身份验证，你的用户进行身份验证，然后在你的中心，会强制哪些用户或角色有权调用的方法。 在你的中心，你还可以传递身份验证信息，如用户名或用户是否属于某个角色，与客户端。

SignalR 提供[Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)特性来指定哪些用户有权访问的中心或方法。 将 Authorize 属性应用到一个中心或中心中的特定方法。 如果没有 Authorize 特性中，中心上的所有公共方法可供连接到集线器的客户端。 有关中心的详细信息，请参阅[身份验证和授权 SignalR 集线器的](hub-authorization.md)。

你将应用`Authorize`属性为中心，但不是持久连接。 若要强制实施授权规则，当使用`PersistentConnection`必须重写`AuthorizeRequest`方法。 永久连接有关的详细信息，请参阅[身份验证和授权的 SignalR 永久性连接](persistent-connection-authorization.md)。

<a id="connectiontoken"></a>

### <a name="connection-token"></a>连接令牌

SignalR 可以降低风险的通过验证的发件人的标识来执行恶意命令。 对于每个请求，客户端和服务器传递连接令牌，其中包含的连接 id 和身份验证的用户的用户名。 连接 id 唯一标识每个连接的客户端。 新连接创建，且在连接期间会一直保持该 id 时，服务器随机生成的连接 id。 Web 应用程序的身份验证机制提供用户名。 SignalR 使用加密和数字签名来保护连接令牌。

![](introduction-to-security/_static/image2.png)

对于每个请求，服务器将验证令牌，以确保请求来自指定的用户的内容。 用户名必须与对应的连接 id。通过验证的连接 id 和用户名，SignalR 防止恶意用户轻松地模拟其他用户。 如果服务器无法验证连接令牌，则请求将失败。

![](introduction-to-security/_static/image4.png)

因为连接 id 是验证过程的一部分，不应显示一个用户的连接 id 与其他用户或存储的值在客户端，如在一个 cookie。

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>当重新连接时重新加入组

默认情况下，SignalR 应用程序将自动重新将用户分配到相应的组时从临时中断，如连接时删除并重新建立连接超时之前重新连接。当重新连接后，客户端将传递包括连接 id 和分配的组的组令牌。 组令牌进行数字签名和加密。 客户端重新连接; 之后保留相同的连接 id因此，传递从重新连接的客户端的连接 id 必须匹配上一个客户端使用的连接 id。 此验证可防止恶意用户将请求加入未经授权的组，当重新连接时传递。

但是，务必请注意，组令牌不会过期。 如果用户在过去，属于组，但已禁止从该组，则该用户可能能够模拟包括禁止的组的组令牌。 如果您需要安全地管理哪些用户属于哪些组，你需要存储在服务器上，该数据比如说在数据库中。 然后，将逻辑添加到你在服务器验证用户是否属于某个组的应用程序。 验证组成员身份的示例，请参阅[使用组](../guide-to-the-api/working-with-groups.md)。

自动重新加入组时，才应用连接暂时中断后重新连接。 如果用户断开连接的离开应用程序或应用程序重新启动，你的应用程序必须处理如何将该用户添加到正确的组。 有关详细信息，请参阅[使用组](../guide-to-the-api/working-with-groups.md)。

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>SignalR 如何防止跨站点请求伪造

跨站点请求伪造 (CSRF) 是一种攻击，恶意站点向在当前登录用户的易受攻击站点发送请求的位置。 SignalR 可 CSRF 防止通过使恶意站点创建 SignalR 应用程序的有效请求情况极其少见。

### <a name="description-of-csrf-attack"></a>CSRF 攻击的说明

下面是 CSRF 攻击的示例：

1. 用户登录到 www.example.com 时，使用窗体身份验证。
2. 服务器对用户进行身份验证。 来自服务器的响应包括身份验证 cookie。
3. 且无需注销，用户访问恶意网站。 此恶意站点包含的以下 HTML 窗体： 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 请注意，窗体操作发送到易受攻击的站点，不适用于恶意站点。 这是 CSRF 的"跨站点"部分。
4. 用户可单击提交按钮。 浏览器包括与请求的身份验证 cookie。
5. 请求与用户的身份验证上下文的 example.com 服务器上运行，并可以执行任何身份验证的用户可以执行的操作。

虽然此示例需要用户单击窗体按钮，恶意页无法同样轻松地运行 AJAX 请求发送到 SignalR 应用程序的脚本。 此外，使用 SSL 不会阻止 CSRF 攻击中，因为恶意站点可以发送"https://"请求。

通常情况下，CSRF 攻击是可能对网站进行身份验证，使用 cookie 的因为浏览器向目标网站发送所有相关 cookie。 但是，CSRF 攻击并不局限于利用 cookie。 例如，基本和摘要式身份验证也是易受攻击的。 用户登录时基本或摘要式身份验证后，浏览器会话结束之前会自动发送凭据。

### <a name="csrf-mitigations-taken-by-signalr"></a>执行 SignalR CSRF 缓解措施

SignalR 执行以下步骤来创建到你的应用程序的有效请求会阻止恶意站点。 SignalR 采用默认情况下的这些步骤，您不需要在代码中执行任何操作。

- **禁用跨域请求**  
 SignalR 禁用跨域请求，以防止用户来自外部域调用 SignalR 终结点。 SignalR 考虑来自外部域的任何请求无效，并阻止请求。 我们建议您保留此默认行为;否则，恶意网站可以欺骗用户将命令发送到你的站点。 如果你需要使用跨域请求，请参阅[如何建立的跨域连接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。
- **将连接标记传递在查询字符串内，不 cookie**  
 SignalR 作为 cookie，而不是传递作为查询字符串值，连接令牌。 在一个 cookie 中存储连接令牌是不安全的因为浏览器可以无意中将转发连接令牌时遇到恶意代码。 此外，在查询字符串中传递连接令牌可防止连接令牌持久化超出当前的连接。 因此，恶意用户不能在另一个用户的身份验证凭据的请求。
- **验证连接令牌**  
 中所述[连接令牌](#connectiontoken)部分中，服务器知道哪个连接 id 是与每个经过身份验证的用户相关联。 服务器不会处理来自用户名称不匹配的连接 id 的任何请求。 不太可能的恶意用户无法猜测有效的请求，因为恶意用户必须知道用户名称和当前的随机生成的连接 id。只要该连接结束，该连接 id 将变为无效。 匿名用户不应有权访问任何敏感信息。

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR 安全建议

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>安全套接字层 (SSL) 协议

SSL 协议使用加密来保护客户端和服务器之间的数据传输。 如果你 SignalR 的应用程序之间传输敏感信息的客户端和服务器，则使用 SSL 传输。 有关设置 SSL 的详细信息，请参阅[如何设置 SSL 在 IIS 7 上](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>不使用组作为一种安全机制

组是一种简便方式收集相关的用户，但它们不是用于限制对敏感信息的访问的安全机制。 这在用户可以自动重新组加入期间重新连接时尤其如此。 相反，请考虑向角色添加具有权限的用户并将限制对为该角色的成员中心方法的访问。 有关限制的基于角色的访问的示例，请参阅[身份验证和授权 SignalR 集线器的](hub-authorization.md)。 检查是否对组的用户访问，当重新连接时的示例，请参阅[使用组](../guide-to-the-api/working-with-groups.md)。

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>安全地处理来自客户端的输入

若要确保恶意用户不向其他用户发送脚本，必须对来自客户端旨在用于广播到其他客户端的所有输入进行都编码。 因为你 SignalR 应用程序可能有许多不同类型的客户端，应该对服务器上，而不接收客户端的消息进行编码。 因此，HTML 编码适用为 web 客户端，而不是其他类型的客户端。 例如，若要显示聊天消息的 web 客户端方法将安全地处理用户名称和消息通过调用`html()`函数。

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>协调活动连接的用户状态中的更改

如果用户的身份验证状态发生更改的活动连接存在时，用户将收到一个错误，表明，"用户标识无法更改在 SignalR 连接处于活动期间。" 在这种情况下，你的应用程序应重新连接到服务器以确保连接 id 和用户名得到协调。 例如，如果你的应用程序允许用户注销时存在的活动连接，连接的用户名将不再匹配下一个请求传入的名称。 你将想要停止连接，才能在用户注销，然后重新启动。

但是，务必请注意，大多数应用程序将不需要手动停止并启动连接。 如果你的应用程序用户重定向到单独的页面后日志记录，例如 Web 窗体应用程序或 MVC 应用程序中的默认行为，或者注销后刷新当前页面时，自动断开连接的活动连接，而不需要任何其他操作。

下面的示例演示如何停止和启动连接时的用户状态已更改。

[!code-html[Main](introduction-to-security/samples/sample3.html)]

或者，如果您的网站使用 Forms 身份验证，使用滑动过期，并且没有任何活动需要有效的身份验证 cookie，可能会更改用户的身份验证状态。 在这种情况下，用户将被注销，并且用户名称将不再匹配连接令牌中的用户名称。 可以通过添加一些定期请求 web 服务器需要有效的身份验证 cookie 上资源的脚本来解决此问题。 下面的示例演示如何请求每隔 30 分钟的资源。

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>自动生成的 JavaScript 代理文件

如果你不希望在每个用户的 JavaScript 代理文件中包括的所有中心和方法，则可以禁用自动生成的文件。 如果你具有多个中心和方法，但不是希望每个用户需要注意的所有方法，可以选择此选项。 通过设置禁用自动生成**EnableJavaScriptProxies**到**false**。

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

有关 JavaScript 代理文件的详细信息，请参阅[生成的代理和它为您完成](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。 <a id="exceptions"></a>

### <a name="exceptions"></a>异常

应避免将异常对象传递到客户端，因为对象可能会公开给客户端的敏感信息。 相反，在显示的相关错误消息的客户端上调用方法。

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
