---
uid: signalr/overview/performance/scaleout-with-sql-server
title: "与 SQL Server 的 SignalR 扩展 |Microsoft 文档"
author: MikeWasson
description: "Visual Studio 2013.NET 4.5 SignalR 本主题中使用软件版本的早期版本的信息的本主题的版本 2 早期版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 18ce212f5cb7849d522248f9c462b5b48e3487ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-sql-server"></a>与 SQL Server 的 SignalR 扩展
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


在本教程中，你将使用 SQL Server 以将消息分布到的 SignalR 应用程序部署在两个单独的 IIS 实例。 此外可以在单个测试计算机上，运行本教程，但若要获取完整的效果，你需要 SignalR 将应用部署到两个或多个服务器。 在一台服务器，或单独的专用服务器上，还必须安装 SQL Server。 另一种方法是运行在 Azure 上使用虚拟机的教程。

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>系统必备

Microsoft SQL Server 2005 或更高版本。 底板支持桌面和服务器版本的 SQL Server。 它不支持 SQL Server Compact Edition 或 Azure SQL 数据库。 （如果你的应用程序托管在 Azure 上，请考虑服务总线底板相反。）

## <a name="overview"></a>概述

我们获取详细的教程之前，以下是将要执行的操作的快速概览。

1. 创建一个新的空数据库。 底板将此数据库中创建必要的表。
2. 将这些 NuGet 包添加到你的应用程序： 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. 创建 SignalR 应用程序。
4. 将以下代码添加到 Startup.cs 配置底板： 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 此代码使用的默认值配置底板[TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx)和[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)。 有关更改这些值的信息，请参阅[SignalR 性能： 向外缩放度量值](signalr-performance.md#scaleout_metrics)。 

## <a name="configure-the-database"></a>配置数据库

确定应用程序是否将使用 Windows 身份验证或 SQL Server 身份验证来访问数据库。 无论如何，请确保数据库用户有权登录、 创建架构，并创建表。

创建用于底板以使用新的数据库。 你可以为数据库指定任何名称。 不需要在数据库中; 中创建的任何表底板将创建必要的表。

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>启用 Service Broker

建议为底板数据库启用 Service Broker。 Service Broker 提供的消息和队列在 SQL Server，这样就更有效地接收更新底板中的本机支持。 （但是，底板还未工作 Service Broker。）

若要检查是否启用 Service Broker，请查询**是\_broker\_启用**中的列**sys.databases**目录视图。

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

若要启用 Service Broker，请使用以下 SQL 查询：

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> 如果此查询出现死锁，请确保没有连接到的数据库应用程序。


如果已启用跟踪，则跟踪还将显示是否已启用 Service Broker。

## <a name="create-a-signalr-application"></a>创建 SignalR 应用程序

按照这些教程任一创建 SignalR 应用程序：

- [开始使用 SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR 2.0 和 MVC 5 入门](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

接下来，我们将修改的聊天应用程序，以支持与 SQL Server 的向外缩放。 首先，将 SignalR.SqlServer NuGet 包添加到你的项目。 在 Visual Studio 中，从**工具**菜单上，选择**库程序包管理器**，然后选择**程序包管理器控制台**。 在 Package Manager Console 窗口中，输入以下命令：

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

接下来，打开 Startup.cs 文件。 以下代码添加到**配置**方法：

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>部署并运行应用程序

准备您的 Windows 服务器实例部署 SignalR 应用程序。

添加 IIS 角色。 包括"应用程序开发"功能，包括 WebSocket 协议。

![](scaleout-with-sql-server/_static/image4.png)

此外包括管理服务 （列于"管理工具"下）。

![](scaleout-with-sql-server/_static/image5.png)

**安装 Web 部署 3.0。** 当你运行 IIS 管理器，它将提示你安装 Microsoft Web 平台，或者可以[下载 intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)。 在平台安装程序中，搜索 Web 部署并安装 Web 部署 3.0

![](scaleout-with-sql-server/_static/image6.png)

检查 Web 管理服务正在运行。 如果没有，请启动服务。 （如果你看不到 Web 管理服务中的 Windows 服务的列表，请确保你添加的 IIS 角色时安装管理服务。）

最后，打开 TCP 端口 8172。 这是 Web 部署工具使用的端口。

现在你已准备好部署到服务器从你的开发计算机的 Visual Studio 项目。 在解决方案资源管理器，右键单击解决方案，然后单击**发布**。

有关更多详细有关 web 部署的文档，请参阅[for Visual Studio 和 ASP.NET 的 Web 部署内容映射](../../../whitepapers/aspnet-web-deployment-content-map.md)。

如果你部署到两个服务器应用程序，可以在一个单独的浏览器窗口中打开每个实例，并查看每个接收来自其他 SignalR 消息。 （当然，在生产环境中，两台服务器将位于以下位置负载平衡器之后。）

![](scaleout-with-sql-server/_static/image7.png)

运行应用程序后，你可以看到 SignalR 自动具有在数据库中创建表：

![](scaleout-with-sql-server/_static/image8.png)

SignalR 管理表。 只要应用程序部署，不删除行、 修改表中，和等等。
