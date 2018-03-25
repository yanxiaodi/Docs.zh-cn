---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: "SignalR 疑难解答 |Microsoft 文档"
author: pfletcher
description: "本文介绍开发 SignalR 应用程序的常见问题。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 2394ee81f4592417a034e47db6eefd3e4b91a9af
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="signalr-troubleshooting"></a>SignalR 故障排除
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本文档将介绍使用 SignalR 的常见故障排除问题。
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


本文档包含以下各节。

- [以无提示方式调用客户端和服务器之间的方法，则会失败](#connection)
- [配置 IIS websocket ping/pong 来检测死的客户端](#pong)
- [其他连接问题](#other)
- [编译和服务器端错误](#server)
- [Visual Studio 问题](#vs)
- [Internet 信息服务的问题](#iis)
- [Microsoft Azure 问题](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>以无提示方式调用客户端和服务器之间的方法，则会失败

本部分介绍方法调用失败没有有意义的错误消息的客户端和服务器之间可能的原因。 在 SignalR 应用程序中，服务器已没有有关该客户端实现; 的方法的信息当服务器调用时的客户端方法，方法名称和参数数据发送到客户端，并仅当它存在于指定的服务器的格式时，才执行此方法。 如果没有匹配的方法是在上找到客户端，会执行任何操作，并且在服务器上不会引发任何错误消息。

要进一步调查不调用客户端方法，可以打开之前调用的中心查看哪些调用 start 方法都来自服务器的日志记录。 若要启用日志记录中的 JavaScript 应用程序，请参阅[如何启用客户端日志记录 （JavaScript 客户端版本）](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging)。 若要启用日志记录在.NET 客户端应用程序，请参阅[如何启用客户端日志记录 （.NET 客户端版本）](../guide-to-the-api/hubs-api-guide-net-client.md#logging)。

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>拼写错误的方法、 不正确的方法签名或不正确的中心名称

如果名称或调用方法的签名与客户端上适当的方法不完全匹配，则调用将失败。 验证由服务器调用的方法名称匹配的客户端上的方法的名称。 此外，SignalR 创建中心代理使用混合使用大小写的方法，以根据在 JavaScript 中，因此调用的方法，`SendMessage`在服务器上将调用`sendMessage`客户端代理中。 如果你使用`HubName`属性在服务器端代码中，验证是否使用的名称与用来在客户端上创建中心的名称匹配。 如果不使用`HubName`属性中，验证是否 camel 大小写形式，如而不是 ChatHub chatHub JavaScript 客户端在中心的名称。

### <a name="duplicate-method-name-on-client"></a>客户端上的重复的方法名称

请确认不有重复的方法仅大小写上不同的客户端上。 如果客户端应用程序调用的方法`sendMessage`，请验证是否未还调用的方法`SendMessage`以及。

### <a name="missing-json-parser-on-the-client"></a>客户端上缺少 JSON 分析器

SignalR 需要 JSON 分析器存在才可序列化的服务器和客户端之间的调用。 如果你的客户端没有内置的 JSON 分析器 （如 Internet Explorer 7)，你将需要包含一个应用程序中。 你可以下载 JSON 分析器[此处](http://nuget.org/packages/json2)。

### <a name="mixing-hub-and-persistentconnection-syntax"></a>混合使用中心 PersistentConnection 语法

SignalR 使用两个通信模型： 中心和 PersistentConnections。 调用这些两个通信模型的语法是在客户端代码不同。 如果你已在服务器代码中添加一个中心，请验证所有客户端代码使用正确的中心语法。

**在 JavaScript 客户端中创建 PersistentConnection 的 JavaScript 客户端代码**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**在 Javascript 客户端中创建一个中心代理的 JavaScript 客户端代码**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# 服务器代码映射到 PersistentConnection 的路由**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**将路由映射到一个中心，或多个中心上，如果你有多个应用程序的 C# 服务器代码**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>在添加订阅之前启动的连接

如果可以从服务器调用的方法添加到代理之前启动中心的连接，则将不接收消息。 下面的 JavaScript 代码将不会正确启动中心：

**将不允许将接收的中心消息的不正确 JavaScript 客户端代码**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

相反，请在调用开始之前添加方法订阅：

**正确将订阅添加到一个中心的 JavaScript 客户端代码**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>在中心代理服务器上缺少方法名称

验证服务器上定义的方法向订阅客户端上。 即使服务器定义的方法，则它必须仍将添加到客户端代理。 方法可以通过以下方式添加到客户端代理 (请注意，该方法添加到`client`的中心，而不中心直接成员):

**将方法添加到中心代理的 JavaScript 客户端代码**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>中心或未声明为公共的中心方法

若要在客户端上可见，中心实现和方法必须声明为`public`。

### <a name="accessing-hub-from-a-different-application"></a>从不同的应用程序访问中心

仅可以通过实现 SignalR 客户端应用程序访问 SignalR 集线器。 SignalR 无法进行交互操作，与其他通信库 （如 SOAP 或 WCF web 服务。）如果没有可用于你的目标平台的无 SignalR 客户端，你不能直接访问服务器的终结点。

### <a name="manually-serializing-data"></a>手动序列化数据

SignalR 将自动使用 JSON 序列化你的方法参数-这无需自行执行此操作。

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>没有对 OnDisconnected 函数中的客户端执行的远程中心方法

此行为是有意安排的。 当`OnDisconnected`是调用，已进入中心`Disconnected`状态，不允许进一步中心方法调用。

**正确 OnDisconnected 事件中执行代码的 C# 服务器代码**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>以一致的时间不引发 OnDisconnect

此行为是有意安排的。 当用户尝试通过导航离开 SignalR 连接处于活动的页时，SignalR 客户端然后将进行通知服务器将停止客户端连接的最大努力尝试。 如果 SignalR 客户端的最大努力尝试无法到达服务器，服务器将停止后可配置连接`DisconnectTimeout`更高版本，在这段时间`OnDisconnected`事件将激发。 如果 SignalR 客户端的最大努力尝试不成功，`OnDisconnected`将立即触发事件。

有关设置信息`DisconnectTimeout`设置，请参阅[处理连接生存期事件： DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout)。

### <a name="connection-limit-reached"></a>已达到连接限制

当使用 IIS 的完整版本，如 Windows 7 客户端操作系统上，受 10 连接限制。 在使用客户端操作系统，改用 IIS Express 以避免此限制。

### <a name="cross-domain-connection-not-set-up-properly"></a>跨域连接未正确设置

如果跨域连接 （连接为其的 SignalR URL 不是为宿主的页位于同一域中） 未正确设置，连接可能会失败并且不显示错误消息。 有关如何启用跨域通信的信息，请参阅[如何建立的跨域连接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>连接使用 NTLM (Active Directory) 不在.NET 客户端中工作

如果连接未正确配置，在.NET 客户端应用程序中使用域安全的连接可能会失败。 若要在域环境中使用 SignalR，请将所需的连接属性，如下所示设置：

**实现连接凭据的 C# 客户端代码**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>配置 IIS websocket ping/pong 来检测死的客户端

SignalR 服务器不知道，是否客户端是否已死以及它们依赖于从基础 websocket 连接失败的通知，即 OnClose 回调。 此问题的一种解决方案是配置 IIS websocket ping/pong 为你执行操作。 这可确保，如果它在意外中断，你的连接将关闭。 有关详细信息请参阅[此 stackoverflow 文章](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss)。

<a id="other"></a>

## <a name="other-connection-issues"></a>其他连接问题

本部分介绍的原因和解决方案的具体的症状或在连接过程中发生的错误消息。

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>"可以发送数据之前，必须调用 start"错误

如果代码引用 SignalR 对象启动连接之前，通常出现此错误。 处理程序和 like 绑定，将在服务器上定义的调用方法必须添加连接完成后。 请注意，调用`Start`是异步的因此，代码调用可能在它之前执行后完成。 连接完全启动后添加处理程序的最佳方法是将其放到作为参数传递到的启动方法的回调函数：

**正确添加引用 SignalR 对象的事件处理程序的 JavaScript 客户端代码**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

如果连接会在仍然引用 SignalR 对象时停止，则也将出现此错误。

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"永久移动 301"或者"暂时移动 302"错误

如果项目包含一个名为 SignalR 会干扰自动创建代理文件夹，则可能出现此错误。 若要避免此错误，不要使用一个名为文件夹`SignalR`你的应用程序，或关闭的启用自动代理生成。 请参阅[生成代理和它为您完成](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)有关详细信息。

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>在.NET 或 Silverlight 客户端中的"403 禁止访问"错误

在跨域通信未正确启用跨域环境中可能出现此错误。 有关如何启用跨域通信的信息，请参阅[如何建立的跨域连接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。 若要建立在 Silverlight 客户端的跨域连接，请参阅[来自 Silverlight 客户端的跨域连接](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)。

### <a name="404-not-found-error"></a>"404 未找到"错误

有几个原因，此问题。 验证所有以下操作：

- **中心代理地址引用的格式不正确：**如果对生成的中心代理地址的引用的格式不正确，则通常会出现此错误。 验证对中心地址的引用可以正确地完成。 请参阅[如何引用动态生成的代理](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy)有关详细信息。
- **将路由添加到应用程序，然后添加中心路由：**如果你的应用程序使用的其他路由，验证是否添加的第一个路由是对调用`MapSignalR`。
- **适用于无扩展名的 Url 中使用 IIS 7 或不带更新 7.5:**使用 IIS 7 或 7.5 需要更新无扩展名的 url，以便服务器可以提供对中心定义访问`/signalr/hubs`。 找不到更新[此处](https://support.microsoft.com/kb/980368)。
- **IIS 缓存过期或已损坏：**若要验证缓存内容不是过期，可清除缓存的 PowerShell 窗口中输入以下命令：

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>"500 内部服务器错误"

这是一个非常一般性错误，可能有多种原因。 错误的详细信息应出现在服务器的事件日志，或可以通过调试服务器发现。 通过打开服务器上的详细错误都可以获得更多详细的错误信息。 有关详细信息，请参阅[如何处理中心类中的错误](../guide-to-the-api/hubs-api-guide-server.md#handleErrors)。

如果防火墙或代理配置不正确，导致请求标头重写，通常还出现此错误。 解决方案是确保在防火墙或代理上启用了端口 80。

### <a name="unexpected-response-code-500"></a>"意外的响应代码： 500"

如果在应用程序中使用的.NET framework 的版本与在 Web.Config 中指定的版本不匹配，则可能出现此错误。该解决方案旨在验证应用程序设置和 Web.Config 文件中使用了.NET 4.5。

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt;是不确定的"错误

如果导致此错误，将调用`MapSignalR`不会正确进行。 请参阅[如何注册 SignalR 中间件和配置 SignalR 选项](../guide-to-the-api/hubs-api-guide-server.md#route)有关详细信息。

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>用户代码未处理 JsonSerializationException

验证发送到你的方法的参数不包括非可序列化类型 （如文件句柄或数据库连接）。 如果你需要在你不想要将发送到客户端 （不管是进行安全，或者序列化的原因），使用服务器端对象上使用成员`JSONIgnore`属性。

### <a name="protocol-error-unknown-transport-error"></a>"协议错误： 未知的传输"错误

如果客户端不支持使用 SignalR 的传输，则可能出现此错误。 请参阅[传输和回退](../getting-started/introduction-to-signalr.md#transports)有关可与 SignalR 在其的浏览器的信息。

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"JavaScript Hub 代理生成已禁用。"

如果将发生此错误`DisableJavaScriptProxies`时还包括对在动态生成的代理的引用设置`signalr/hubs`。 有关手动创建代理的详细信息，请参阅[生成的代理和它为您完成](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)。

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"连接 ID 格式不正确"或"SignalR 连接处于活动状态期间无法更改用户标识"错误

如果正在使用身份验证，并且客户端已注销，在停止连接之前，可能出现此错误。 该解决方案旨在停止之前注销客户端的 SignalR 连接。

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"未捕获的错误： SignalR: jQuery 找不到。 请确保 jQuery 引用然后 SignalR.js 文件"错误

SignalR JavaScript 客户端需要 jQuery 运行。 验证你对 jQuery 引用正确，使用的路径有效，以及对 jQuery 的引用是之前对 SignalR 的引用。

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"未捕获的 TypeError： 无法读取属性&lt;属性&gt;的未定义"错误

不具有 jQuery 或引用正确的中心代理导致此错误。 验证正确，使用的路径有效，以及对 jQuery 的引用是对中心代理引用之前 jQuery 和中心代理你参考。 对中心代理的默认引用应如下所示：

**HTML 客户端代码与正确引用中心代理**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>"由用户代码未处理 RuntimeBinderException"错误

可能会发生此错误时的不正确的重载`Hub.On`使用。 如果该方法具有返回值，则必须作为泛型类型参数指定返回类型：

**（无需生成的代理） 客户端上定义的方法**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>连接 ID 不一致或连接中断之间页面加载

此行为是有意安排的。 由于中心对象中的页对象承载，中心被销毁时刷新该页面。 多页应用程序需要维护用户与连接 Id 之间的关联，从而使它们将页面加载之间保持一致。 可以在服务器上存储的连接 Id`ConcurrentDictionary`对象或数据库。

### <a name="value-cannot-be-null-error"></a>"值不能为 null"错误

当前不支持带有可选参数的服务器端方法;如果省略可选参数，该方法将失败。 有关详细信息，请参阅[可选参数](https://github.com/SignalR/SignalR/issues/324)。

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox 无法建立到上的服务器的连接&lt;地址&gt;"Firebug 时出错

如果协商的 WebSocket 传输会失败，改为使用另一个传输，可以在 Firebug 看到此错误消息。 此行为是有意安排的。

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>在.NET 客户端应用程序中的"远程证书无效的验证过程根据"错误

如果你的服务器需要自定义客户端证书，则可以将 x509 证书添加到连接之前发出的请求。 将证书添加到连接使用`Connection.AddClientCertificate`。

### <a name="connection-drops-after-authentication-times-out"></a>连接中断后身份验证超时

此行为是有意安排的。 连接处于活动状态; 时，不能修改身份验证凭据若要刷新的凭据，必须停止并重新启动连接。

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected 获取调用两次时使用 jQuery Mobile

jQuery Mobile`initializePage`函数强制重新执行，每个页中的脚本从而创建第二个连接。 此问题的解决方法包括：

- 包括对 jQuery Mobile JavaScript 文件之前的引用。
- 禁用`initializePage`函数通过设置`$.mobile.autoInitializePage = false`。
- 等待页后，可以完成初始化之前启动连接。

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>消息被延迟 Silverlight 应用程序使用服务器发送的事件中

使用服务器上 Silverlight 发送事件时，消息被延迟。 若要强制长轮询以改为使用，请在启动连接时使用以下：

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"权限被拒绝"使用永久框架协议

这是一个已知的问题，所述[此处](https://github.com/SignalR/SignalR/issues/1963)。 此故障现象，可能会看到使用最新的 JQuery 库;解决方法是降级到 JQuery 1.8.2 应用程序。

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException： 不是有效的 web 套接字请求。

如果使用 WebSocket 协议，但网络代理修改请求标头，则可能出现此错误。 解决方案是配置代理服务器以允许在端口 80 上的 WebSocket。

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"异常：&lt;方法名称&gt;方法无法解析"客户端时在服务器上调用方法

使用不能发现在 JSON 负载，如数组中的数据类型，则可能会导致此错误。 解决方法是使用 JSON，如 IList 通过可发现的数据类型。 有关详细信息，请参阅[.NET 客户端无法调用具有数组参数的中心方法](https://github.com/SignalR/SignalR/issues/2672)。

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>编译和服务器端错误

 以下部分包含对编译器和服务器端运行时错误可能的解决方案。 

### <a name="reference-to-hub-instance-is-null"></a>对中心实例引用为 null

由于系统会为每个连接创建的 hub 实例，你无法创建实例的中心在代码中自己。 若要在一个中心从外部集线器本身上调用方法，请参阅[如何调用方法的客户端和管理从中心类外部的组](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub)有关如何获取对中心上下文的引用。

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session 为 null

此行为是有意安排的。 SignalR 不支持 ASP.NET 会话状态，因为启用会话状态将会破坏双工消息传递。

### <a name="no-suitable-method-to-override"></a>没有合适的方法重写

如果你使用的较旧的文档或博客中的代码，你可能会看到此错误。 验证未引用的已更改或已弃用的方法名称 (如`OnConnectedAsync`)。

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl is null

此行为是有意安排的。 此成员已弃用，不应使用。

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"名为 signalr.hubs 的路由集合中已路由"错误

将出现此错误，如果`MapSignalR`两次由你的应用程序调用。 一些示例应用程序调用`MapSignalR`直接在 Startup 类; 其他人在中进行调用的包装类。 确保你的应用程序不会不两。

### <a name="websocket-is-not-used"></a>未使用 WebSocket

如果已验证你的服务器和客户端满足 WebSocket 的要求 (列入[支持的平台](../getting-started/supported-platforms.md)文档)，你将需要在你的服务器上启用 WebSocket。 找不到为执行此操作的说明[此处](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。

### <a name="connection-is-undefined"></a>$.connection 未定义

此错误表示，在页面上的脚本加载不正确，或者是中心代理不可访问或正在访问的不正确。 验证在你页面上的脚本引用对应于在项目中，加载的脚本和 /signalr/hubs 可以访问在浏览器中，如果服务器运行。

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>找不到需要编译动态表达式的一个或多个类型

此错误指示`Microsoft.CSharp`库是缺失。 添加在**程序集的&gt;Framework**选项卡。

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>不能从 Clients.Caller 在 Visual Basic 中或在强类型化的中心; 访问调用方状态"从类型任务 （的对象） 到类型 'String' 的转换无效"错误

若要访问调用方状态在 Visual Basic 中或在强类型化的中心，使用`Clients.CallerState`属性 （在 SignalR 2.1 中引入） 而不是`Clients.Caller`。

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Visual Studio 问题

本部分介绍在 Visual Studio 中遇到问题。

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>在解决方案资源管理器中不显示脚本文档节点

一些我们的教程在调试时将你定向到解决方案资源管理器中的"脚本文档"节点。 此节点由 JavaScript 调试器中，并且将只会显示调试 Internet Explorer; 中的浏览器客户端时如果使用 Chrome 或 Firefox，将不会出现该节点。 如果正在运行另一个客户端调试器，例如 Silverlight 调试器 JavaScript 调试器也将不运行。

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>Visual Studio 2008 或更低版本 SignalR 不起作用

此行为是有意安排的。 SignalR 需要.NET Framework 4 或更高版本;这需要 Visual Studio 2010 或更高版本的情况下开发 SignalR 应用程序。 SignalR 的服务器组件需要.NET Framework 4.5。

<a id="iis"></a>

## <a name="iis-issues"></a>IIS 问题

本部分包含使用 Internet 信息服务的问题。

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>在 Visual Studio 开发服务器上，但不是在 IIS 中的 SignalR 工作原理

SignalR 支持在 IIS 7.0 和 7.5，但必须添加无扩展名的 Url 的支持。 若要添加支持无扩展名的 Url，请参阅[https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR 需要 ASP.NET （ASP.NET 未安装 IIS 默认情况下） 在服务器上安装。 若要安装 ASP.NET，请参阅[ASP.NET 下载](https://www.asp.net/downloads)。

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Microsoft Azure 问题

本部分包含与 Microsoft Azure 的问题。

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException 托管在 Azure 辅助角色中的 SignalR 时

承载在 Azure 辅助角色中的 SignalR 在异常中可能会导致"无法加载文件或程序集 Microsoft.Owin，Version = 2.0.0.0"。 这是使用 NuGet; 一个已知的问题绑定重定向不会自动添加在 Azure 辅助角色项目中。 若要解决此问题，您可以手动添加绑定重定向。 添加以下行`app.config`辅助角色项目文件。

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>收到的消息通过 Azure 底板后更改主题名称

使用 Azure 底板的主题是在内部; 进行维护不应将它们用户可配置。
