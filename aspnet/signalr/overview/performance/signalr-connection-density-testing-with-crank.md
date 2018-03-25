---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: "使用曲柄测试 SignalR 连接密度 |Microsoft 文档"
author: tfitzmac
description: "使用曲柄测试 SignalR 连接密度"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
<a name="signalr-connection-density-testing-with-crank"></a>使用曲柄测试 SignalR 连接密度
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本文介绍如何使用曲柄工具来测试与多个模拟客户端应用程序。


后在其宿主环境 （任一 Azure web 角色，IIS，或使用 Owin 自承载） 中运行你的应用程序，你可以测试应用程序的响应使用曲柄工具连接密度的高级别。 宿主环境可以是 Internet 信息服务 (IIS) 服务器、 Owin 主机或 Azure web 角色。 (注意： 性能计数器不可在 Azure App Service Web Apps，因此你将无法从连接密度测试获取性能数据。)

连接密度是指可以在服务器建立的并发 TCP 连接数。 每个 TCP 连接可能会产生其自己的开销，并打开大量的空闲连接将最终创建内存瓶颈。

[SignalR 基本代码](https://github.com/signalr/signalr)包括一个名为负载测试工具**Crank**。 在找不到最新版本的曲柄[Dev 分支](https://github.com/SignalR/signalr/tree/dev)GitHub 上。 你可以下载的 Zip 存档 SignalR 的 Dev 分支的基本代码[此处](https://github.com/SignalR/SignalR/archive/dev.zip)。

曲柄可能用于完全 saturate 服务器的内存以便计算可能服务器硬件上的空闲连接的总数。 或者，你可能还用于曲柄负载测试了一定数量的内存压力下的服务器通过掌握连接，直至达到特定计数或特定内存阈值。

在测试时，务必使用远程客户端以避免任何竞争的资源 （即 TCP 连接和内存）。 监视客户端以确保它们不会命中可能会阻止服务器达到其完整的容量 （内存或 CPU） 任何瓶颈。 你可能需要增加才能完全加载服务器的客户端的数量。

### <a name="running-a-connection-density-test"></a>运行连接密度测试

本部分介绍 SignalR 应用程序上运行连接密度测试所需的步骤。

1. 下载并构建[SignalR 的开发分支的基本代码](https://github.com/SignalR/SignalR/archive/dev.zip)。 在命令提示符，导航到&lt;项目目录&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug。
2. 应用程序部署到其预期的宿主环境。 记下你的应用程序使用; 终结点例如，在中创建的应用程序[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)，终结点是`http://<yourhost>:8080/signalr`。
3. 安装[SignalR 性能计数器](signalr-performance.md#perfcounters)服务器上。 如果在 Azure 上运行你的应用程序，请参阅[Azure Web 角色中使用 SignalR 性能计数器](using-signalr-performance-counters-in-an-azure-web-role.md)。

一旦你已下载和生成的基本代码，并在主机上安装性能计数器，可以在找到曲柄命令行工具`src\Microsoft.AspNet.SignalR.Crank\bin\Debug`文件夹。

曲柄工具的可用选项包括：

- **/?**： 显示帮助屏幕。 如果也会显示可用选项**Url**省略参数。
- **/ Url**: SignalR 连接的 URL。 此参数是必需的。 使用默认映射 SignalR 的应用程序的情况下，路径将以结束"/ signalr"。
- **/ 传输**： 所使用的传输的名称。 默认值是`auto`，这将选择最佳的可用的协议。 选项包括`WebSockets`， `ServerSentEvents`，和`LongPolling`(`ForeverFrame`不是选项曲柄，因为.NET 客户端，而不使用 Internet Explorer)。 有关 SignalR 如何选择传输的详细信息，请参阅[传输和回退](../getting-started/introduction-to-signalr.md#transports)。
- **/ BatchSize**： 添加每个批中的客户端的数量。 默认值为 50。
- **/ ConnectInterval**: 间隔 （毫秒） 之间添加连接。 默认值为 500。
- **/ 连接**： 用于负载测试应用程序的连接数。 默认值为 100,000。
- **/ ConnectTimeout**： 秒，然后中止测试的超时。 默认值为 300。
- **MinServerMBytes**： 来访问的最小服务器兆字节。 默认值为 500。
- **SendBytes**： 发送到服务器以字节为单位的负载的大小。 默认值为 0。
- **SendInterval**： 延迟的消息与服务器之间的毫秒数。 默认值为 500。
- **SendTimeout**： 超时以毫秒为单位的到服务器的消息。 默认值为 300。
- **ControllerUrl**： 其中一台客户端将承载的控制器集线器的 Url。 默认值为 null （无控制器中心）。 时曲柄会话启动; 启动控制器中心没有任何进一步进行控制器中心和曲柄之间的联系。
- **NumClients**： 模拟客户端连接到应用程序的数量。 默认值为一个。
- **日志文件**： 为测试运行日志文件的文件名。 默认值为 `crank.csv`。
- **SampleInterval**： 的时间性能计数器样本之间的毫秒。 默认值为 1000。
- **SignalRInstance**： 服务器上的性能计数器的实例名称。 默认值是使用客户端连接状态。

### <a name="example"></a>示例

以下命令将测试名的网站`pfsignalr`在 Azure 上承载使用名为"ControllerHub"集线器端口 8080 上的应用程序，使用 100 个连接。

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
