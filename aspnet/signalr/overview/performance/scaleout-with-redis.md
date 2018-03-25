---
uid: signalr/overview/performance/scaleout-with-redis
title: "采用 Redis 的 SignalR 扩展 |Microsoft 文档"
author: MikeWasson
description: "Visual Studio 2013.NET 4.5 SignalR 本主题中使用软件版本的早期版本的信息的本主题的版本 2 早期版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2ef161f35e69ef4a754d2740199166ee48c3fbab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-redis"></a>采用 Redis 的 SignalR 扩展
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


在本教程中，你将使用[Redis](http://redis.io/)若要将消息分布到的 SignalR 应用程序部署在两个单独的 IIS 实例上。

Redis 是一个内存中键 / 值存储。 它还支持发布/订阅模型包含的消息传送系统。 SignalR Redis 底板使用发布/订阅功能来将消息转发到其他服务器。

![](scaleout-with-redis/_static/image1.png)

对于本教程中，你将使用三个服务器：

- 两台运行 Windows，你将使用部署 SignalR 应用程序的服务器。
- 一台运行 Linux，你将用于运行 Redis 服务器。 对于本教程中的屏幕截图，我将使用 Ubuntu 12.04 TLS。

如果你不具有三个物理服务器使用，你可以在 HYPER-V 上创建 Vm。 另一个选项是在 Azure 上创建 Vm。

虽然本教程使用的官方 Redis 实现，此外还有[Windows 端口的 Redis](https://github.com/MSOpenTech/redis)从 MSOpenTech。 安装和配置不相同，但否则步骤是相同的。

> [!NOTE] 
> 
> 采用 Redis 的 SignalR 扩展不支持 Redis 群集。


## <a name="overview"></a>概述

我们获取详细的教程之前，以下是将要执行的操作的快速概览。

1. 安装 Redis 并启动 Redis 服务器。
2. 将这些 NuGet 包添加到你的应用程序： 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. 创建 SignalR 应用程序。
4. 将以下代码添加到 Startup.cs 配置底板： 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>HYPER-V 上的 Ubuntu

使用 Windows HYPER-V，你可以轻松创建 Ubuntu VM 在 Windows Server 上。

下载 Ubuntu ISO 从[http://www.ubuntu.com](http://www.ubuntu.com/)。

Hyper-v 中，添加新的 VM。 在**连接虚拟硬盘**步骤中，选择**创建虚拟硬盘**。

![](scaleout-with-redis/_static/image2.png)

在**安装选项**步骤中，选择**映像文件 (.iso)**，单击**浏览**，并浏览到 Ubuntu 安装 ISO。

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>安装 Redis

遵循的步骤[http://redis.io/download](http://redis.io/download)若要下载并构建 Redis。

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

这将生成 Redis 二进制文件`src`目录。

默认情况下，Redis 不需要密码。 若要设置密码，编辑`redis.conf`文件，该文件夹位于源代码的根目录中。 （使该文件的备份副本，在编辑前） ！添加到以下指令`redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

现在，开始 Redis 服务器：

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

打开端口 6379，后者是 Redis 的默认端口上侦听。 （您可以更改配置文件中的端口号）。

## <a name="create-the-signalr-application"></a>创建 SignalR 应用程序

按照这些教程任一创建 SignalR 应用程序：

- [开始使用 SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR 2.0 和 MVC 5 入门](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

接下来，我们将修改的聊天应用程序，以支持采用 Redis 的扩展。 首先，将 SignalR.Redis NuGet 包添加到你的项目。 在 Visual Studio 中，从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

接下来，打开 Startup.cs 文件。 以下代码添加到**配置**方法：

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "服务器"是正在运行 Redis 服务器的名称。
- *端口*是端口号
- "password"是 redis.conf 文件中定义的密码。
- "AppName"为任何字符串。 SignalR 具有此名称创建 Redis 发布/订阅通道。

例如:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>部署并运行应用程序

准备您的 Windows 服务器实例部署 SignalR 应用程序。

添加 IIS 角色。 包括"应用程序开发"功能，包括 WebSocket 协议。

![](scaleout-with-redis/_static/image5.png)

此外包括管理服务 （列于"管理工具"下）。

![](scaleout-with-redis/_static/image6.png)

**安装 Web 部署 3.0。** 当你运行 IIS 管理器，它将提示你安装 Microsoft Web 平台，或者可以[下载 intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)。 在平台安装程序中，搜索 Web 部署并安装 Web 部署 3.0

![](scaleout-with-redis/_static/image7.png)

检查 Web 管理服务正在运行。 如果没有，请启动服务。 （如果你看不到 Web 管理服务中的 Windows 服务的列表，请确保你添加的 IIS 角色时安装管理服务。）

默认情况下，Web 管理服务侦听 TCP 端口 8172。 在 Windows 防火墙中创建一个新的入站的规则以允许端口 8172 上的 TCP 通信。 有关详细信息，请参阅[配置防火墙规则](https://technet.microsoft.com/library/dd448559(WS.10).aspx)。 （如果你托管在 Azure 上的 Vm，你可以执行此操作直接在 Azure 门户中。 请参阅[如何设置虚拟机的终结点](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/)。)

现在你已准备好部署到服务器从你的开发计算机的 Visual Studio 项目。 在解决方案资源管理器，右键单击解决方案，然后单击**发布**。

有关更多详细有关 web 部署的文档，请参阅[for Visual Studio 和 ASP.NET 的 Web 部署内容映射](../../../whitepapers/aspnet-web-deployment-content-map.md)。

如果你部署到两个服务器应用程序，可以在一个单独的浏览器窗口中打开每个实例，并查看每个接收来自其他 SignalR 消息。 （当然，在生产环境中，两台服务器将位于以下位置负载平衡器之后。）

![](scaleout-with-redis/_static/image8.png)

如果你想要查看的消息发送到 Redis，你可以使用**redis cli**客户端，使用 Redis 安装。

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
