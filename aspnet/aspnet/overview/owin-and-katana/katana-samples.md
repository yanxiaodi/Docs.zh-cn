---
uid: aspnet/overview/owin-and-katana/katana-samples
title: "Katana 示例 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="katana-samples"></a>Katana 示例
====================
by [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana 示例

**ASP.NET 路由示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
在某些应用程序要挂钩 OWIN 组件与非 OWIN 组件并行的 Asp.Net 路由表中。 此示例演示如何使用 MapOwinPath 和 MapOwinRoute 由 Microsoft.Owin.Host.SystemWeb RouteCollection 扩展方法。

**分支管道示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
OWIN 请求处理管道，不需要是线性的则可以对分支不同的方式处理请求。 此示例演示如何构造基于请求路径或其他如标头的请求数据的分支管道。 这些组件是 Microsoft.Owin.Mapping nuget 包中提供。

**自定义服务器示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
演示如何使用自定义的 OWIN 服务器，当自承载 OWIN。

**嵌入示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
可以在您自己的进程内运行某些 OWIN 服务器 (&quot;自承载&quot;)。 此示例演示如何开始使用 Microsoft.Owin.Hosting nuget 程序包提供的工具的 OWIN 应用程序。

**HelloWorld 示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN 是 HTTP 服务器 API 抽象，可以跨多个服务器的应用程序可移植性。 此示例演示如何编写 Hello World 应用程序使用某些**简单包装**围绕原始 OWIN 抽象和运行它的 web 服务器上喜欢 ASP.NET。

**Hello World 原始 OWIN 示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
此示例演示如何编写 Hello World 应用程序使用**原始**OWIN 抽象，如 Asp.Net web 服务器上运行。

**SignalR 示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
演示如何自承载 SignalR 使用 OWIN / Katana。 有关自承载 SignalR 的详细信息，请参阅[教程： 自承载的 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

**静态文件示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
演示如何以支持使用 OWIN 的静态文件的 HTTP 请求 / Katana。

**Web API** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
此示例演示如何承载在 IIS 中的 OWIN 并将 Web API 添加到 OWIN 管道。

**Web 套接字示例** | [源代码](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
演示如何通过使用在 OWIN 支持 Web 套接字[System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)类。
