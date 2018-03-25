---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: "SignalR 1.x 中心 API 指南-JavaScript 客户端 |Microsoft 文档"
author: pfletcher
description: "本文档提供使用中心 API 为 SignalR JavaScript 客户端，如浏览器和 Windows 应用商店 (WinJS) applic 中的版本 1.1 的说明..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>SignalR 1.x 中心 API 指南-JavaScript 客户端
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文档提供使用 SignalR JavaScript 客户端，如浏览器和 Windows 应用商店 (WinJS) 应用程序中的版本 1.1 的中心 API 的简介。
> 
> SignalR 中心 API 使您能够远程过程调用 (Rpc) 从连接的客户端到服务器和客户端到服务器。 在服务器代码中，你定义可以由客户端，调用的方法和调用客户端运行的方法。 在客户端代码中，定义在服务器上，可以调用的方法，并调用在服务器运行的方法。 SignalR 将负责为你的客户端到服务器管道的所有。
> 
> SignalR 还提供了一个称为永久连接的较低级别 API。 简介 SignalR、 集线器和永久连接，或者有关演示如何生成完整的 SignalR 应用程序的教程，请参阅[SignalR-入门](../getting-started/index.md)。


## <a name="overview"></a>概述

本文档包含以下各节：

- [生成的代理及它为你的作用](#genproxy)

    - [何时使用生成的代理](#cantusegenproxy)
- [客户端安装程序](#clientsetup)

    - [如何引用动态生成的代理](#dynamicproxy)
    - [如何为 SignalR 创建物理文件生成代理](#manualproxy)
- [如何建立连接](#establishconnection)

    - [$。 connection.hub 是相同对象该 $.hubConnection() 创建](#connequivalence)
    - [异步执行的 start 方法](#asyncstart)
- [如何建立的跨域连接](#crossdomain)
- [如何配置连接](#configureconnection)

    - [如何指定查询字符串参数](#querystring)
    - [如何指定传输方法](#transport)
- [如何获取代理的中心类](#getproxy)
- [如何在该服务器可以调用的客户端上定义方法](#callclient)
- [如何从客户端调用服务器方法](#callserver)
- [如何处理连接生存期事件](#connectionlifetime)
- [如何处理错误](#handleerrors)
- [如何启用客户端日志记录](#logging)

有关如何进行编程的服务器或.NET 客户端的文档，请参阅以下资源：

- [SignalR 中心 API 指南-服务器](../guide-to-the-api/hubs-api-guide-server.md)
- [SignalR 中心 API 指南-.NET 客户端](../guide-to-the-api/hubs-api-guide-net-client.md)

API 参考主题的链接将指向 API 的.NET 4.5 版本。 如果你使用.NET 4，请参阅[API 主题的.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>生成的代理及它为你的作用

你可以编程 JavaScript 客户端与 SignalR 服务使用或不使用 SignalR 将为你生成的代理进行通信。 代理为你的作用是代码的简化的语法使用连接，服务器调用时，写入方法并在服务器上调用方法。

当你编写代码，调用服务器方法调用时，生成的代理，您可以使用看起来好像你正在执行本地函数的语法： 可以编写`serverMethod(arg1, arg2)`而不是`invoke('serverMethod', arg1, arg2)`。 生成的代理语法还允许即时和容易理解客户端错误，如果键入服务器方法名称。 并且，如果你手动创建定义代理的文件，你还可以编写调用服务器方法的代码来获得 IntelliSense 支持。

例如，假设在服务器上有下面的中心类：

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

下面的代码示例演示 JavaScript 代码看起来像调用`NewContosoChatMessage`上服务器和接收的调用的方法`addContosoChatMessageToPage`从服务器的方法。

**使用生成代理**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**不生成代理**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>何时使用生成的代理

如果你想要注册的服务器调用的客户端方法的多个事件处理程序，则无法使用生成的代理。 否则为你可以选择使用生成的代理或不基于编码的首选项。 如果你选择不使用它，无需引用中的"signalr/中心"URL`script`在客户端代码中的元素。

<a id="clientsetup"></a>

## <a name="client-setup"></a>客户端安装程序

JavaScript 客户端需要对 jQuery 和 SignalR 的核心 JavaScript 文件的引用。 JQuery 版本必须是 1.6.4 或主要的更高版本，如 1.7.2、 1.8.2 或 1.9.1。 如果你决定使用生成的代理，你还需要对生成的 SignalR 代理 JavaScript 文件的引用。 下面的示例演示什么引用可能如下所示使用生成的代理的 HTML 页中。

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

必须按此顺序包含这些引用： jQuery 名字，在此之后，SignalR core 和 SignalR 代理姓氏。

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>如何引用动态生成的代理

在前面的示例中，生成的 SignalR 代理到引用的是动态生成的 JavaScript 代码，不是指向物理文件。 SignalR 为代理服务器在运行过程中创建的 JavaScript 代码，并提供给客户端以响应"/ signalr/中心"URL。 如果你指定的不同的基 URL 的 SignalR 连接在服务器上你`MapHubs`方法，动态生成的代理文件的 URL 是你使用的自定义 URL"/ 中心"追加到它。

> [!NOTE]
> 对于 Windows 8 （Windows 应用商店） JavaScript 客户端，而不是动态生成一个中使用的物理代理文件。 有关详细信息，请参阅[How to create for SignalR 的物理文件生成代理](#manualproxy)本主题中更高版本。


在 ASP.NET MVC 4 Razor 视图中，使用波形符来引用在您的代理文件引用中的应用程序根目录：

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

有关在 MVC 4 中使用 SignalR 的详细信息，请参阅[开始使用 SignalR 和 MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)。

在 ASP.NET MVC 3 Razor 视图中，使用`Url.Content`为你的代理文件引用：

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

在 ASP.NET Web 窗体应用程序，使用`ResolveClientUrl`为你的代理文件引用或通过使用应用程序根相对路径 （开头波形符） ScriptManager 注册，它：

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

作为一般规则，用于指定用于 CSS 或 JavaScript 文件"/ signalr/中心"URL 中使用相同的方法。 如果不使用波形符指定 URL，在某些情况下你的应用程序将正常工作时使用 IIS Express 的 Visual Studio 中的测试，但当你将部署到完整 IIS 时将失败，404 错误。 有关详细信息，请参阅**解析对资源的引用根级别**中[用于 ASP.NET Web 项目的 Visual Studio 中的 Web 服务器](https://msdn.microsoft.com/library/58wxa9w5.aspx)MSDN 网站上的。

当你在调试模式下，Visual Studio 2012 中运行 web 项目和作为浏览器使用 Internet Explorer，如果你可以看到中的代理文件**解决方案资源管理器**下**脚本文档**中, 所示下图。

![在解决方案资源管理器中的 JavaScript 生成的代理文件](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

若要查看文件的内容，请双击**中心**。 如果你不使用 Visual Studio 2012 和 Internet Explorer 中，或者如果你不在调试模式下，你还可以通过浏览到"/ signalR/中心"URL 获取文件的内容。 例如，如果你的站点运行在`http://localhost:56699`，请转到`http://localhost:56699/SignalR/hubs`在浏览器中。

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>如何为 SignalR 创建物理文件生成代理

作为动态生成的代理的替代方法，可以创建具有代理代码的物理文件并引用该文件。 你可能想要执行该操作控制缓存或绑定行为，或你在编码对服务器方法的调用时，可获取 IntelliSense。

若要创建代理文件，请执行以下步骤：

1. 安装[Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet 包。
2. 打开命令提示符，并浏览到*工具*包含 SignalR.exe 文件的文件夹。 工具文件夹位于以下位置：

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. 输入以下命令：

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    路径你*.dll*通常是*bin*项目文件夹中的文件夹。

    此命令创建名为的文件*server.js*在所在的文件夹*signalr.exe*。
4. Put *server.js*文件在项目中的相应文件夹中，将其重命名为适合你的应用程序，并添加对它代替"signalr/中心"引用的引用。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>如何建立连接

你可以建立连接之前，必须创建连接对象、 创建代理，并注册事件处理程序可以从服务器调用的方法。 当设置代理和事件处理程序时，通过调用建立连接`start`方法。

如果你使用的生成的代理，你无需在你自己的代码中创建连接对象，因为生成的代理代码执行此操作。

<a id="nogenconnection"></a>

**（与生成的代理） 建立连接**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**建立的连接 （而无需生成的代理）**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

示例代码使用默认值"/ signalr"URL 以连接到你 SignalR 的服务。 有关如何指定不同的基 URL 的信息，请参阅[ASP.NET SignalR 中心 API 指南-服务器-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。

> [!NOTE]
> 通常情况下注册事件处理程序之前调用`start`方法，以建立连接。 如果你想要建立连接后注册某些事件处理程序，你可以这样做，但你必须注册你之前调用的事件处理程序中至少一个`start`方法。 一个原因是在应用程序，可以有多个中心，但你不想要触发`OnConnected`如果你仅将使用到其中每个中心上的事件。 在中心的代理服务器上的客户端方法存在建立连接后，将告诉 SignalR 触发`OnConnected`事件。 如果你未注册任何事件处理程序之前调用`start`方法，你将能够对中心，但中心的调用的方法`OnConnected`不会调用方法并将从服务器调用任何客户端方法。


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$。 connection.hub 是相同对象该 $.hubConnection() 创建

如你所见从示例中，当你使用生成的代理，`$.connection.hub`引用的连接对象。 这是通过调用获取的相同对象`$.hubConnection()`时未使用生成的代理。 生成的代理代码通过执行下面的语句来创建你的连接：

![在生成的代理文件创建连接](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

当你使用的生成的代理时，您可以执行任何操作与`$.connection.hub`时你不使用生成的代理可以执行与连接对象。

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>异步执行的 start 方法

`start`方法以异步方式执行。 它将返回[jQuery 延迟对象](http://api.jquery.com/category/deferred-object/)，这意味着，你可以通过调用方法，如添加回调函数`pipe`， `done`，和`fail`。 如果你有想要在建立连接后，执行的代码，例如，对服务器方法调用，将该代码放在回调函数，或从一个回调函数调用它。 `.done`后已建立连接，和任何代码，之后必须执行回调方法你`OnConnected`执行完毕后在服务器上的事件处理程序方法。

如果将"现在已连接"语句从前面的示例作为下一步后的代码行放`start`方法调用 (不在`.done`回调)，则`console.log`行之前将执行建立连接时，在下面的示例所示示例：

![错误的方式编写运行建立连接后的代码](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>如何建立的跨域连接

通常如果浏览器加载从页`http://contoso.com`，SignalR 连接处于同一域中，在`http://contoso.com/signalr`。 如果此页`http://contoso.com`建立连接`http://fabrikam.com/signalr`，即跨域连接。 出于安全原因，默认情况下禁用跨域连接。 若要建立的跨域连接，请确保在服务器上，启用了跨域连接，在创建连接对象时指定该连接 URL。 SignalR 将使用适当的技术跨域连接，如[JSONP](http://en.wikipedia.org/wiki/JSONP)或[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)。

选择该选项，在调用时，在服务器上，启用跨域连接`MapHubs`方法。

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

在客户端上，指定 URL 时创建连接对象 （而不生成的代理） 或之前 （使用生成的代理） 中调用 start 方法。

**指定 （与生成的代理） 的跨域连接的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**指定 （无需生成的代理） 的跨域连接的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

当你使用`$.hubConnection`构造函数，你不需要包括`signalr`在 URL 中因为它将自动添加 (除非你指定`useDefaultUrl`作为`false`)。

你可以创建多个连接到不同的终结点。

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - 未设置`jQuery.support.cors`为 true，在代码中。
> 
>     ![未设置为 true 的 jQuery.support.cors](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR 处理使用 JSONP 还是 CORS。 设置`jQuery.support.cors`以 true 禁用 JSONP，因为它会导致 SignalR 假定浏览器支持 CORS。
> - 当您正在连接到本地主机 URL 时，Internet Explorer 10 不会考虑将它的跨域连接，因此应用程序将使用本地 IE 10 即使尚未启用该服务器上的跨域连接。
> - 有关使用 Internet Explorer 9 的跨域连接的信息的信息，请参阅[此 StackOverflow 线程](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)。
> - 有关使用 Chrome 的跨域连接的信息的信息，请参阅[此 StackOverflow 线程](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)。
> - 示例代码使用默认值"/ signalr"URL 以连接到你 SignalR 的服务。 有关如何指定不同的基 URL 的信息，请参阅[ASP.NET SignalR 中心 API 指南-服务器-/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)。


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>如何配置连接

建立连接之前，你可以指定查询字符串参数或指定的传输方法。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>如何指定查询字符串参数

如果你想要将数据发送到服务器，当客户端连接时，你可以将查询字符串参数添加到连接对象。 下面的示例演示如何在客户端代码中设置的查询字符串参数。

**（使用生成的代理） 调用 start 方法前设置查询字符串值**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**在调用 （无需生成的代理） 的启动方法之前设置查询字符串值**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

下面的示例演示如何读取服务器代码中的查询字符串参数。

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>如何指定传输方法

作为连接的过程的一部分，SignalR 客户端通常与服务器协商来确定支持的最佳传输由服务器和客户端。 如果你已经知道你想要使用的传输，你可以通过在调用时指定的传输方法来跳过此协商过程`start`方法。

**指定的传输方法 （使用生成的代理） 的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**指定的传输方法 （而无需生成的代理） 的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

作为替代方法，你可以在其中你希望 SignalR 尝试它们的顺序来指定多个传输方法：

**指定自定义传输回退方案 （使用生成的代理） 的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**指定自定义传输回退方案 （无需生成的代理） 的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

用于指定的传输方法，可以使用以下值：

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

下面的示例演示如何找出连接正在使用哪种传输方法。

**显示使用 （与生成的代理） 的连接的传输方法的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**显示使用 （无需生成的代理） 的连接的传输方法的客户端代码**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

有关如何检查在服务器代码中的传输方法的信息，请参阅[ASP.NET SignalR 中心 API 指南-服务器-如何从上下文属性中获取有关客户端的信息](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)。 有关传输和回退机制的详细信息，请参阅[简介 SignalR 的传输和回退](../getting-started/introduction-to-signalr.md#transports)。

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>如何获取代理的中心类

你创建的每个连接对象封装有关连接到包含一个或多个中心类 SignalR 服务的信息。 若要与中心类通信，你使用代理对象 （如果你不使用生成的代理），会创建您自己或为你生成。

客户端上的代理名称是中心类名称采用 camel 大小写版本。 SignalR 自动使此更改，以便 JavaScript 代码可以符合 JavaScript 约定。

**在服务器上的中心类**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**获取对生成的客户端代理的引用的中心**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**创建中心类 （而不生成的代理） 的客户端代理**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

如果修饰你中心类`HubName`特性，请使用的确切名称，而无需更改大小写。

**具有 HubName 属性的服务器上的中心类**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**获取对生成的客户端代理的引用的中心**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**创建中心类 （而不生成的代理） 的客户端代理**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>如何在该服务器可以调用的客户端上定义方法

若要定义服务器可从一个中心中调用的方法，通过将添加一个事件处理程序到中心代理`client`属性生成的代理，或调用`on`方法如果你未使用生成的代理。 参数可以是复杂的对象。

添加事件处理程序，然后才能调用`start`方法，以建立连接。 (如果你想要在调用后添加事件处理程序`start`方法，请参阅中的说明[如何建立连接](#establishconnection)前面在此文档，并使用用于定义一种方法，而无需使用生成的代理所示的语法。)

方法名称匹配不区分大小写。 例如，`Clients.All.addContosoChatMessageToPage`在服务器上将执行`AddContosoChatMessageToPage`， `addContosoChatMessageToPage`，或`addcontosochatmessagetopage`客户端上。

**（使用生成的代理） 的客户端上定义方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**另一种方法 （使用生成的代理） 的客户端上定义方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**客户端 （不生成的代理，或调用 start 方法之后添加时） 上定义方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**调用客户端方法的服务器代码**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

下面的示例包括复杂对象作为方法参数。

**采用 （具有生成的代理） 的复杂对象的客户端上定义方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**采用复杂对象 （而不生成的代理） 的客户端上定义方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**定义的复杂对象的服务器代码**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**服务器代码，调用使用复杂对象的客户端方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>如何从客户端调用服务器方法

若要从客户端调用服务器方法，使用`server`属性生成的代理或`invoke`中心代理，如果你未使用生成的代理上的方法。 返回值或参数可以是复杂的对象。

在中心传入方法名称的 camel 大小写版本。 SignalR 自动使此更改，以便 JavaScript 代码可以符合 JavaScript 约定。

下面的示例演示如何调用没有返回值的服务器方法以及如何调用服务器方法没有返回值。

**没有 HubMethodName 属性与服务器方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**在参数中传递定义的复杂对象的服务器代码**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**客户端代码调用 （使用生成的代理） 服务器的方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**客户端代码调用 （无需生成的代理） 服务器的方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

如果修饰具有的中心方法`HubMethodName`特性，请使用该名称，而无需更改大小写。

**服务器方法**具有 HubMethodName 属性

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**客户端代码调用 （使用生成的代理） 服务器的方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**客户端代码调用 （无需生成的代理） 服务器的方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

前面的示例演示如何调用没有返回值的服务器方法。 下面的示例演示如何调用具有一个返回值的服务器方法。

**服务器代码具有一个返回值的方法**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Stock 类用于**返回值

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**客户端代码调用 （使用生成的代理） 服务器的方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**客户端代码调用 （无需生成的代理） 服务器的方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>如何处理连接生存期事件

SignalR 提供以下连接可以处理的生存期事件：

- `starting`： 通过连接发送任何数据之前引发。
- `received`： 引发时的连接上收到任何数据。 提供接收到的数据。
- `connectionSlow`： 当客户端检测到慢速或经常删除连接时引发。
- `reconnecting`： 基础传输开始重新连接时引发。
- `reconnected`： 基础传输已重新连接时引发。
- `stateChanged`： 在连接状态更改时引发。 提供的旧的状态和新的状态 （连接、 已连接、 Reconnecting，或已断开连接）。
- `disconnected`： 连接已断开连接时引发。

例如，如果你想要有可能会导致明显延迟的连接问题时显示警告消息，处理`connectionSlow`事件。

**句柄 connectionSlow 事件 （使用生成的代理）**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**句柄 connectionSlow 事件 （而无需生成的代理）**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

有关详细信息，请参阅[了解和处理连接生存期中事件的 SignalR](index.md)。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>如何处理错误

SignalR JavaScript 客户端提供`error`可以添加的处理程序的事件。 失败方法还可用于添加而从服务器方法调用导致的错误处理程序。

如果未显式启用服务器上的详细的错误消息，在出错后返回 SignalR 的异常对象包含有关错误的极少信息。 例如，如果调用`newContosoChatMessage`失败，错误对象中的错误消息包含"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"发送到在生产环境中的客户端的详细的错误消息不建议出于安全原因，但如果你想要启用详细的错误消息疑难解答的目的，在服务器上使用下面的代码。

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

下面的示例演示如何添加错误事件的处理程序。

**添加一个错误处理程序 （具有生成的代理）**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**添加错误处理程序 （而无需生成的代理）**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

下面的示例演示如何处理的方法调用中出现错误。

**句柄 （使用生成的代理） 的方法调用中出现错误**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**句柄 （无需生成的代理） 的方法调用中出现错误**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

如果方法调用失败，`error`也会引发事件，因此你的代码中`error`方法处理程序并在`.fail`方法回调会执行。

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>如何启用客户端日志记录

若要启用客户端日志记录在连接上，设置`logging`属性上的连接对象，然后才能调用`start`方法，以建立连接。

**启用日志记录 （使用生成的代理）**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**启用日志记录 （无需生成的代理）**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

若要查看日志，请打开浏览器的开发人员工具，并转到控制台选项卡。本教程演示的分步说明和屏幕截图显示如何执行此操作，请参阅[服务器广播使用 ASP.NET Signalr-启用日志记录](index.md)。
