---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "启用 SignalR 跟踪 |Microsoft 文档"
author: tfitzmac
description: "本文档介绍如何启用和配置对 SignalR 服务器和客户端的跟踪。 跟踪可用于查看有关事件的诊断信息..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="enabling-signalr-tracing"></a>启用 SignalR 跟踪
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文档介绍如何启用和配置对 SignalR 服务器和客户端的跟踪。 跟踪可用于 SignalR 应用程序中查看有关事件的诊断信息。
> 
> 本主题最初由 Patrick Fletcher 编写。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR 版本 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


启用跟踪后，SignalR 应用程序创建事件日志的条目。 你可以从客户端和服务器登录事件。 跟踪对服务器日志连接、 向外扩展提供程序和消息总线事件。 跟踪客户端日志连接事件。 SignalR 2.1 及更高版本，客户端上的跟踪记录中心调用消息的全部内容。

## <a name="contents"></a>内容

- [在服务器上启用跟踪](#server)

    - [服务器事件记录到文本文件](#server_text)
    - [服务器事件记录到事件日志](#server_eventlog)
- [在.NET 客户端 （仅限 Windows 桌面应用） 中启用跟踪](#net_client)

    - [桌面客户端事件记录到控制台](#desktop_console)
    - [桌面客户端事件记录到文本文件](#desktop_text)
- [在 Windows Phone 8 客户端中启用跟踪](#phone)

    - [Windows Phone 客户端事件记录到 UI](#phone_ui)
    - [Windows Phone 客户端事件记录到调试控制台](#phone_debug)
- [在 JavaScript 客户端中启用跟踪](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>在服务器上启用跟踪

启用应用程序的配置文件 （App.config 或 Web.config，具体取决于项目的类型。） 中的服务器的跟踪你指定要记录的事件的类别。 在配置文件中，你还指定是否事件记录到文本文件、 Windows 事件日志中或使用的实现一个自定义日志[TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx)。

服务器事件类别包括以下类型的消息：

| 源 | 消息 |
| --- | --- |
| SignalR.SqlMessageBus | SQL 消息总线向外扩展提供程序安装程序、 数据库操作、 错误和超时事件 |
| SignalR.ServiceBusMessageBus | 服务总线向外扩展提供程序主题创建和订阅、 错误和消息事件 |
| SignalR.RedisMessageBus | Redis 向外扩展提供程序连接、 断开连接和错误事件 |
| SignalR.ScaleoutMessageBus | 向外扩展消息事件 |
| SignalR.Transports.WebSocketTransport | WebSocket 传输连接、 断开连接、 消息和错误事件 |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents 传输连接、 断开连接、 消息和错误事件 |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame 传输连接、 断开连接、 消息和错误事件 |
| SignalR.Transports.LongPollingTransport | LongPolling 传输连接、 断开连接、 消息和错误事件 |
| SignalR.Transports.TransportHeartBeat | 传输连接、 断开连接和 keepalive 事件 |
| SignalR.ReflectedHubDescriptorProvider | 中心发现事件 |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>服务器事件记录到文本文件

下面的代码演示如何启用跟踪每个类别的事件。 此示例将配置应用程序事件记录到文本文件。

**启用跟踪的 XML 服务器代码**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

在上面的代码，`SignalRSwitch`条目指定[TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx)用于发送到指定的日志的事件。 在这种情况下，设置为`Verbose`这意味着所有的调试和跟踪消息记录。

下面的输出显示的条目`transports.log.txt`使用上面的配置文件的应用程序的文件。 显示一个新连接、 已删除的连接和传输检测信号事件。

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>服务器事件记录到事件日志

若要事件记录到事件日志，而不是一个文本文件，将更改中的条目的值`sharedListeners`节点。 下面的代码演示如何服务器事件记录到事件日志：

**XML 用于日志记录事件写入事件日志的服务器代码**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

事件记录在应用程序日志中，并且可通过事件查看器中，如下所示：

![显示 SignalR 日志的事件查看器](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> 在使用事件日志时，设置**TraceLevel**到**错误**需要可管理的消息数。

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>在.NET 客户端 （仅限 Windows 桌面应用） 中启用跟踪

.NET 客户端控制台中，一个文本文件，或使用的实现自定义日志可以记录事件[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)。

若要启用日志记录在.NET 客户端中，设置连接的`TraceLevel`属性[TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx)值，与`TraceWriter`到有效的属性[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)实例。

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>桌面客户端事件记录到控制台

下面的 C# 代码演示如何在.NET 客户端到控制台中记录事件：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>桌面客户端事件记录到文本文件

下面的 C# 代码演示如何在.NET 客户端到文本文件中记录事件：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

下面的输出显示的条目`ClientLog.txt`使用上面的配置文件的应用程序的文件。 它显示客户端将连接到服务器，并调用客户端方法的中心调用`addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>在 Windows Phone 8 客户端中启用跟踪

对于 Windows Phone 应用程序的 SignalR 应用程序使用相同的.NET 客户端作为桌面应用，但[Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx)和写入的文件[StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx)不可用。 相反，你需要创建的自定义实现[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)跟踪。 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Windows Phone 客户端事件记录到 UI

[SignalR 基本代码](https://github.com/SignalR/SignalR/archive/master.zip)包括将跟踪输出写入一个 Windows Phone 示例[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)使用自定义[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx)实现调用`TextBlockWriter`。 此类可在**samples/Microsoft.AspNet.SignalR.Client.WP8.Samples**项目。 创建的实例时`TextBlockWriter`，在当前传递[SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)，和一个[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)其中它将创建[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)要用于跟踪输出：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

然后将将跟踪输出写入到新[TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)中创建[StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx)中传递：

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Windows Phone 客户端事件记录到调试控制台

若要将输出发送到调试控制台而不是 UI 中，创建的实现[TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) ，写入调试窗口中，并将其分配给连接的[TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx)属性：

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

然后将跟踪信息写入 Visual Studio 中的调试窗口：

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>在 JavaScript 客户端中启用跟踪

若要启用客户端日志记录在连接上，设置`logging`属性上的连接对象，然后才能调用`start`方法，以建立连接。

**客户端 JavaScript 代码来启用跟踪到浏览器控制台 （的生成的代理）**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**客户端 JavaScript 代码来启用跟踪到浏览器控制台 （而无需生成的代理）**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

当启用了跟踪时，JavaScript 客户端会将事件记录到浏览器控制台中。 若要访问浏览器控制台，请参阅[监视传输](../getting-started/introduction-to-signalr.md#MonitoringTransports)。

下面的屏幕截图显示 SignalR JavaScript 客户端使用启用的跟踪。 它显示浏览器控制台中的连接和中心调用事件：

![在浏览器控制台中的 SignalR 跟踪事件](enabling-signalr-tracing/_static/image4.png)
