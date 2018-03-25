---
uid: signalr/overview/getting-started/introduction-to-signalr
title: "SignalR 简介 |Microsoft 文档"
author: pfletcher
description: "本文介绍了 SignalR 是什么，以及一些了要创建的解决方案。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5bb49c9c2405d232ba5e067d99f8879b3bc99361
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
<a name="introduction-to-signalr"></a>SignalR 简介
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本文介绍了 SignalR 是什么，以及一些了要创建的解决方案。 
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)。


## <a name="what-is-signalr"></a>什么是 SignalR？

ASP.NET SignalR 是为 ASP.NET 开发人员库，它简化了将实时 web 功能添加到应用程序的过程。 实时 web 功能是能够具有服务器代码推送到连接的客户端立即内容变得可用，而不是让服务器等待客户端请求新数据。

SignalR 可用于将任何种类的"实时"web 功能添加到 ASP.NET 应用程序。 尽管聊天经常使用作为示例，可以完成得多。 用户的任何次刷新网页可看到新数据，或页面实现[长轮询](http://en.wikipedia.org/wiki/Push_technology#Long_polling)若要检索新数据，它是的候选使用 SignalR。 示例包括仪表板和监视应用程序，协作应用程序 （例如同时进行编辑的文档），作业的进度更新和实时窗体。

SignalR 也启用全新类型的 web 应用程序需要高频率更新在服务器上，例如，实时游戏。

SignalR 提供一个简单的 API，用于创建从服务器端.NET 代码的浏览器 （和其他客户端平台） 调用客户端中的 JavaScript 函数的服务器到客户端的远程过程调用 (RPC)。 SignalR 还包括用于连接管理 API （例如，连接和断开连接事件） 和分组连接。

![使用 SignalR 的调用方法](introduction-to-signalr/_static/image1.png)

SignalR 自动，处理的连接管理，可以在将消息广播到所有连接的客户端同时，如聊天室。 你还可以向特定客户端发送消息。 客户端和服务器之间的连接是持久性的与经典的 HTTP 连接，这是为每个通信重新建立不同。

SignalR 支持"服务器推送"功能，在其中服务器代码可以调用的客户端代码中使用的浏览器远程过程调用 (RPC)，而不是常见请求-响应模式在 web 上今天。

SignalR 应用程序可以向外扩展到数千个客户端使用 Service Bus、 SQL Server 或[Redis](http://redis.io)。

SignalR 是开放源代码，可通过访问[GitHub](https://github.com/signalr)。

## <a name="signalr-and-websocket"></a>SignalR 和 WebSocket

SignalR 可用的地方，使用新的 WebSocket 传输，并在必要时回退到较旧的传输。 虽然您肯定可以编写你直接使用 WebSocket，并使用 SignalR 意味着大量的额外功能，你将需要实现将已进行了为你的应用程序。 最重要的是，这意味着，你可以编写代码，你的应用程序以利用而无需担心创建一个单独的代码路径的较旧的客户端 WebSocket。 SignalR 还可保护你无需担心对 WebSocket，更新，因为 SignalR 将继续更新以支持在基础传输协议，更改各个 WebSocket 版本提供你的应用程序一致的接口。

虽然你肯定可以创建使用 WebSocket 单独的解决方案，SignalR 提供了所有需要编写你自己，如回退到其他传输和修改你的应用程序更新到 WebSocket 实现的功能。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>传输和回退

SignalR 是上某些要求进行客户端和服务器之间的实时工作的传输的抽象。 SignalR 连接作为 HTTP，启动，并在可用然后提升到的 WebSocket 连接。 因为它将建立最有效的服务器内存使用、 具有最低的延迟，并具有最基础功能 （如完整双工客户端和服务器之间的通信），但它还具有最严格，WebSocket 处于 SignalR 的理想传输要求： WebSocket 要求服务器使用 Windows Server 2012 或 Windows 8 和.NET Framework 4.5。 如果不满足这些要求，SignalR 将尝试使用其他传输以其连接。

### <a name="html-5-transports"></a>HTML 5 传输

这些传输依赖于支持[HTML 5](http://en.wikipedia.org/wiki/HTML5)。 如果客户端浏览器不支持 HTML 5 标准，将使用较旧的传输。

- **WebSocket** (如果服务器和浏览器指示其可以支持 Websocket)。 WebSocket 是建立客户端和服务器之间的 true 持久的双向连接的唯一传输。 但是，WebSocket 还具有最严格的要求;它仅在最新版本的 Microsoft Internet Explorer、 Google Chrome 和 Mozilla Firefox 受到完全支持，并在其他浏览器，如 Opera 和 Safari 中只有部分实现。
- **服务器发送事件**，也称为 EventSource （如果浏览器支持服务器发送事件，它本质上是除 Internet Explorer 的所有浏览器）。

### <a name="comet-transports"></a>Comet 传输

基于以下传输[Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web 应用程序模型，在浏览器或其他客户端维护一个长时间保留的 HTTP 请求，服务器可以用于将数据推送到客户端不使用客户端专门请求它。

- **不限次数帧**（适用于仅 Internet Explorer)。 不限次数帧创建隐藏的 IFrame 的未完成的服务器上的终结点向发出请求。 服务器然后不断脚本向发送客户端立即执行，从而提供从服务器到客户端的单向实时连接。 从客户端到服务器的连接使用单独的连接从服务器到客户端连接和每个需要发送的数据的标准的 HTTP 请求，如创建新的连接。
- **Ajax 长轮询**。 长轮询不会创建持续性连接，但改为轮询具有保持打开状态直到服务器响应，此时将关闭连接，并且立即请求新的连接的请求的服务器。 连接重置时，这会产生一定的延迟。

有关哪些传输受支持的配置下的详细信息，请参阅[支持的平台](supported-platforms.md)。

### <a name="transport-selection-process"></a>传输选择过程

以下列表显示 SignalR 使用来决定该传输使用的步骤。

1. 如果浏览器为 Internet Explorer 8 或更早版本，则使用长轮询。
2. 如果配置 JSONP (即，`jsonp`参数设置为`true`连接在启动时)，使用长轮询。
3. 如果操作正在进行的跨域连接，（即，如果该 SignalR 终结点不作为宿主的页位于同一域中），然后 WebSocket 将使用如果满足以下条件：

    - 客户端支持 CORS （跨域资源共享）。 有关在其的客户端支持 CORS 的详细信息，请参阅[在 caniuse.com CORS](http://www.caniuse.com/CORS)。
    - 客户端支持 WebSocket
    - 服务器支持 WebSocket

    如果不满足任何这些条件，则将使用长轮询。 跨域连接的详细信息，请参阅[如何建立的跨域连接](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)。
4. 如果未配置 JSONP 并且此连接不是跨域，如果客户端和服务器支持它，将使用 WebSocket。
5. 如果客户端或服务器不支持 WebSocket，如果可用，则使用服务器发送事件。
6. 如果服务器发送事件不可用，将尝试永久帧。
7. 如果永久帧失败，则使用长轮询。

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>监视传输

你可以确定你的应用程序使用通过启用日志记录在你的中心，并在浏览器中打开控制台窗口什么传输。

若要启用在浏览器的中心的事件的日志记录，请向客户端应用程序添加以下命令：

`$.connection.hub.logging = true;`

- 在 Internet Explorer 中，通过按 F12，打开开发人员工具，然后单击控制台选项卡。

    ![在 Microsoft Internet 资源管理器控制台](introduction-to-signalr/_static/image2.png)
- 在 Chrome 中，通过按 Ctrl + Shift + J 打开控制台。

    ![在 Google Chrome 的控制台](introduction-to-signalr/_static/image3.png)

控制台打开和启用日志记录，你将能够看到 SignalR 正在使用的传输。

![控制台在 Internet Explorer 中显示 WebSocket 传输](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>指定传输

协商传输需要一定的时间和客户端/服务器资源。 如果已知的客户端功能，则传输可以指定在启动客户端连接时。 下面的代码段演示如何启动使用 Ajax 长轮询传输中，因为如果它已知道，客户端不支持任何其他协议将使用的连接：

`connection.start({ transport: 'longPolling' });`

如果你想要尝试按顺序的特定传输的客户端，你可以指定回退的顺序。 下面的代码段演示试图 WebSocket 和失败、 直接转到长轮询。

`connection.start({ transport: ['webSockets','longPolling'] });`

用于指定传输的字符串常量定义，如下所示：

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>连接和中心

SignalR API 包含用于客户端和服务器之间进行通信的两个模型： 永久连接和中心。

连接表示发送单收件人、 分组，或广播消息的简单终结的点。 开发人员直接访问 SignalR 公开的低级别的通信协议 （以.NET 代码 PersistentConnection 类表示） 的持久性连接 API 提供。 使用连接通信模型将熟悉的开发人员可以使用基于连接的 Api，如 Windows Communication Foundation。

中心是基于连接 API，从而使你的客户端和服务器相互直接调用方法的更多高级管道。 SignalR 处理跨计算机界限调度像是通过幻数，从而允许客户端在服务器上调用方法，作为轻松地为本地方法，反之亦然。 使用中心通信模型将熟悉的开发人员可以使用远程调用 Api 如.NET 远程处理。 使用中心还使你可以将强类型的参数传递给方法，使模型绑定。

### <a name="architecture-diagram"></a>体系结构关系图

下图显示了中心、 永久连接和的传输使用的基础技术之间的关系。

![显示 Api、 传输和客户端的 SignalR 体系结构图示](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>中心的工作原理

当服务器端代码在客户端上调用方法时，跨包含的名称和要调用的方法的参数的活动传输发送一个数据包 （与方法参数发送一个对象，它使用序列化 JSON）。 客户端在客户端代码中定义的方法将方法名称，然后进行匹配。 如果没有匹配项，则将执行客户端方法使用的反序列化的参数数据。

可以使用类似的工具来监视的方法调用[Fiddler。](http://fiddler2.com/) 下图显示从 SignalR 服务器发送到 web 浏览器客户端在 Fiddler 日志窗格中的方法调用。 方法调用从调用的集线器发送`MoveShapeHub`，正在调用的方法称为`updateShape`。

![Fiddler 日志显示 SignalR 流量视图](introduction-to-signalr/_static/image6.png)

在此示例中，中心名称标识与`H`参数; 该方法名称识别具有以下`M`参数，并且发送到该方法的数据识别具有以下`A`参数。 生成此消息的应用程序创建在[高频率实时](tutorial-high-frequency-realtime-with-signalr.md)教程。

### <a name="choosing-a-communication-model"></a>选择的通信模型

大多数应用程序应使用的中心 API。 可在以下情况下使用连接 API:

- 需要指定将发送的实际消息的格式。
- 开发人员更愿意使用一种消息传递和调度模式，而不是远程调用模型。
- 现有应用程序使用消息传送模型是在移植后使用 SignalR。
