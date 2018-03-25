---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: "了解和处理在 SignalR 连接生存期事件 |Microsoft 文档"
author: pfletcher
description: "本文介绍如何使用事件中心 API 公开的。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fd9cafd8d7706807998793c3c39377fe9604266
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>了解和处理在 SignalR 连接生存期事件
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本文概述了你可以处理的 SignalR 连接、 重新连接和断开连接事件以及你可以配置的超时和 keepalive 设置。
> 
> 本文假设你已有的 SignalR 和连接生存期事件一些知识。 SignalR 的简介，请参阅[简介 SignalR](../getting-started/introduction-to-signalr.md)。 有关连接生存期事件的列表，请参阅以下资源：
> 
> - [如何连接生存期中处理事件的中心类](hubs-api-guide-server.md#connectionlifetime)
> - [如何处理 JavaScript 客户端在连接生存期事件](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [如何处理.NET 客户端在连接生存期事件](hubs-api-guide-net-client.md#connectionlifetime)
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

本文包含以下各节：

- [连接生存期术语和方案](#terminology)

    - [SignalR 连接、 传输连接和物理连接](#signalrvstransport)
    - [传输断开连接情况](#transportdisconnect)
    - [客户端断开连接方案](#clientdisconnect)
    - [服务器断开连接方案](#serverdisconnect)
- [超时和 keepalive 设置](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [如何更改超时和 keepalive 设置](#changetimeout)
- [如何通知用户断开连接](#notifydisconnect)
- [如何重新持续连接](#continuousreconnect)
- [如何断开连接的服务器代码中的客户端](#disconnectclientfromserver)
- [检测断开连接的原因](#detectingreasonfordisconnection)

API 参考主题的链接将指向 API 的.NET 4.5 版本。 如果你使用.NET 4，请参阅[API 主题的.NET 4 版本](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)。

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>连接生存期术语和方案

`OnReconnected` SignalR Hub 中的事件处理程序可以执行后直接`OnConnected`但不是后`OnDisconnected`给定客户端。 你可以重新连接，而无需断开连接的原因是，有几种方法在 SignalR 使用 word"连接"。

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR 连接、 传输连接和物理连接

本文将区分*SignalR 连接*，*传输连接*，和*物理连接*:

- **SignalR 连接**指的是客户端和服务器 URL，由 SignalR API 维护并由连接 id。 唯一标识之间的逻辑关系 有关此关系的数据由 SignalR 维护和用于建立传输连接。 数据在客户端调用时释放的关系端和 SignalR`Stop`当 SignalR 尝试重新建立的丢失的传输连接时达到方法或超时限制。
- **传输连接**指的是客户端和维护四个传输 Api 之一的服务器之间的逻辑关系： Websocket，服务器将发送事件，下去帧，或者长轮询。 SignalR 使用传输 API 创建的传输连接，并传输 API 取决于创建传输连接的物理网络连接存在。 传输连接结束时 SignalR 将终止该进程，或当传输 API 检测到物理连接已断开。
- **物理连接**引用物理网络链接--电线，无线信号，路由器，等等 — 促进客户端计算机和服务器计算机之间的通信。 物理连接必须存在才能建立传输连接，并且必须以建立 SignalR 连接建立的传输连接。 但是，重大的物理连接不会始终立即结束传输连接或 SignalR 连接，如将在本主题后面所述。

下图中，在 SignalR 连接由的中心 API 和 PersistentConnection API SignalR 层、 传输连接由传输层，和由服务器之间的行表示物理连接和的客户端。

![SignalR 体系结构关系图](handling-connection-lifetime-events/_static/image1.png)

当调用`Start`方法 SignalR 客户端中的，为 SignalR 客户端代码提供建立物理连接到服务器所需的所有信息。 SignalR 客户端代码使用此信息来发出 HTTP 请求和建立物理连接使用四个传输方法之一。 如果传输连接失败或服务器出现故障，SignalR 连接不会立即消失由于客户端仍需要自动重新建立新的传输连接到相同的 SignalR URL 的信息。 在此方案中，涉及到从用户应用程序无需干预，并当 SignalR 客户端代码创建一个新的传输连接，它不会启动新的 SignalR 连接。 连续性的 SignalR 连接反映事实中，在调用时创建的连接 ID`Start`方法，不会更改。

`OnReconnected`集线器上的事件处理程序执行的传输连接已丢失后未自动重新建立时。 `OnDisconnected`在 SignalR 连接的一端执行事件处理程序。 SignalR 连接可以结束任何以下方法：

- 如果客户端调用`Stop`方法，一条停止消息发送到服务器，并且客户端和服务器会立即结束 SignalR 连接。
- 客户端和服务器之间的连接丢失后，客户端尝试重新连接和服务器等待客户端重新连接。 如果尝试重新连接失败并且断开连接超时期限结束时，客户端和服务器结束 SignalR 连接。 客户端停止尝试重新连接，并在服务器释放 SignalR 连接其表示形式。
- 如果客户端停止运行无有机会调用`Stop`方法，为客户端重新连接，请等待的服务器，然后结束后断开连接超时期限 SignalR 连接。
- 如果服务器停止运行，客户端尝试重新连接 （重新创建传输连接），，然后结束后断开连接超时期限 SignalR 连接。

当不存在连接问题，并在用户应用程序通过调用结束 SignalR 连接`Stop`方法、 SignalR 连接并传输连接开始和在大约在同一时间结束。 下列各节更详细地说明了其他方案。

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>传输断开连接情况

物理连接可能较慢，或者可能有连接中断。 根据中断的长度等因素，可能会丢弃传输连接。 SignalR 然后尝试重新建立传输连接。 有时传输连接 API 检测中断并删除传输连接中，和 SignalR 找出立即的连接已丢失。 在其他情况下，既不传输连接 API，也不 SignalR 感知将立即变为连接已丢失。 对于除长轮询的所有传输，SignalR 客户端使用调用的函数*keepalive*检查传输 API 是无法检测到的连接丢失。 长时间轮询连接有关的信息，请参阅[超时和 keepalive 设置](#timeoutkeepalive)本主题中更高版本。

当连接处于非活动状态时，定期服务器会将 keepalive 数据包发送到客户端。 截至日期在撰写本文时，默认频率为每隔 10 秒。 通过侦听这些数据包，客户端可以指示是否没有连接问题。 如果在有要求时，未收到 keepalive 数据包，一段时间之后客户端假定没有连接问题，如变慢，或者中断。 如果在较长的时间后仍未收到 keepalive，客户端将假定已断开连接，并且它将开始尝试重新连接。

下图说明了物理连接的出现问题，未立即识别的传输 API 时在典型方案中引发的客户端和服务器事件。 关系图适用于下列情况：

- 传输是 Websocket、 永久框架或服务器发送事件。
- 有几个不同时期的物理网络连接中断。
- 传输 API 不会成为感知的中断，因此 SignalR 依赖于 keepalive 功能以进行检测。

![传输断开连接](handling-connection-lifetime-events/_static/image2.png)

如果客户端将进入重新连接模式，但无法建立传输连接在断开连接超时限制内的，服务器将终止 SignalR 连接。 在这种情况的服务器执行的中心`OnDisconnected`方法和客户端管理连接更高版本的情况下将发送到客户端断开连接消息的队列。 如果客户端然后未重新连接，它将接收的断开连接命令和调用`Stop`方法。 在此方案中，`OnReconnected`时客户端重新连接，不执行和`OnDisconnected`客户端调用时不执行`Stop`。 下图说明了这种情况。

![传输中断的服务器超时](handling-connection-lifetime-events/_static/image3.png)

可以在客户端引发的 SignalR 连接生存期事件如下所示：

- `ConnectionSlow`客户端事件。

    Keepalive 超时期限预设的比例过最后一条消息，或收到 keepalive ping 时引发。 默认 keepalive 超时警告时间为 2/3 的 keepalive 超时。 Keepalive 超时为 20 秒，因此会在大约 13 秒出现的警告。

    默认情况下，服务器发送 keepalive ping 每隔 10 秒，并且客户端检查有关每 2 秒 （三分之一的 keepalive 超时值和 keepalive 超时警告值之间的差异） 的 keepalive ping。

    如果传输 API 变得感知的断开连接，则在 keepalive 超时警告期传递之前，断开连接的可能通知 SignalR。 在这种情况下，`ConnectionSlow`将不会引发事件，并 SignalR 将直接转到`Reconnecting`事件。
- `Reconnecting`客户端事件。

    （a） 传输 API 检测的连接已丢失，或 （b） 的 keepalive 超时期限过最后一条消息，或收到 keepalive ping 时引发。 SignalR 客户端代码首先尝试重新连接。 如果您的应用程序采取某种操作，在传输连接丢失时，你可以处理此事件。 默认 keepalive 超时期限当前为 20 秒。

    如果客户端代码尝试调用 Hub 方法，在重新连接模式 SignalR 时，SignalR 将尝试发送命令。 大多数情况下，此类尝试都将失败，但在某些情况下它们可能会成功。 对于服务器发送的事件、 永久框架分隔符和长时间轮询传输，SignalR 使用两条通信通道，一个客户端使用发送消息，它用来接收消息的另一个。 用于接收的通道是永久打开其中一个，和物理连接被中断时关闭的一个。 通道用于发送保持可用性，因此，如果物理连接恢复后，重新建立接收通道之前，从客户端到服务器的方法调用可能会成功。 指定的注册表文件才 SignalR 重新打开用于接收通道将接收消息的返回值。
- `Reconnected`客户端事件。

    重新建立传输连接时引发。 `OnReconnected`中心中的事件处理程序执行。
- `Closed`客户端事件 (`disconnected`在 JavaScript 中的事件)。

    在断开连接超时期限过期时 SignalR 客户端代码尝试在传输连接断开后重新连接时引发。 默认值断开连接超时为 30 秒。 (由于连接终止时，也会引发此事件`Stop`调用方法。)

传输连接中断，由传输 API 未检测到不延迟的服务器的时间比 keepalive 超时警告期限长 keepalive ping 接收可能不会导致任何连接生存期要引发的事件。

某些网络环境有意关闭空闲连接，并保持连接数据包的另一个功能是帮助防止这告知这些网络 SignalR 连接正在使用的。 在极端情况下的默认频率进行 keepalive ping 不可能不足以防止关闭的连接。 在这种情况下，你可以配置 keepalive ping 更频繁发送。 有关详细信息，请参阅[超时和 keepalive 设置](#timeoutkeepalive)本主题中更高版本。

> [!NOTE] 
> 
> **重要**： 此处所述的事件的顺序不能保证。 SignalR 使每次尝试在此方案中，根据不可预测地方式引发连接生存期事件，但有许多变体网络事件和基础通信框架，例如传输 Api 处理它们的多种方法。 例如，`Reconnected`可能引发事件，当客户端重新连接时，或`OnConnected`尝试建立的连接不成功时，可能运行在服务器上的处理程序。 本主题描述仅通常进行某些典型的情况下将产生的影响。


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>客户端断开连接方案

在浏览器客户端，维护 SignalR 连接的 SignalR 客户端代码的网页的 JavaScript 上下文中运行。 具有原因 SignalR 连接已结束时从一个导航到另一页和页该的原因你有多个连接与多个连接 Id 如果连接从多个浏览器窗口或选项卡。 当用户关闭浏览器窗口或选项卡上，或导航到新页或刷新页面时，SignalR 连接立即结束因为 SignalR 客户端代码和句柄该浏览器事件你调用`Stop`方法。 在这些情况下，或当你的应用程序调用的任何客户端平台`Stop`方法，`OnDisconnected`在服务器上立即执行的事件处理程序和客户端引发`Closed`事件 (事件名为`disconnected`中JavaScript)。

如果客户端应用程序或计算机上运行崩溃或进入睡眠状态 （例如，当用户关闭便携式计算机），服务器不会知道有关发生了什么情况。 就服务器知道，客户端丢失可能会导致连接中断和客户端可能会尝试重新连接。 因此，在这些情况下，服务器在等待，让客户端重新连接时，有机会和`OnDisconnected`断开连接超时期限过期 （大约 30 秒在默认情况下），才会执行。 下图说明了这种情况。

![客户端计算机失败](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>服务器断开连接方案

当服务器处于脱机状态-它重新启动，则将失败，应用程序域回收、 等-结果可能是类似于丢失连接，或传输 API 和 SignalR 可能会立即明白，服务器将消失，并且 SignalR 可能开始尝试重新连接且不引发`ConnectionSlow`事件。 如果客户端将进入重新连接模式，并且服务器将恢复或重新启动或新的服务器联机的断开连接超时期限过期之前，客户端将重新连接到还原的或新服务器。 在这种情况下，客户端上仍然存在 SignalR 连接和`Reconnected`引发事件。 在第一个服务器上，`OnDisconnected`绝不会执行，并在新服务器上，`OnReconnected`尽管执行`OnConnected`永远不会为此客户端之前该服务器上执行。 （结果相同，则客户端重新连接到同一台服务器后重新启动或应用程序域回收，因为当在服务器重启它有以前的连接活动的任何内存。）下图假定该传输 API 将变为感知断开连接的立即，因此`ConnectionSlow`不会引发事件。

![服务器故障和重新连接](handling-connection-lifetime-events/_static/image5.png)

如果服务器不会成为可用断开连接的超时期限内，SignalR 连接结束。 在此方案中，`Closed`事件 (`disconnected` JavaScript 客户端中) 将在客户端上引发但`OnDisconnected`在于： 绝不调用在服务器上。 下图假定，传输 API 不会成为感知的断开连接，以便检测到的 SignalR keepalive 功能和`ConnectionSlow`引发事件。

![服务器故障和超时](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>超时和 keepalive 设置

默认值`ConnectionTimeout`， `DisconnectTimeout`，和`KeepAlive`值适用于大多数方案，但如果你的环境有特殊需求，可以更改。 例如，如果你的网络环境将关闭 5 秒均处于空闲状态的连接，你可能需要减小 keepalive 值。

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

此设置表示要保留的传输连接，打开和等待关闭它并打开一个新连接之前响应的时间量。 默认值为 110 秒。

此设置适用仅当 keepalive 功能被禁用时，这通常仅适用于长时间轮询传输。 下图说明了此设置在 long 类型的值的效果轮询传输连接。

![长时间轮询传输连接](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

此设置表示之后的传输连接丢失在引发之前要等待的时间量`Disconnected`事件。 默认值为 30 秒。 当你将设置`DisconnectTimeout`，`KeepAlive`自动设置为 1/3`DisconnectTimeout`值。

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

此设置表示发送 keepalive 数据包通过空闲连接前等待的时间量。 默认值为 10 秒。 此值不能超过 1/3 的`DisconnectTimeout`值。

如果你想要同时设置`DisconnectTimeout`和`KeepAlive`，将其设置`KeepAlive`后`DisconnectTimeout`。 否则为你`KeepAlive`设置将被覆盖时`DisconnectTimeout`自动设置`KeepAlive`为 1/3 的超时值。

如果你想要禁用 keepalive 功能，设置`KeepAlive`为 null。 对于长时间会自动禁用 Keepalive 功能轮询传输。

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>如何更改超时和 keepalive 设置

若要更改这些设置的默认值，请将它们设置`Application_Start`中你*Global.asax*文件，如下面的示例中所示。 示例代码所示的值是一样的默认值。

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>如何通知用户断开连接

在某些应用程序可能想要有连接问题时向用户显示一条消息。 你有几个选项如何和何时执行此操作。 以下代码示例适用于 JavaScript 客户端使用生成的代理。

- 处理`connectionSlow`事件 SignalR 是知道连接问题之前它将进入重新连接模式, 时，就会立即显示一条消息。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- 处理`reconnecting`事件时 SignalR 感知断开连接并进入重新连接模式显示一条消息。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- 处理`disconnected`事件显示一条消息，当尝试重新连接已超时。在此方案中，重新建立与服务器的连接再次的唯一方法是重新 SignalR 连接启动通过调用`Start`方法，将创建一个新的连接 id。 下面的代码示例使用一个标志来确保发出到通过调用导致 SignalR 连接正常结束后不重新连接超时之后才, 通知`Stop`方法。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>如何重新持续连接

某些应用程序可能想要在已丢失，并尝试重新连接已超时后自动重新建立连接。若要做到这一点，你可以调用`Start`方法从你`Closed`事件处理程序 (`disconnected` JavaScript 客户端上的事件处理程序)。 你可能需要等待一段时间之前调用`Start`以避免执行此操作太频繁服务器或物理连接时不可用时。 下面的代码示例适用于 JavaScript 客户端使用生成的代理。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

需要注意的移动客户端中的潜在问题是连续的重新连接尝试的服务器或物理连接不可用时可能会导致不必要的电池消耗。

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>如何断开连接的服务器代码中的客户端

SignalR 版本 2 不具有用于客户端断开连接的内置服务器 API。 有[计划在将来添加此功能](https://github.com/SignalR/SignalR/issues/2101)。 在当前的 SignalR 版本中，从服务器中断开客户端的最简单方法是在客户端上实现断开连接方法，从服务器中调用该方法。 下面的代码示例显示使用生成的代理的 JavaScript 客户端断开连接方法。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> 安全-既不此方法用于客户端断开连接，也不建议内置 API 将会解决运行恶意代码，因为客户端无法重新连接或受到攻击的代码可能会删除的黑客攻击客户端的方案`stopClient`方法或更改它能做什么。 为实现有状态的拒绝服务 (DOS) 保护的适当位置位于不在框架或服务器层中，但而是在前端的基础结构。


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>检测断开连接的原因

SignalR 2.1 向服务器添加重载`OnDisconnect`事件，该值指示是否客户端有意断开连接，而不超时。`StopCalled`参数为 true，如果客户端显式关闭了连接。 在 JavaScript 中，如果服务器错误导致客户端断开连接、 错误信息将传递给客户端作为`$.connection.hub.lastError`。

**C# 服务器代码：`stopCalled`参数**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**JavaScript 客户端代码： 访问`lastError`中`disconnect`事件。**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
