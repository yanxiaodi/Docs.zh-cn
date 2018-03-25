---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: "ASP.NET SignalR 中心 API 指南-服务器 (C#) |Microsoft 文档"
author: pfletcher
description: "本文档介绍了使用代码示例演示为 SignalR 版本 2，编程 ASP.NET SignalR 中心 API 的服务器端..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>ASP.NET SignalR 中心 API 指南-服务器 (C#)
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文档提供使用代码示例演示常见的选项为 SignalR 版本 2，编程 ASP.NET SignalR 中心 API 的服务器端的简介。
> 
> SignalR 中心 API 使您能够远程过程调用 (Rpc) 从连接的客户端到服务器和客户端到服务器。 在服务器代码中，你定义可以由客户端，调用的方法和调用客户端运行的方法。 在客户端代码中，定义在服务器上，可以调用的方法，并调用在服务器运行的方法。 SignalR 将负责为你的客户端到服务器管道的所有。
> 
> SignalR 还提供了一个称为永久连接的较低级别 API。 有关 SignalR、 集线器和永久连接的介绍，请参阅[简介 SignalR 2](../getting-started/introduction-to-signalr.md)。
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
> ## <a name="topic-versions"></a>主题版本
> 
> 有关 SignalR 的早期版本的信息，请参阅[SignalR 较旧版本](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>概述

本文档包含以下各节：

- [如何注册 SignalR 中间件](#route)

    - [/Signalr URL](#signalrurl)
    - [配置 SignalR 选项](#options)
- [如何创建和使用 Hub 类](#hubclass)

    - [中心对象生存期](#transience)
    - [混合使用大小写的 JavaScript 客户端中的中心名称](#hubnames)
    - [多个中心](#multiplehubs)
    - [强类型中心](#stronglytypedhubs)
- [如何在客户端可以调用的中心类中定义方法](#hubmethods)

    - [混合使用大小写的 JavaScript 客户端中的方法名称](#methodnames)
    - [当以异步方式执行](#asyncmethods)
    - [定义重载](#overloads)
    - [报告从中心方法调用的进度](#progress)
- [如何从中心类方法调用客户端](#callfromhub)

    - [选择哪些客户端将接收 RPC](#selectingclients)
    - [方法名称没有编译时验证](#dynamicmethodnames)
    - [不区分大小写的方法名称匹配](#caseinsensitive)
    - [异步执行](#asyncclient)
- [如何从中心类管理组成员身份](#groupsfromhub)

    - [异步执行的 Add 和 Remove 方法](#asyncgroupmethods)
    - [组成员身份持久性](#grouppersistence)
    - [单用户组](#singleusergroups)
- [如何连接生存期中处理事件的中心类](#connectionlifetime)

    - [当调用 OnConnected、 OnDisconnected 和 OnReconnected](#onreconnected)
    - [不填充的调用方状态](#nocallerstate)
- [如何从上下文属性中获取有关客户端的信息](#contextproperty)
- [如何将状态传递客户端和中心类之间](#passstate)
- [如何处理中心类中的错误](#handleErrors)
- [如何调用方法的客户端和管理从中心类外部的组](#callfromoutsidehub)

    - [调用客户端方法](#callingclientsoutsidehub)
    - [管理组成员资格](#managinggroupsoutsidehub)
- [如何启用跟踪](#tracing)
- [如何自定义中心管道](#hubpipeline)

有关如何程序客户端的文档，请参阅以下资源：

- [SignalR 中心 API 指南-JavaScript 客户端](hubs-api-guide-javascript-client.md)
- [SignalR 中心 API 指南-.NET 客户端](hubs-api-guide-net-client.md)

SignalR 2 的服务器组件才.NET 4.5 中提供。 运行.NET 4.0 的服务器必须使用 SignalR 1.x 版。

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>如何注册 SignalR 中间件

若要定义的路由的客户端将用于连接到你的中心，调用`MapSignalR`应用程序启动时的方法。 `MapSignalR`是[扩展方法](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)为`OwinExtensions`类。 下面的示例演示如何定义使用 OWIN startup 类 SignalR 中心路由。

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

如果你将 SignalR 功能添加到 ASP.NET MVC 应用程序，请确保在其他路由之前已添加的 SignalR 路由。 有关详细信息，请参阅[教程： 开始使用 SignalR 2 和 MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>/Signalr URL

默认情况下，客户端将用于连接到你的中心路由 URL 是"/ signalr"。 （不要将此 URL 以"/ signalr/中心"URL，即自动生成的 JavaScript 文件混淆。 有关生成的代理的详细信息，请参阅[SignalR 中心 API 指南-JavaScript 客户端-生成的代理和它为您完成](hubs-api-guide-javascript-client.md#genproxy)。)

可能有特殊的情况下，它让 SignalR; 不能使用此基 URL例如，你有一个文件夹中名为的项目*signalr*并且你不想要更改的名称。 在这种情况下，更改基 URL，如下面的示例中所示 (将"/ signalr"与你所需的 URL 的示例代码中)。

**指定的 URL 的服务器代码**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**指定的 URL （与生成的代理） 的 JavaScript 客户端代码**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**指定的 URL （无需生成的代理） 的 JavaScript 客户端代码**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**指定的 URL 的.NET 客户端代码**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>配置 SignalR 选项

重载的`MapSignalR`方法使您能够指定的自定义 URL、 自定义的依赖项解析程序和以下选项：

- 启用从浏览器客户端使用 CORS 或 JSONP 的跨域调用。

    通常如果浏览器加载从页`http://contoso.com`，SignalR 连接处于同一域中，在`http://contoso.com/signalr`。 如果此页`http://contoso.com`建立连接`http://fabrikam.com/signalr`，即跨域连接。 出于安全原因，默认情况下禁用跨域连接。 有关详细信息，请参阅[ASP.NET SignalR 中心 API 指南-JavaScript 客户端-如何建立的跨域连接](hubs-api-guide-javascript-client.md#crossdomain)。
- 启用详细的错误消息。

    发生错误时，SignalR 的默认行为是发送到客户端通知消息，而有关发生了什么情况的详细信息。 向客户端发送详细的错误信息不是建议在生产中，因为恶意用户可能能够使用攻击你的应用程序中的信息。 有关故障排除，可以使用此选项以暂时启用更详细的错误报告。
- 禁用自动生成的 JavaScript 代理文件。

    默认情况下，以响应的 URL"/ signalr/中心"生成你的中心类与代理的 JavaScript 文件。 如果不想使用 JavaScript 代理，或者如果你想要手动生成此文件并引用的物理文件中你的客户端，你可以使用此选项以禁用代理生成。 有关详细信息，请参阅[SignalR 中心 API 指南-JavaScript 客户端-如何为 SignalR 创建物理文件生成代理](hubs-api-guide-javascript-client.md#manualproxy)。

下面的示例演示如何对的调用中指定的 SignalR 连接 URL 和这些选项`MapSignalR`方法。 若要指定自定义 URL，将"/ signalr"在与你想要使用的 URL 的示例。

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>如何创建和使用 Hub 类

若要创建一个中心，创建派生自的类[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)。 下面的示例演示一个简单的中心类聊天应用程序。

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

在此示例中，连接的客户端可以调用`NewContosoChatMessage`方法，和时，收到的数据已将广播到所有连接的客户端。

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>中心对象生存期

未实例化中心类，或从服务器; 上代码中调用其方法来说，为你完成通过 SignalR 中心管道。 SignalR 在每次它需要处理中心操作，例如当客户端连接、 断开连接，或向服务器发出调用的方法时创建中心类的新实例。

因为中心类的实例是暂时的不能使用它们来维护到下一个方法调用中的状态。 每次服务器接收方法调用从客户端，你的中心类进程的新实例的消息。 若要维护通过多个连接和方法调用的状态，使用如数据库或一个静态变量的其他某种方法上的中心类或不同类的不是派生自`Hub`。 如果您保留在内存中的数据，使用静态变量之类的方法上的中心类，数据将丢失当应用程序域进行回收时。

如果你想要将消息发送到客户端，从你自己的中心类外部运行的代码，则不能通过实例化的中心类实例，但你可以执行此操作通过为您的中心类获取对 SignalR 上下文对象的引用。 有关详细信息，请参阅[如何调用方法的客户端和管理从中心类外部的组](#callfromoutsidehub)本主题中更高版本。

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>混合使用大小写的 JavaScript 客户端中的中心名称

默认情况下，JavaScript 客户端到中心通过使用类名称的混合使用大小写版本引用。 SignalR 自动使此更改，以便 JavaScript 代码可以符合 JavaScript 约定。 前面的示例将称为`contosoChatHub`在 JavaScript 代码。

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**使用生成的代理的 JavaScript 客户端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

如果你想要指定客户端使用，添加另一个名称`HubName`属性。 当你使用`HubName`属性，没有任何名称更改为 JavaScript 客户端上的 camel 大小写。

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**使用生成的代理的 JavaScript 客户端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>多个中心

应用程序中，可以定义多个中心类。 当你这样做时，共享的连接，但组是单独：

- 所有客户端将使用相同的 URL 来建立与你的服务的 SignalR 连接 ("/ signalr"或如果你指定一个你自定义 URL)，并且所有中心使用连接的服务定义。

    与单个类中定义所有中心功能的多个中心没有性能差异。
- 所有中心都获取相同的 HTTP 请求信息。

    由于所有中心都共享相同的连接，服务器获取的唯一的 HTTP 请求信息是什么进入时建立 SignalR 连接的原始 HTTP 请求。 如果你使用连接请求将信息从客户端传递到服务器，通过指定查询字符串，不能向不同的中心提供不同的查询字符串。 所有中心将都收到相同的信息。
- 生成的 JavaScript 代理文件将包含在一个文件中的所有中心的代理。

    有关 JavaScript 代理的信息，请参阅[SignalR 中心 API 指南-JavaScript 客户端-生成的代理和它为您完成](hubs-api-guide-javascript-client.md#genproxy)。
- 在中心内定义组。

    SignalR 可以定义在名为组以将广播到连接的客户端的子集。 组都会单独维护每个中心。 例如，一个名为"管理员"组应包括的客户端的一组你`ContosoChatHub`类和相同的组名称所指一组不同的客户端你`StockTickerHub`类。

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>强类型中心

若要定义一个接口，使你的中心方法，你的客户端可以引用 （和启用 Intellisense 在你的中心方法），派生你的中心`Hub<T>`（SignalR 2.1 中引入） 而非`Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>如何在客户端可以调用的中心类中定义方法

若要公开你想要从客户端调用的中心上的方法，声明一个公共方法，如下面的示例中所示。

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

你可以指定返回类型和参数，包括复杂类型并对数组，就像在任何 C# 方法。 客户端和服务器之间进行通信的接收参数或返回到调用方的任何数据是通过使用 JSON，和 SignalR 自动处理复杂对象的绑定和对象的数组。

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>混合使用大小写的 JavaScript 客户端中的方法名称

默认情况下，JavaScript 客户端到中心方法通过使用的方法名称的混合使用大小写版本引用。 SignalR 自动使此更改，以便 JavaScript 代码可以符合 JavaScript 约定。

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**使用生成的代理的 JavaScript 客户端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

如果你想要指定客户端使用，添加另一个名称`HubMethodName`属性。

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**使用生成的代理的 JavaScript 客户端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>当以异步方式执行

如果该方法将是长时间运行或已进行的工作将涉及等待，如数据库查找或 web 服务调用，请使中心方法异步通过返回[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(代替了`void`返回) 或[任务&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)对象 (代替了`T`返回类型)。 当您返回`Task`从 SignalR 方法的对象会等待`Task`若要完成，然后发送解包的结果返回到客户端，以便在客户端中的方法调用的代码没有差异。

使得的中心方法异步可避免阻止使用 WebSocket 传输时的连接。 当传输为 WebSocket Hub 方法以同步方式执行，直到中心方法完成之后将阻止从同一个客户端集线器上的方法的后续调用。

下面的示例演示相同的方法编码为异步运行，或以异步方式后, 跟适用于调用任一版本的 JavaScript 客户端代码。

**同步**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**异步**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**使用生成的代理的 JavaScript 客户端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

有关如何在 ASP.NET 4.5 中使用异步方法的详细信息，请参阅[使用 ASP.NET MVC 4 中的异步方法](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。

<a id="overloads"></a>

### <a name="defining-overloads"></a>定义重载

如果你想要定义的方法的重载，每个重载中的参数数目必须不同。 如果你只需通过指定不同的参数类型区分重载方法，你的中心类能够编译，但 SignalR 服务将引发当客户端尝试的运行时异常时调用的重载之一。

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>报告从中心方法调用的进度

SignalR 2.1 添加了对支持[进度报告模式](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx).NET 4.5 中引入。 若要实现进度报告，定义`IProgress<T>`为你的客户端可以访问你中心方法的参数：

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

当编写的长时间运行 server 方法，务必使用异步编程模式类似异步 / 等待而不是阻止中心线程。

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>如何从中心类方法调用客户端

若要从服务器中调用客户端方法，使用`Clients`中心类方法中的属性。 下面的示例演示调用的服务器代码`addNewMessageToPage`所有连接的客户端和 JavaScript 客户端中定义的方法的客户端代码。

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**使用生成的代理的 JavaScript 客户端**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

无法从客户端方法; 获取返回值语法如下`int x = Clients.All.add(1,1)`不起作用。

你可以指定复杂类型和参数的数组。 下面的示例将复杂类型传递到方法参数中的客户端。

**调用使用复杂对象的客户端方法的服务器代码**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**定义的复杂对象的服务器代码**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**使用生成的代理的 JavaScript 客户端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>选择哪些客户端将接收 RPC

客户端属性返回[HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)提供若干选项用于指定哪些客户端将接收 RPC 的对象：

- 所有连接的客户端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- 只有调用客户端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- 除调用客户端以外的所有客户端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- 特定客户端由连接 ID 标识。

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    此示例调用`addContosoChatMessageToPage`上调用的客户端并且具有与使用相同的效果`Clients.Caller`。
- 所有连接的客户端除外指定客户端，并由连接 ID 标识。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- 在指定的组的所有连接的客户端。

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- 在指定的组的所有连接的客户端除外指定客户端，并由连接 ID 标识。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- 在指定的组的所有连接的客户端除非调用的客户端。

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- 用户 Id 标识特定用户。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    默认情况下，这是`IPrincipal.Identity.Name`，但这可以通过更改[与全局主机注册的 IUserIdProvider 实现](mapping-users-to-connections.md#IUserIdProvider)。
- 所有客户端和组列表中的连接 Id。

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- 组的列表。

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- 按名称的用户。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- （在 SignalR 2.1 中引入） 的用户名称的列表。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>方法名称没有编译时验证

你指定的方法名称被解释为动态对象，这意味着没有 IntelliSense 或它的编译时验证。 在运行时计算表达式。 当方法调用执行时，SignalR 将方法名称和参数值发送到客户端，并在客户端有一种方法的名称相匹配，方法被称为和参数值将传递给它。 如果客户端上不找到任何匹配方法，则不会引发错误。 有关格式的数据的 SignalR 将传输到客户端在后台调用客户端方法时的信息，请参阅[简介 SignalR](../getting-started/introduction-to-signalr.md)。

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>不区分大小写的方法名称匹配

方法名称匹配不区分大小写。 例如，`Clients.All.addContosoChatMessageToPage`在服务器上将执行`AddContosoChatMessageToPage`， `addcontosochatmessagetopage`，或`addContosoChatMessageToPage`客户端上。

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>异步执行

调用此方法以异步方式执行。 提供对客户端的方法调用将立即执行而无需等待 SignalR 完成传输到客户端的数据，除非另行指定，后面的代码行应等待方法完成后的任何代码。 下面的代码示例演示如何以按顺序执行两个客户端方法。

**使用 Await (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

如果你使用`await`等待直到客户端方法完成之前执行下一步一行代码，这并不表示客户端在执行下的一行代码之前，将实际收到的消息。 "完成"的客户端方法调用仅意味着 SignalR 已完成所需将消息发送的一切。 如果你需要验证客户端收到消息，你必须自己进行编程该机制。 例如，无法代码`MessageReceived`方法的中心，并在`addContosoChatMessageToPage`方法在客户端可以调用`MessageReceived`执行完操作后任何起作用，您需要在客户端上执行操作。 在`MessageReceived`中心中你可以执行任何工作取决于实际的客户端接收和处理的原始方法调用。

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>如何使用作为方法名称的字符串变量

如果你想要通过使用作为强制转换的方法名称的字符串变量调用客户端方法`Clients.All`(或`Clients.Others`，`Clients.Caller`等) 到`IClientProxy`，然后调用[Invoke （方法名称、 参数...）](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>如何从中心类管理组成员身份

SignalR 中的组提供一种方法将消息广播到连接的客户端的指定子集。 组可以具有任意数量的客户端，并且客户端可以是任意数量的组的成员。

若要管理组成员身份，使用[添加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)和[删除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)提供方法`Groups`中心类的属性。 下面的示例演示`Groups.Add`和`Groups.Remove`方法在由客户端代码调用的中心方法中使用跟调用它们的 JavaScript 客户端代码。

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**使用生成的代理的 JavaScript 客户端**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

你无需显式创建组。 实际上自动创建组对的调用中指定其名称的第一个时间`Groups.Add`，并将其删除从中它的成员身份中删除最后一次连接时。

用于获取组成员身份列表或组的列表中没有任何 API。 SignalR 将消息发送到客户端和基于组[发布/订阅模型](http://en.wikipedia.org/wiki/Publish/subscribe)，而服务器不保持组或组成员身份的列表。 这有助于最大程度提高可伸缩性，因为每当向 web 场添加节点时，SignalR 维护任何状态都将传播到新节点。

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>异步执行的 Add 和 Remove 方法

`Groups.Add`和`Groups.Remove`方法异步执行。 如果你想要将客户端添加到组并立即向客户端发送一条消息，通过使用组，则必须确保`Groups.Add`方法先完成。 下面的代码示例演示如何执行该操作。

**向组添加客户端，然后消息传送该客户端**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>组成员身份持久性

SignalR 跟踪的连接，不是用户，因此，如果你希望某个用户是同一组中每次用户建立的连接，则必须调用`Groups.Add`每次用户建立新连接。

后的连接暂时中断，有时 SignalR 可以还原该连接自动。 在这种情况下，SignalR 正在还原相同的连接，不建立新连接，并因此客户端的组成员身份会自动还原。 这是可能甚至临时中断时重新启动服务器或发生故障，结果因为对于每个客户端，包括组成员身份的连接状态是往返到客户端。 如果一台服务器出现故障，并且都替换为新的服务器连接超时之前，客户端可以自动重新连接到新服务器，并重新注册它是成员的组中。

当不能在连接丢失后自动恢复连接或连接超时时，或客户端断开连接 （例如，当浏览器导航到新页） 时，组成员身份都将丢失。 在下次用户连接的时将新的连接。 若要维护组成员身份，当同一个用户建立新连接时，将应用程序来跟踪用户和组之间的关联和还原每次用户建立新连接的组成员身份。

有关连接和重新连接的详细信息，请参阅[如何连接生存期中处理事件的中心类](#connectionlifetime)本主题中更高版本。

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>单用户组

通常使用 SignalR 的应用程序需要跟踪的用户连接之间的关联才能知道哪些用户已发送一条消息并哪些用户应该能够接收一条消息。 组用于在两种常用模式之一中执行该操作。

- 单用户组。

    可以将用户名称指定为组名称，并将当前的连接 ID 添加到组，每次用户连接或重新连接。 若要将消息发送到发送到组的用户。 此方法的缺点是组不为您提供了一种方法，若要了解是否用户是联机还是脱机。
- 跟踪用户名称和连接 Id 之间的关联。

    你可以将每个用户名称和一个或多个连接 Id 之间的关联存储在字典或数据库，并更新每次用户连接或断开连接的存储的数据。 若要将消息发送到用户你指定的连接 Id。 此方法的缺点是它采用更多内存。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>如何连接生存期中处理事件的中心类

适用于处理连接生存期事件的典型原因是能够跟踪是否某位用户连接，并跟踪的用户名称和连接 Id 之间的关联。 若要运行你自己的代码，客户端连接或断开连接时，重写`OnConnected`， `OnDisconnected`，和`OnReconnected`虚方法的中心类，如下面的示例中所示。

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>当调用 OnConnected、 OnDisconnected 和 OnReconnected

浏览器导航到新页，每次新的连接已建立，这意味着将执行 SignalR`OnDisconnected`方法跟`OnConnected`方法。 建立新连接时，SignalR 始终会创建新的连接 ID。

`OnReconnected` SignalR 可以从自动恢复，如当电缆暂时断开连接后又重新连接超时之前连接的连接已临时中断时，调用方法。`OnDisconnected`时客户端断开连接并且 SignalR 无法自动重新连接，如当浏览器导航到新页调用方法。 因此，给定的客户端事件的可能序列是`OnConnected`， `OnReconnected`， `OnDisconnected`; 或`OnConnected`， `OnDisconnected`。 你不会看到序列`OnConnected`， `OnDisconnected`，`OnReconnected`针对给定连接。

`OnDisconnected`某些情况下，例如服务器出现故障时不会调用方法或应用程序域获取回收。 当另一台服务器变为联机或应用程序域完成其回收时，某些客户端可能无法重新连接并激发`OnReconnected`事件。

有关详细信息，请参阅[了解和处理连接生存期中事件的 SignalR](handling-connection-lifetime-events.md)。

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>不填充的调用方状态

连接生存期事件处理程序调用这些方法，在服务器上，这意味着将放在任何状态`state`客户端上的对象将不会填充`Caller`服务器上的属性。 璝惠`state`对象和`Caller`属性，请参阅[如何将状态传递客户端和中心类之间](#passstate)本主题中更高版本。

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>如何从上下文属性中获取有关客户端的信息

若要获取有关客户端的信息，请使用`Context`中心类的属性。 `Context`属性返回[HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)对象提供了访问权的以下信息：

- 调用的客户端连接 ID。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    连接 ID 是由 SignalR （这无法在你自己的代码中指定值） 分配的 GUID。 没有为每个连接，并且如果你有多个中心应用程序中，所有中心都使用 ID 的相同连接的一个连接 ID。
- HTTP 标头数据。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    你还可以获取 HTTP 标头从`Context.Headers`。 将相同内容的多个引用的原因在于`Context.Headers`首先，创建`Context.Request`更高版本，已添加属性和`Context.Headers`被保留用于向后兼容性。
- 查询字符串数据。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    你还可以获取查询字符串数据从`Context.QueryString`。

    获取此属性中的查询字符串是用于建立 SignalR 连接的 HTTP 请求。 通过配置的连接，这是一种简便方式从客户端向服务器传递有关客户端的数据，可以在客户端添加查询字符串参数。 下面的示例演示当你使用生成的代理时，JavaScript 客户端中添加的查询字符串的一种方法。

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    有关设置查询字符串参数的详细信息，请参阅针对的 API 指南[JavaScript](hubs-api-guide-javascript-client.md)和[.NET](hubs-api-guide-net-client.md)客户端。

    你可以找到用于在查询字符串数据中，供内部使用 SignalR 的一些其他值一起连接的传输方法：

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    值`transportMethod`将"webSockets"、"serverSentEvents"、"foreverFrame"或"longPolling"。 请注意，如果此值签入`OnConnected`事件处理程序方法中，在某些情况下可能一开始就会传输该值不是连接的最终协商的传输方法。 在这种情况下方法将引发异常，建立最终传输方法时进行调用的试更高版本。
- Cookie。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    你还可以获得的 cookie `Context.RequestCookies`。
- 用户信息。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- 请求的 HttpContext 对象：

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    使用此方法，而不是获得`HttpContext.Current`获取`HttpContext`SignalR 连接的对象。

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>如何将状态传递客户端和中心类之间

客户端代理提供`state`可以在其中存储要传输到每个方法调用的服务器的数据的对象。 可以在服务器上访问此数据`Clients.Caller`由客户端调用的中心方法中的属性。 `Clients.Caller`连接生存期事件处理程序方法将不填充属性`OnConnected`， `OnDisconnected`，和`OnReconnected`。

创建或更新中的数据`state`对象和`Clients.Caller`属性在两个方向起作用。 您可以更新服务器中的值和它们会传递回客户端。

下面的示例演示将以发送给每个方法调用的服务器的状态存储的 JavaScript 客户端代码。

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

下面的示例演示的.NET 客户端中的等效代码。

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

在中心类中，你可以访问此数据`Clients.Caller`属性。 下面的示例显示检索在前面的示例中称为的状态的代码。

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> 状态持久化此机制不适用于大量数据，因为所有内容放在`state`或`Clients.Caller`属性是往返与每个方法调用。 它可用于较小的项，如用户名称或计数器。


在 VB.NET 或强类型的中心中，无法通过访问调用方状态对象`Clients.Caller`; 相反，使用`Clients.CallerState`（SignalR 2.1 中引入）：

**在 C# 中使用 CallerState**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**在 Visual Basic 中使用 CallerState**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>如何处理中心类中的错误

若要处理在中心类方法中发生的错误，请使用一个或多个以下方法：

- 在 try-catch 块中包装你的方法代码和日志的异常对象。 出于调试目的可以将异常发送到客户端，但出于安全原因详细的信息发送给在生产环境中的客户端不建议。
- 创建处理的中心管道模块[OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)方法。 下面的示例演示的管道模块，记录错误，跟模块注入中心管道的 Startup.cs 中的代码。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- 使用`HubException`（SignalR 2 中引入） 的类。 可以从任何中心调用引发此错误。 `HubError`构造函数采用一个字符串消息和一个对象，用于存储额外的错误数据。 SignalR 将自动序列化异常，并将其发送到客户端，其中它将用于拒绝或失败中心方法调用。

    下面的代码示例演示如何引发`HubException`期间的中心调用，以及如何处理在 JavaScript 和.NET 客户端上的异常。

    **服务器代码演示 HubException 类**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **演示在一个中心中引发 HubException 响应的 JavaScript 客户端代码**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **演示在一个中心中引发 HubException 响应的.NET 客户端代码**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

有关中心管道模块的详细信息，请参阅[如何自定义中心管道](#hubpipeline)本主题中更高版本。

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>如何启用跟踪

若要启用服务器端跟踪，system.diagnostics 将元素添加到 Web.config 文件中，在此示例中所示：

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Visual Studio 中运行应用程序时，你可以查看中的日志**输出**窗口。

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>如何调用方法的客户端和管理从中心类外部的组

若要从你的中心类比另一个类中调用客户端方法，中心获取对 SignalR 上下文对象的引用，并使用的客户端上调用方法或管理组。

下面的示例`StockTicker`类获取上下文对象、 将其存储在类的实例，将类实例存储在静态属性，并使用从单一类实例的上下文调用`updateStockPrice`上的客户端的方法连接到名为集线器`StockTickerHub`。

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

如果你需要在长生存期对象中使用上下文多个时间，一次获取该引用，并保存它而不是每个时再次开始。 一次获取上下文可确保 SignalR 将消息发送到相同的序列顺序中心方法使客户端方法调用中的客户端。 有关演示如何使用中心 SignalR 上下文的教程，请参阅[服务器广播使用 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>调用客户端方法

你可以指定哪些客户端将接收 RPC，但是必须比当您调用从中心类较少的选项。 这样做的原因是，上下文不相关联的特定调用从客户端，因此任何方法，如需要知识的当前的连接 ID， `Clients.Others`，或`Clients.Caller`，或`Clients.OthersInGroup`，不可用。 可用选项如下：

- 所有连接的客户端。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- 特定客户端由连接 ID 标识。

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- 所有连接的客户端除外指定客户端，并由连接 ID 标识。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- 在指定的组的所有连接的客户端。

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- 所有连接的客户端在指定的组除指定客户端，由连接 ID 标识。

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

如果要调用到非中心类从方法中你的中心类，你可以在当前的连接 ID 传递并使用使用`Clients.Client`， `Clients.AllExcept`，或`Clients.Group`以模拟`Clients.Caller`， `Clients.Others`，或`Clients.OthersInGroup`。 在下面的示例中，`MoveShapeHub`类将传递到的连接 ID`Broadcaster`类，以便`Broadcaster`类可以模拟`Clients.Others`。

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>管理组成员资格

用于管理组中，与你在中心类具有相同的选项。

- 将客户端添加到组

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- 从组中删除客户端

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>如何自定义中心管道

SignalR 可将你自己的代码注入到中心管道。 下面的示例演示自定义中心管道模块记录来自客户端和客户端上调用的传出方法调用每个传入方法调用：

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

下面的代码中*Startup.cs*文件注册要在中心管线中运行的模块：

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

有许多不同的方法可以重写。 完整列表，请参阅[HubPipelineModule 方法](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)。
