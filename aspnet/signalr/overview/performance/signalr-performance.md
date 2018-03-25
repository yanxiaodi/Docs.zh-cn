---
uid: signalr/overview/performance/signalr-performance
title: "SignalR 性能 |Microsoft 文档"
author: pfletcher
description: "SignalR 性能"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance"></a>SignalR 性能
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本主题介绍如何设计、 测量和提高 SignalR 应用程序中的性能。
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


有关对 SignalR 性能和缩放一个新演示文稿，请参阅[缩放使用 ASP.NET SignalR 实时 Web](https://channel9.msdn.com/Events/Build/2013/3-502)。

本主题包含以下各节：

- [设计注意事项](#design)
- [优化性能你 SignalR 服务器](#tuning)
- [故障排除性能问题](#troubleshooting)
- [使用 SignalR 性能计数器](#perfcounters)
- [使用其他性能计数器](#othercounters)
- [其他资源](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>设计注意事项

本部分介绍可以实现 SignalR 应用程序，以确保不影响性能通过生成不必要的网络流量的设计过程中的模式。

### <a name="throttling-message-frequency"></a>限制消息频率

即使在发出高频率 （如实时游戏应用程序） 的消息的应用，大多数应用程序无需发送第二个较多消息。 若要减少的每个客户端生成的流量，消息循环可以实现队列和发送出消息不能超过固定费率频繁 （即最多一定数量的消息将发送每隔一秒，如果在该时间中有消息terval 发送)。 限制为某些传输率 （从客户端和服务器） 的消息的示例应用，请参阅[使用 SignalR 高频率实时](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)。

### <a name="reducing-message-size"></a>减少消息大小

你可以减少 SignalR 消息的大小，从而降低的序列化对象的大小。 在服务器代码中，如果您要发送的对象，其中包含要传输不需要的属性，防止这些属性进行序列化使用`JsonIgnore`属性。 属性的名称也存储在 message;可以使用缩短至属性的名称`JsonProperty`属性。 下面的代码示例演示如何从正在发送到客户端，排除属性以及如何缩短属性名称：

**.NET 服务器代码来演示 JsonIgnore 属性来排除数据被发送到客户端，以及要减少消息大小的 JsonProperty 属性**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

若要保留可读性 / 在客户端代码可维护性，在收到消息后，缩写的属性名称可以是重新映射到用户友好名称。 下面的代码示例演示如何重新映射到较长的通过定义消息协定 （映射） 的缩写的名称的可能方法和使用`reMap`函数可将协定应用于优化的消息类：

**重新映射的客户端 JavaScript 代码缩短到用户可读名称的属性名称**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

名称可以缩短到服务器，从客户端的消息中使用相同的方法。

减少内存占用量 （即的消息所使用的内存量） 消息的对象还可提高性能。 例如，如果的完整范围的`int`不需要`short`或`byte`可以改为使用。

因为消息存储在服务器内存，从而减少的消息的大小中的消息总线还可以解决服务器内存问题。

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>优化性能你 SignalR 服务器

以下配置设置可以用于调整为更好的性能 SignalR 应用程序服务器。 有关如何提高性能 ASP.NET 应用程序中的常规信息，请参阅[因而提高了 ASP.NET 性能](https://msdn.microsoft.com/library/ff647787.aspx)。

**SignalR 配置设置**

- **DefaultMessageBufferSize**： 默认情况下，SignalR 将保留在内存中的每个连接的中心每 1000 条消息。 如果正在使用大消息，这可能产生内存问题，这可以缓解通过减小此值。 可以在设置此设置`Application_Start`事件处理程序在 ASP.NET 应用程序，或在`Configuration`在自承载应用程序的 OWIN 启动类的方法。 下面的示例演示如何以减少你的应用程序，以减少使用的服务器内存量的内存需求量减小此值：

    **会在 Startup.cs 的.NET 服务器代码用于减少默认消息缓冲区大小**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS 配置设置**

- **每个应用程序的最大并发请求**： 并发 IIS 数目增加请求将增加可用于为请求提供服务的服务器资源。 默认值为 5000;若要增加此设置，请在提升的命令提示符执行以下命令：

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**： 这是最大 Http.sys 排队应用程序池的请求数。 如果队列已满，新请求将收到 503"服务不可用"响应。 默认值为 1000。

    缩短的工作进程中承载你的应用程序的应用程序池的队列长度将节省内存资源。 有关详细信息，请参阅[管理、 优化和配置应用程序池](https://technet.microsoft.com/library/cc745955.aspx)。

**ASP.NET 配置设置**

本节包含可以在中设置的配置设置`aspnet.config`文件。 两个位置之一，具体取决于平台中可以找到此文件：

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

可能会提高 SignalR 性能的 ASP.NET 设置包括：

- **每个 CPU 的最大并发请求**： 增加此设置可以缓解性能瓶颈。 若要增加此设置，添加以下配置设置来`aspnet.config`文件：

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **请求队列限制**： 当连接总数超过`maxConcurrentRequestsPerCPU`设置，ASP.NET 将开始限制使用队列的请求。 若要增加队列的大小，你可以增加`requestQueueLimit`设置。 若要执行此操作，将添加到以下配置设置`processModel`中的节点`config/machine.config`(而非`aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>故障排除性能问题

本部分介绍如何在你的应用程序中查找性能瓶颈。

### <a name="verifying-that-websocket-is-being-used"></a>验证正在使用 WebSocket

尽管 SignalR 可以使用各种传输方式，客户端和服务器之间的通信，WebSocket 提供显著的性能优势，并且应在客户端和服务器支持它。 若要确定客户端和服务器是否满足 WebSocket 的要求，请参阅[传输和回退](../getting-started/introduction-to-signalr.md#transports)。 若要确定你的应用程序中正在使用什么传输，可以使用浏览器开发人员工具，并检查日志以查看连接的使用什么传输。 有关使用 Internet Explorer 和 Chrome 中的浏览器开发工具的信息，请参阅[传输和回退](../getting-started/introduction-to-signalr.md#transports)。

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>使用 SignalR 性能计数器

本部分介绍如何启用和使用 SignalR 性能计数器，在中找到`Microsoft.AspNet.SignalR.Utils`包。

### <a name="installing-signalrexe"></a>安装 signalr.exe

可以使用名为 SignalR.exe 的实用工具向服务器添加性能计数器。 若要安装此实用程序，请按照下列步骤：

1. 在 Visual Studio 应用程序中，选择**工具**，**库程序包管理器**，**管理解决方案的 NuGet 包...**
2. 搜索**signalr.utils**，并选择安装。

    ![](signalr-performance/_static/image1.png)
3. 接受的许可协议才能安装包。
4. SignalR.exe 将安装到`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`。

### <a name="installing-performance-counters-with-signalrexe"></a>使用 SignalR.exe 安装性能计数器

若要安装 SignalR 性能计数器，请在提升的命令提示符使用以下参数运行 SignalR.exe:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

若要删除 SignalR 性能计数器，请在提升的命令提示符使用以下参数运行 SignalR.exe:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR 性能计数器

实用工具程序包将安装以下性能计数器。 "总"计数器测量事件的数，因为最后一个应用程序池或服务器重新启动。

**连接度量值**

以下度量值度量发生的连接生存期事件。 有关详细信息，请参阅[了解和处理连接生存期事件](../guide-to-the-api/handling-connection-lifetime-events.md)。

- **连接的连接**
- **重新连接的连接**
- **断开连接的连接**
- **连接当前**

**消息度量值**

以下度量值度量值产生 SignalR 的消息流量。

- **接收连接消息的总数**
- **发送的总连接消息**
- **连接接收的消息/秒**
- **连接发送的消息/秒**

**消息总线度量值**

以下度量值度量流量通过内部 SignalR 消息总线，位于所有传入和传出 SignalR 消息队列。 一条消息是**已发布**发送或广播时。 A**订阅服务器**在此上下文是消息总线上的订阅; 此选项应等于数客户端加上在服务器本身。 **分配辅助**是一个组件，将数据发送到活动连接;**忙的辅助**是指正在发送一条消息。

- **消息总线消息接收的总数**
- **消息总线消息 Received/Sec**
- **发布内容的消息总线消息总数**
- **消息总线邮件发布/秒**
- **消息总线订阅服务器当前**
- **消息总线订阅服务器总数**
- **消息总线订阅服务器/秒**
- **消息总线分配辅助角色**
- **消息总线忙碌工作线程**
- **消息总线主题当前**

**错误度量值**

以下度量值度量 SignalR 消息流量所生成错误。 **中心解析**无法解析中心或中心方法时发生错误。 **中心调用**错误是调用 hub 方法时引发的异常。 **传输**错误是在 HTTP 请求或响应过程中引发的连接错误。

- **错误： 所有总数**
- **错误： All/秒**
- **错误： 中心解析总数**
- **错误： 中心解析/秒**
- **错误： 中心调用总数**
- **错误： 中心调用/秒**
- **错误： 传输总数**
- **错误： 传输/秒**

<a id="scaleout_metrics"></a>

**向外缩放度量值**

以下度量值度量流量和向外扩展提供程序生成的错误。 A**流**在此上下文中程序是一个扩展单元向外扩展提供程序使用; 这是如果使用的 SQL Server 表，如果使用 Service Bus，主题和订阅如果使用 Redis。 每个流可确保有序的读取和写入操作;单个流是一个潜在的缩放瓶颈，因此可以增加流的数量，以帮助减少该瓶颈。 如果使用多个流，则 SignalR 将自动分布这些流的方式，以确保从任何给定的连接发送的消息是按顺序的 （分片） 消息。

[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)设置由 SignalR 维护的向外缩放发送队列长度的控件。 将其设置为一个值大于 0 将将所有消息都放入发送队列发送一次一个地到配置的消息传递基架。 如果队列的大小超过所配置的长度，将发送的后续调用失败，立即并[InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx)直到中队列的消息数小于设置试。 默认情况下，因为实现背板通常具有其自己的队列或流控制就地，队列处于禁用状态。 对于 SQL Server 连接池有效地限制进展情况在任何时候的发送的数目。

默认情况下，只有一个流适用于 SQL Server 和 Redis，五个流用于服务总线和队列处于禁用状态，但可以通过在 SQL Server 和 Service Bus 上的配置更改这些设置：

**.NET 服务器代码配置 SQL Server 底板的表计数和队列长度**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**.NET 服务器代码配置 Service Bus 底板的主题计数和队列长度**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

A**缓冲功能**流是指已进入错误的状态; 出错状态流时，发送到底板所有消息之前，都无法立即流不再出现错误。 **发送队列长度**是公布的但尚未发送的消息数。

- **向外扩展消息总线消息 Received/Sec**
- **向外缩放流总数**
- **向外缩放流式处理打开**
- **向外缩放流缓冲**
- **向外缩放错误总数**
- **Scaleout Errors/Sec**
- **向外缩放发送队列长度**

这些计数器测量的新增功能的详细信息，请参阅[采用 Azure Service Bus 的 SignalR 扩展](scaleout-with-windows-azure-service-bus.md)。

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>使用其他性能计数器

以下性能计数器也可能是用于监视应用程序的性能。

**Memory**

- .NET CLR 内存\\所有堆 （对于 w3wp) 中的 # 字节

**ASP.NET 2.0**

- ASP.NET\Requests Current
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- 处理器 Information\Processor 时间

**TCP/IP**

- TCPv6/已建立的连接
- TCPv4/已建立的连接

**Web 服务**

- Web Service\Current Connections
- Web Service\Maximum 连接

**线程处理**

- .NET CLR 锁定和线程\\的当前逻辑线程数
- .NET CLR 锁定和线程\\的当前物理线程数

<a id="otherresources"></a>

## <a name="other-resources"></a>其他资源

ASP.NET 性能监视和优化的详细信息，请参阅以下主题：

- [ASP.NET 性能概述](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [在 IIS 7.5、 IIS 7.0 中和 IIS 6.0 上的 ASP.NET 线程使用情况](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt;元素 （Web 设置）](https://msdn.microsoft.com/library/dd560842.aspx)
