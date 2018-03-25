---
title: "ASP.NET Core 中的 WebSocket 支持"
author: tdykstra
description: "了解如何在 ASP.NET Core 中开始使用 WebSocket。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>ASP.NET Core 中 WebSocket 的介绍

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Andrew Stanton-Nurse](https://github.com/anurse)

本文介绍 ASP.NET Core 中 WebSocket 的入门方法。 [WebSocket](https://wikipedia.org/wiki/WebSocket) 是一个协议，支持通过 TCP 连接建立持久的双向信道。 它可用于聊天、股票报价和游戏等应用程序，以及 Web 应用程序中需要实时功能的任何情景。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）。 有关详细信息，请参阅[后续步骤](#next-steps)。


## <a name="prerequisites"></a>系统必备

* ASP.NET Core 1.1（无法在 1.0 上运行）
* ASP.NET Core 运行的任何操作系统：
  
  * Windows 7 / Windows Server 2008 及更高版本
  * Linux
  * macOS

* **例外**：如果在带有 IIS 或 WebListener 的 Windows 上运行应用，必须使用：

  * Windows 8 / Windows Server 2012 及更高版本
  * IIS 8 / IIS 8 Express
  * 必须在 IIS 中启用 WebSocket

* 有关受支持的浏览器，请参阅 http://caniuse.com/#feat=websockets。

## <a name="when-to-use-it"></a>何时使用

需要直接使用套接字连接时，请使用 WebSocket。 例如，实时游戏可能需要最佳性能。

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) 为实时功能提供了更丰富的应用程序模型，但它仅在 ASP.NET 上运行，不能在 ASP.NET Core 上运行。 SignalR 的 Core 版本正在开发中；若要了解其进度，请参阅 [适用于 SignalR Core 的 GitHub 存储库](https://github.com/aspnet/SignalR)。

如果不想等待 SignalR Core，现在可直接使用 WebSocket。 但是可能必须开发 SignalR 提供的功能，例如：

* 通过自动回退到替代传输方法来支持更广泛的浏览器版本。
* 连接断开时自动重新连接。
* 支持服务器上的客户端调用方法，反之亦然。
* 支持缩放到多个服务器。

## <a name="how-to-use-it"></a>使用方法

* 安装 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 包。
* 配置中间件。
* 接受 WebSocket 请求。
* 发送和接收消息。

### <a name="configure-the-middleware"></a>配置中间件

在 `Startup` 类的 `Configure` 方法中添加 WebSocket 中间件。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

可配置以下设置：

* `KeepAliveInterval` - 向客户端发送“ping”帧的频率，以确保代理保持连接处于打开状态。
* `ReceiveBufferSize` - 用于接收数据的缓冲区的大小。 只有高级用户才需要对其进行更改，以便根据数据大小调整性能。

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>接受 WebSocket 请求

在请求生命周期后期（例如在 `Configure` 方法或 MVC 操作的后期），检查它是否是 WebSocket 请求并接受 WebSocket 请求。

该示例来自 `Configure` 方法的后期。

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket 请求可以来自任何 URL，但此示例代码只接受 `/ws` 的请求。

### <a name="send-and-receive-messages"></a>发送和接收消息

`AcceptWebSocketAsync` 方法将 TCP 连接升级到 WebSocket 连接，并提供 [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) 对象。 使用 WebSocket 对象发送和接收消息。

之前显示的接受 WebSocket 请求的代码将 `WebSocket` 对象传递给 `Echo` 方法；此处为 `Echo` 方法。 代码接收消息并立即发回相同的消息。 一直在循环中执行此操作，直到客户端关闭连接。 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

如果在开始此循环之前接受 WebSocket，中间件管道会结束。  关闭套接字后，管道展开。 也就是说，如果接受 WebSocket ，请求会在管道中停止前进，就像点击 MVC 操作一样。  但是完成此循环并关闭套接字时，请求将在管道中后退。

## <a name="next-steps"></a>后续步骤

本文附带的[示例应用程序](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)是一个简单的回显应用程序。 它有一个可建立 WebSocket 连接的网页，且服务器仅将其收到的消息重新发回到客户端。 从命令提示符运行它（它未设置为从具有 IIS Express 的 Visual Studio 运行）并导航到 http://localhost:5000。 该网页的左上方显示连接状态：

![网页的初始状态](websockets/_static/start.png)

选择“连接”，向显示的 URL 发送 WebSocket 请求。  输入测试消息并选择“发送”。 完成后，请选择“关闭套接字”。 “通信日志”部分会报告每一个发生的“打开”、“发送”和“关闭”操作。

![网页的初始状态](websockets/_static/end.png)
