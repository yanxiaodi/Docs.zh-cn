---
uid: signalr/overview/performance/scaleout-in-signalr
title: "简介中 SignalR 的向外缩放 |Microsoft 文档"
author: MikeWasson
description: "Visual Studio 2013.NET 4.5 SignalR 本主题中使用软件版本的早期版本的信息的本主题的版本 2 早期版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-scaleout-in-signalr"></a>简介中 SignalR 的向外缩放
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

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


一般情况下，有两种方法来扩展 web 应用程序：*向上扩展*和*向外扩展*。

- 使用更多 RAM、 Cpu、 等使用更大的服务器 （或较大的 VM） 的方式向上扩展。
- 扩展意味着添加更多服务器以处理负载。

向上扩展的问题是你快速命中了机的大小限制。 除此之外，你需要向外扩展。但是，当你向外扩展，客户端可以获取路由到不同的服务器。 连接到一台服务器的客户端不会收到从另一台服务器发送的消息。

![](scaleout-in-signalr/_static/image1.png)

一种解决方案是将使用名为的组件，在服务器之间的消息转发*底板*。 启用底板，每个应用程序实例将消息发送到底板，与底板将其转发给其他应用程序实例。 （在电子行业，底板是并行的连接器的组。 打比方，SignalR 基架连接多个服务器。）

![](scaleout-in-signalr/_static/image2.png)

SignalR 目前提供三个背板：

- **Azure Service Bus**。 Service Bus 是允许组件按松散耦合的方式发送消息的消息传递基础结构。
- **Redis**。 Redis 是一个内存中键 / 值存储。 Redis 支持发布/订阅 ("pub/sub") 模式，用于将消息发送。
- **SQL Server**。 SQL Server 底板将消息写入 SQL 表。 底板为有效的消息传递使用 Service Broker。 但是，它还适用如果未启用 Service Broker。

如果你部署 Azure 上的应用程序，请考虑使用 Redis 底板使用[Azure Redis 缓存](https://azure.microsoft.com/services/cache/)。 如果将部署到你自己的服务器场，请考虑 SQL Server 或 Redis 背板。

以下主题包含针对每个底板的分步教程：

- [使用 Azure 服务总线的 SignalR 横向扩展](scaleout-with-windows-azure-service-bus.md)
- [使用 Redis 的 SignalR 横向扩展](scaleout-with-redis.md)
- [使用 SQL Server 的 SignalR 横向扩展](scaleout-with-sql-server.md)

## <a name="implementation"></a>实现

在 SignalR 通过消息总线发送每条消息。 消息总线实现[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)接口，从而提供发布/订阅抽象。 通过替换默认的工作的底板**IMessageBus**含总线为该底板设计的。 例如，Redis 消息总线是[RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)，并使用 Redis [pub/sub](http://redis.io/topics/pubsub)机制来发送和接收消息。

每个服务器实例连接到通过总线底板。 发送一条消息，它将转到面板，并底板将其发送到每个服务器。 当服务器从底板获取一条消息时，它会在其本地缓存中将该消息。 服务器然后将从其本地缓存消息传送到客户端。

对于每个客户端连接，在读取消息流的客户端的进度被跟踪使用游标。 （游标表示消息流中的位置。）如果客户端断开连接并重新连接，但它会将总线要求提供任何客户端的光标在值后到达的消息。 连接使用时，会发生情况同样[长轮询](../getting-started/introduction-to-signalr.md#transports)。 长时轮询请求完成后，客户端将打开一个新连接，并请求到达光标位置后的消息。

光标机制的工作方式即使客户端路由到不同的服务器上重新连接。 底板已意识到所有服务器，并且它并不重要的客户端连接到哪一台服务器。

## <a name="limitations"></a>限制

使用基架，则是低于客户端直接与单个服务器节点通信时的最大消息吞吐量。 这是因为底板将转发到每个节点，每条消息，因此底板可能成为瓶颈。 这一限制是否是问题取决于应用程序。 例如，下面是一些典型的 SignalR 方案：

- [服务器广播](../getting-started/tutorial-server-broadcast-with-signalr.md)（例如，股票行情）： 背板很适合此方案中，因为该服务器控制速率发送消息的速率。
- [客户端到客户端](../getting-started/tutorial-getting-started-with-signalr.md)（如聊天）： 在此方案中，底板可能成为瓶颈如果的消息数会随着客户端的数量; 也就是说，如果消息的速率增长按比例随着越来越多的客户端将加入。
- [高频率实时](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)（例如，实时游戏）： 底板不建议用于此方案。

## <a name="enabling-tracing-for-signalr-scaleout"></a>启用跟踪的 SignalR 扩展

若要启用跟踪背板，请将以下各节添加到 web.config 文件中，根下**配置**元素：

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
