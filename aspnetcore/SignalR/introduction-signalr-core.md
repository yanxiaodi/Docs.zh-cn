---
title: "在 ASP.NET Core 上的 SignalR 简介"
author: rachelappel
description: "了解如何 ASP.NET 核心 SignalR 库简化了将实时 web 功能添加到应用。"
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction-signalr-core
ms.openlocfilehash: 3fa70c957b246787d4e457c74f90ad797b3af766
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-signalr"></a>SignalR 简介

作者：[Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>什么是 SignalR？

ASP.NET 核心 SignalR 是一个库，简化了添加到应用的实时 web 功能。 实时 web 功能可立即使服务器端代码能够将内容推送到客户端。

适用于 SignalR 的良好候选：

* 需要从服务器的高频率更新的应用程序。 示例包括游戏、 社交网络、 投票、 拍卖、 映射和 GPS 应用。
* 仪表板和监视应用程序。 示例包括公司的仪表板，即时销售更新，或经常出差警报。
* 协作应用程序。 白板应用和团队会议软件是协作应用程序的示例。
* 需要通知的应用程序。 社交网络、 电子邮件、 聊天、 游戏、 旅行警报和很多其他应用程序使用通知。

SignalR 提供了一个 API，用于创建服务器到客户端[远程过程调用 (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)。 Rpc 在客户端上从服务器端.NET 核心代码中调用 JavaScript 函数。

有关 ASP.NET 核心 SignalR:

* 自动处理的连接管理。
* 允许同时广播到所有连接的客户端的消息。 例如，聊天室。
* 允许将消息发送到特定客户端或客户端组。
* 为在开放源代码[GitHub](https://github.com/aspnet/signalr)。
* 可缩放。

客户端和服务器之间的连接是持久性的与 HTTP 连接不同。

## <a name="transports"></a>传输

SignalR 通过多种方法用于构建实时 web 应用程序的摘要。 [Websocket](https://tools.ietf.org/html/rfc7118)是最佳的传输，但那些不可用时，可以使用其他技术 Server-Sent 事件和长轮询。 SignalR 将自动检测并初始化了合适的传输，基于在服务器和客户端上受支持的功能。

## <a name="hubs-and-endpoints"></a>中心和终结点

SignalR 使用中心和终结点，客户端和服务器之间进行通信。 中心 API 介绍的大多数方案。

基于终结点 API，从而使你的客户端和服务器相互调用方法的高级管道，则集线器。 SignalR 处理调度跨计算机界限自动，允许客户端在服务器上调用方法，作为轻松地为本地方法，反之亦然。 中心允许将强类型参数传递给方法，从而使模型绑定。 SignalR 提供两个内置的中心协议： 文本协议基于 JSON 和基于二进制协议[MessagePack](https://msgpack.org/)。  MessagePack 通常会产生较小比时使用 JSON 消息。 较旧的浏览器必须支持[XHR 级别 2](https://caniuse.com/#feat=xhr2)以提供 MessagePack 协议支持。

中心调用通过使用活动传输发送消息的客户端代码。 消息包含的名称和客户端方法的参数。 为方法参数发送的对象是使用已配置的协议反序列化。 客户端尝试匹配到客户端代码中的方法的名称。 如果找到匹配项，使用反序列化的参数数据运行客户端方法。

终结点提供原始类似套接字的 API，使其能够读取和写入来自客户端。 它是最多向开发人员处理分组、 广播和其他功能。 中心 API 基于终结点层。

下图显示了中心、 终结点，和客户端之间的关系。

![SignalR 映射](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a>相关资源

[开始使用 SignalR 为 ASP.NET Core](xref:signalr/get-started-signalr-core)
