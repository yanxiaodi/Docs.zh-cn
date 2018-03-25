---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: "教程： 自承载 SignalR |Microsoft 文档"
author: pfletcher
description: "本教程演示如何创建自承载的 SignalR 2 服务器，以及如何与 JavaScript 客户端连接到它。 教程 V 中使用的软件版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-signalr-self-host"></a>自承载的教程： SignalR
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

[下载已完成的项目](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> 本教程演示如何创建自承载的 SignalR 2 服务器，以及如何与 JavaScript 客户端连接到它。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 版本 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>本教程使用 Visual Studio 2012
> 
> 
> 若要使用本教程使用 Visual Studio 2012，请执行以下操作：
> 
> - 更新你[程序包管理器](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。
> - 安装[Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web 平台安装程序中，搜索并安装**ASP.NET 和 Web Tools for Visual Studio 2012 的 2013.1**。 这将安装 SignalR 类的 Visual Studio 模板，如**中心**。
> - 某些模板 (如**OWIN Startup 类**) 将不可用; 对于这些数据，改为使用的类文件。
> 
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>概述

SignalR 服务器通常承载在 IIS 中，ASP.NET 应用程序中，但它也可以是自承载 （如控制台应用程序或 Windows 服务） 使用的自承载的库。 此库，就像所有 SignalR 2 基于 OWIN ([打开适用于.NET 的 Web 界面](http://owin.org))。 OWIN 定义.NET web 服务器和 web 应用程序之间的抽象。 OWIN 将在服务器上，这使 OWIN 适合于你自己的过程，在 IIS 外部的 web 应用程序的自承载的 web 应用程序中脱离出来。

不在 IIS 中承载的原因包括：

- IIS 不可用或必要的如不含 IIS 的现有服务器场的其中的环境。
- IIS 的性能开销需要避免这样做。
- SignalR 功能是添加到的现有应用程序在 Windows 服务、 Azure 辅助角色或其他进程中运行。

如果正在作为自承载出于性能原因开发解决方案，它具有测试也建议在 IIS 中托管的应用程序来确定的性能优势。

本教程包含以下各节：

- [创建服务器](#server)
- [访问与 JavaScript 客户端的服务器](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>创建服务器

在本教程中，你将在控制台应用程序中，创建托管的服务器，但服务器可以承载于任何种类的过程中，如 Windows 服务或 Azure 辅助角色。 有关用于宿主 SignalR 服务器 Windows 服务中的示例代码，请参阅[Windows 服务中的 Self-Hosting SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)。

1. 使用管理员特权打开 Visual Studio 2013。 选择**文件**，**新项目**。 选择**Windows**下**Visual C#**中的节点**模板**窗格中，然后选择**控制台应用程序**模板。 将新项目"SignalRSelfHost"，然后单击**确定**。

    ![](tutorial-signalr-self-host/_static/image1.png)
2. 通过选择打开库包管理器控制台**工具**，**库程序包管理器**，**程序包管理器控制台**。
3. 在包管理器控制台中，输入以下命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    此命令将 SignalR 2 自承载库添加到项目。
4. 在包管理器控制台中，输入以下命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    此命令将 Microsoft.Owin.Cors 库添加到项目。 此库将用于跨域支持，需要托管 SignalR 和不同的域中的网页客户端应用程序。 由于你将托管 SignalR 服务器和 web 客户端在不同的端口，这意味着必须为这些组件之间的通信启用该跨域。
5. 将 Program.cs 的内容替换为以下代码。

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    上面的代码包括三个类：

    - **程序**，包括**Main**方法定义执行的主要路径。 在此方法中，类型的 web 应用程序**启动**开始时间为指定的 URL (`http://localhost:8080`)。 如果安全终结点上需要，可以实现 SSL。 请参阅[如何： 使用 SSL 证书配置端口](https://msdn.microsoft.com/library/ms733791.aspx)有关详细信息。
    - **启动**、 包含 SignalR 服务器的配置的类 (本教程使用的唯一配置是对调用`UseCors`)，和对`MapSignalR`，其中的项目中创建中心的任何对象的路由。
    - **MyHub**，应用程序将提供给客户端的 SignalR Hub 类。 此类具有一个方法，**发送**，客户端将调用将一条消息广播到所有其他连接的客户端。
6. 编译并运行该应用程序。 服务器正在运行的地址应显示在控制台窗口中。

    ![](tutorial-signalr-self-host/_static/image2.png)
7. 如果执行失败，异常`System.Reflection.TargetInvocationException was unhandled`，你将需要重新启动 Visual Studio，使用管理员特权。
8. 停止应用程序，然后继续下一节。

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>访问与 JavaScript 客户端的服务器

在本部分中，你将使用相同的 JavaScript 客户端从[入门教程](../getting-started/tutorial-getting-started-with-signalr.md)。 我们将仅使到客户端，则需要显式定义的中心 URL 的一个修改。 使用自承载的应用程序，服务器可能不一定是在相同的地址作为连接 URL （由于反向代理和负载平衡器），因此需要显式定义该 URL。

1. 在**解决方案资源管理器**，右击该解决方案并选择**添加**，**新项目**。 选择**Web**节点，然后选择**ASP.NET Web 应用程序**模板。 将"JavascriptClient"的项目，然后单击**确定**。

    ![](tutorial-signalr-self-host/_static/image3.png)
2. 选择**空**模板，并保留未选定的剩余选项。 选择**创建项目**。

    ![](tutorial-signalr-self-host/_static/image4.png)
3. 在包管理器控制台中，选择中的"JavascriptClient"项目**默认项目**下拉列表中，并执行以下命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    此命令将在客户端安装你将需要的 SignalR 和 JQuery 库。
4. 右键单击项目并选择**添加**，**新项**。 选择**Web**节点，然后选择 HTML 页。 将该页命名为**Default.html**。

    ![](tutorial-signalr-self-host/_static/image5.png)
5. 新的 HTML 页的内容替换为以下代码。 请验证脚本引用此处匹配项目的脚本文件夹中的脚本。

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    下面的代码 （在上面的代码示例中突出显示） 是对客户端使用 （除了 SignalR beta 版 2 到升级代码） 获取开始本教程中所做的添加。 此代码行显式设置的基本连接 URL SignalR 在服务器上。

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. 右键单击该解决方案，然后选择**设置启动项目...**.选择**多启动项目**单选按钮，并设置这两个项目的**操作**到**启动**。

    ![](tutorial-signalr-self-host/_static/image6.png)
7. 右键单击"Default.html"并选择**设为起始页**。
8. 运行该应用程序。 服务器和页上将启动。 你可能需要重新加载网页 (或选择**继续**在调试器中) 如果该服务器已启动之前加载页面。
9. 在浏览器中，提供用户名出现提示时。 该页面的 URL 复制到另一个浏览器选项卡或窗口，并提供不同的用户名。 你将能够将消息从一个浏览器窗格中发送到另一个，如下所示入门教程。
