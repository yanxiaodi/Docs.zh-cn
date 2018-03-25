---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: "ASP.NET SignalR 中心 API 指南-.NET 客户端 (C#) |Microsoft 文档"
author: pfletcher
description: "本文档提供使用 SignalR 在.NET 客户端，如 Windows 应用商店 (WinRT)、 WPF、 Silverlight 和 cons 版本 2 的中心 API 的说明..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: c52a02291e18b1dd8a9d95b33fe466d17aae835f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>ASP.NET SignalR 中心 API 指南-.NET 客户端 (C#)
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文档提供使用 SignalR 在.NET 客户端，例如 Windows 应用商店 (WinRT)、 WPF、 Silverlight 和控制台应用程序中的版本 2 的中心 API 的简介。
> 
> SignalR 中心 API 使您能够远程过程调用 (Rpc) 从连接的客户端到服务器和客户端到服务器。 在服务器代码中，你定义可以由客户端，调用的方法和调用客户端运行的方法。 在客户端代码中，定义在服务器上，可以调用的方法，并调用在服务器运行的方法。 SignalR 将负责为你的客户端到服务器管道的所有。
> 
> SignalR 还提供了一个称为永久连接的较低级别 API。 简介 SignalR、 集线器和永久连接，或者有关演示如何生成完整的 SignalR 应用程序的教程，请参阅[SignalR-入门](../getting-started/index.md)。
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

- [客户端安装程序](#clientsetup)
- [如何建立连接](#establishconnection)

    - [来自 Silverlight 客户端的跨域连接](#slcrossdomain)
- [如何配置连接](#configureconnection)

    - [如何在 WPF 客户端中设置的最大并发连接数](#maxconnections)
    - [如何指定查询字符串参数](#querystring)
    - [如何指定传输方法](#transport)
    - [如何指定 HTTP 标头](#httpheaders)
    - [如何指定客户端证书](#clientcertificate)
- [如何创建中心代理](#proxy)
- [如何在该服务器可以调用的客户端上定义方法](#callclient)

    - [不带参数的方法](#clientmethodswithoutparms)
    - [具有指定参数类型的参数的方法](#clientmethodswithparmtypes)
    - [使用指定的参数的动态对象的参数的方法](#clientmethodswithdynamparms)
    - [如何移除处理程序](#removehandler)
- [如何从客户端调用服务器方法](#callserver)
- [如何处理连接生存期事件](#connectionlifetime)
- [如何处理错误](#handleerrors)
- [如何启用客户端日志记录](#logging)
- [WPF、 Silverlight 和控制台应用程序的代码示例，该服务器可以调用客户端方法](#wpfsl)

有关示例.NET 客户端项目，请参阅以下资源：

- [gustavo armenta / SignalR 示例](https://github.com/gustavo-armenta/SignalR-Samples)GitHub.com （WinRT、 Silverlight，控制台应用程序示例） 上。
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) GitHub.com （WPF 示例） 上。
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) GitHub.com （控制台应用程序示例） 上。

有关如何进行编程的服务器或 JavaScript 客户端的文档，请参阅以下资源：

- [SignalR 中心 API 指南-服务器](hubs-api-guide-server.md)
- [SignalR 中心 API 指南-JavaScript 客户端](hubs-api-guide-javascript-client.md)

API 参考主题的链接将指向 API 的.NET 4.5 版本。 如果你使用.NET 4，请参阅[API 主题的.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。

<a id="clientsetup"></a>

## <a name="client-setup"></a>客户端安装程序

安装[Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet 包 (不[Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)包)。 此包支持的.NET 4 和.NET 4.5 WinRT、 Silverlight、 WPF、 控制台应用程序和 Windows Phone 客户端。

如果对客户端的 SignalR 的版本必须在服务器的版本不同，SignalR 通常是能够适应差异。 例如，运行 SignalR 版本 2 的服务器将支持具有 1.1.x 安装客户端，以及具有版本 2 安装客户端。 如果服务器上的版本和客户端上的版本之间的差异是太大，或如果客户端与服务器较新，SignalR 引发`InvalidOperationException`客户端尝试建立连接时出现异常。 错误消息为"`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>如何建立连接

你可以建立连接之前，你必须创建`HubConnection`对象，并创建代理。 若要建立连接，请调用`Start`方法`HubConnection`对象。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> 对于 JavaScript 客户端必须注册至少一个事件处理程序之前调用`Start`方法，以建立连接。 这不是必需的.NET 客户端。 对于 JavaScript 客户端，生成的代理代码会自动创建为存在的所有中心的代理的服务器上，并注册处理程序是如何指示哪些中心你的客户端打算使用。 但是.NET 客户端您中心代理手动创建，因此 SignalR 假定，需要使用任何创建的代理的中心。


示例代码使用默认值"/ signalr"URL 以连接到你 SignalR 的服务。 有关如何指定不同的基 URL 的信息，请参阅[ASP.NET SignalR 中心 API 指南-服务器-/signalr URL](hubs-api-guide-server.md#signalrurl)。

`Start`方法以异步方式执行。 若要确保后面的代码行不执行直到建立连接后，使用`await`在 ASP.NET 4.5 异步方法或`.Wait()`中一种同步方法。 不要使用`.Wait()`WinRT 客户端中。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>来自 Silverlight 客户端的跨域连接

有关如何启用从 Silverlight 客户端的跨域连接的信息，请参阅[服务跨域边界提供](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)。

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>如何配置连接

建立连接之前，你可以指定任何以下选项：

- 并发连接限制。
- 查询字符串参数。
- 传输方法。
- HTTP 标头。
- 客户端证书。

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>如何在 WPF 客户端中设置的最大并发连接数

在 WPF 客户端，你可能需要增加从其默认值为 2 的并发连接的最大数目。 建议的值为 10。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

有关详细信息，请参阅[ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>如何指定查询字符串参数

如果你想要将数据发送到服务器，当客户端连接时，你可以将查询字符串参数添加到连接对象。 下面的示例演示如何在客户端代码中设置的查询字符串参数。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

下面的示例演示如何读取服务器代码中的查询字符串参数。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>如何指定传输方法

作为连接的过程的一部分，SignalR 客户端通常与服务器协商来确定支持的最佳传输由服务器和客户端。 如果你已经知道你想要使用的传输，你可以绕过此协商过程。 若要指定的传输方法，传入传输对象指向 Start 方法。 下面的示例演示如何在客户端代码中指定的传输方法。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)命名空间包括可用于指定的传输的以下类。

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) （可用仅当服务器和客户端使用.NET 4.5。）
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) （会自动选择支持的客户端和服务器的最佳传输。 这是默认传输。 通过此到`Start`方法具有相同的效果不传递任何内容。)

此列表中不包括 ForeverFrame 传输，因为它仅由浏览器使用。

有关如何检查在服务器代码中的传输方法的信息，请参阅[ASP.NET SignalR 中心 API 指南-服务器-如何从上下文属性中获取有关客户端的信息](hubs-api-guide-server.md#contextproperty)。 有关传输和回退机制的详细信息，请参阅[简介 SignalR 的传输和回退](../getting-started/introduction-to-signalr.md#transports)。

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>如何指定 HTTP 标头

若要设置 HTTP 标头，使用`Headers`连接对象上的属性。 下面的示例演示如何添加一个 HTTP 标头。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>如何指定客户端证书

若要添加客户端证书，请使用`AddClientCertificate`连接对象上的方法。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>如何创建中心代理

为了从服务器中，可以调用一个中心的客户端上定义方法，并调用在服务器上的集线器上的方法，通过调用创建的中心代理`CreateHubProxy`连接对象上。 字符串在传入到`CreateHubProxy`是你的中心类的名称或指定的名称`HubName`属性如果其中一个已在服务器上使用。 名称匹配不区分大小写。

**在服务器上的中心类**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**创建中心类用于客户端代理**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

如果修饰你中心类`HubName`特性，请使用该名称。

**在服务器上的中心类**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**创建中心类用于客户端代理**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

如果调用`HubConnection.CreateHubProxy`多次具有相同`hubName`，获取相同的缓存`IHubProxy`对象。

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>如何在该服务器可以调用的客户端上定义方法

若要定义该服务器可以调用一个方法，使用代理服务器的`On`方法以注册事件处理程序。

方法名称匹配不区分大小写。 例如，`Clients.All.UpdateStockPrice`在服务器上将执行`updateStockPrice`， `updatestockprice`，或`UpdateStockPrice`客户端上。

不同的客户端平台都具有不同的要求，有关如何编写方法的代码以更新 UI。 所示的示例适用于 WinRT (Windows 应用商店.NET) 客户端。 中提供了 WPF、 Silverlight 和控制台应用程序示例[本主题中后面的一个单独的段](#wpfsl)。

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>不带参数的方法

如果要处理的方法没有参数，使用的非泛型重载`On`方法：

**服务器代码调用不带参数的客户端方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**方法的 WinRT 客户端代码调用不带参数的服务器 ([请参阅本主题中的更高版本的 WPF 和 Silverlight 示例](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>具有指定参数类型的参数的方法

如果正在处理该方法具有参数，指定的参数类型作为泛型类型的`On`方法。 存在一些泛型重载`On`方法，以使您可以指定最多 8 个参数 (在 Windows Phone 7 上的 4)。 在以下示例中，一个参数发送到`UpdateStockPrice`方法。

**服务器代码调用带有参数的客户端方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**用于参数 Stock 类**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**服务器与一个参数调用的方法的 WinRT 客户端代码 ([请参阅本主题中的更高版本的 WPF 和 Silverlight 示例](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>使用指定的参数的动态对象的参数的方法

作为泛型类型的指定参数的替代方法`On`方法，你可以指定参数作为动态对象：

**服务器代码调用带有参数的客户端方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**用于参数 Stock 类**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**服务器与一个参数，参数中使用的动态对象调用方法的 WinRT 客户端代码 ([请参阅本主题中的更高版本的 WPF 和 Silverlight 示例](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>如何移除处理程序

若要移除处理程序，调用其`Dispose`方法。

**从服务器调用的方法的客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**客户端代码将处理程序移除**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>如何从客户端调用服务器方法

若要在服务器上调用方法，使用`Invoke`中心代理上的方法。

如果服务器方法没有返回值，使用的非泛型重载`Invoke`方法。

**服务器代码的方法没有返回值**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**客户端代码调用没有返回值的方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

如果服务器方法具有一个返回值，指定的返回类型的泛型类型作为`Invoke`方法。

**服务器代码具有一个返回值并采用复杂类型参数的方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**使用参数和返回值的 Stock 类**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**客户端代码调用方法，它具有一个返回值并采用 ASP.NET 4.5 的异步方法中的复杂类型参数，**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**客户端代码调用方法，它具有一个返回值并采用一种同步方法中的复杂类型参数，**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke`方法以异步方式执行，并返回`Task`对象。 如果没有指定`await`或`.Wait()`下, 一行代码将执行之前调用的方法执行完毕。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>如何处理连接生存期事件

SignalR 提供以下连接可以处理的生存期事件：

- `Received`： 引发时的连接上收到任何数据。 提供接收到的数据。
- `ConnectionSlow`： 当客户端检测到慢速或经常删除连接时引发。
- `Reconnecting`： 基础传输开始重新连接时引发。
- `Reconnected`： 基础传输已重新连接时引发。
- `StateChanged`： 在连接状态更改时引发。 提供的旧状态和新的状态。 有关连接状态的值请参阅[ConnectionState 枚举](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)。
- `Closed`： 连接已断开连接时引发。

例如，如果你想要显示的错误的不严重，但会导致间歇性连接问题的警告消息，如缓慢或频繁删除的连接，处理`ConnectionSlow`事件。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

有关详细信息，请参阅[了解和处理连接生存期中事件的 SignalR](handling-connection-lifetime-events.md)。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>如何处理错误

如果未显式启用服务器上的详细的错误消息，在出错后返回 SignalR 的异常对象包含有关错误的极少信息。 例如，如果调用`newContosoChatMessage`失败，错误对象中的错误消息包含"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"发送到在生产环境中的客户端的详细的错误消息不建议出于安全原因，但如果你想要启用详细的错误消息疑难解答的目的，在服务器上使用下面的代码。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

若要处理 SignalR 引发的错误，你可以添加的处理程序`Error`连接对象上的事件。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

若要处理方法调用中的错误，请在 try catch 块中包装代码。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>如何启用客户端日志记录

若要启用客户端日志记录，将设置`TraceLevel`和`TraceWriter`连接对象上的属性。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF、 Silverlight 和控制台应用程序的代码示例，该服务器可以调用客户端方法

用于定义该服务器可以调用的客户端方法的前面所示的代码示例适用于 WinRT 客户端。 下面的示例演示为 WPF、 Silverlight 和控制台应用程序客户端的等效代码。

### <a name="methods-without-parameters"></a>不带参数的方法

**不带参数的服务器从调用的方法的 WPF 客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**不带参数的服务器从调用的方法的 Silverlight 客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**从服务器不带参数调用方法的控制台应用程序客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>具有指定参数类型的参数的方法

**服务器与一个参数调用的方法的 WPF 客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Silverlight 客户端代码来调用服务器与一个参数的方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**控制台应用程序服务器与一个参数调用的方法的客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>使用指定的参数的动态对象的参数的方法

**服务器与一个参数，使用动态对象参数进行调用的方法的 WPF 客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Silverlight 客户端代码来调用服务器与一个参数，参数中使用的动态对象的方法**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**控制台应用程序服务器与一个参数，使用动态对象参数进行调用的方法的客户端代码**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
