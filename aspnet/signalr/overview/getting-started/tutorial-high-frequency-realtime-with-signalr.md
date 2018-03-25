---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: "教程： 高频率实时使用 SignalR 2 |Microsoft 文档"
author: pfletcher
description: "本教程演示如何创建的 web 应用程序使用 ASP.NET SignalR 来提供高频率的消息传递功能。 高频率在消息传递..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>教程： 高频率实时使用 SignalR 2
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

[下载已完成的项目](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> 本教程演示如何创建的 web 应用程序使用 ASP.NET SignalR 2 来提供高频率的消息传递功能。 在这种情况下消息传送高频率意味着发送，按固定费率; 的更新如果此应用程序，最多 10 条消息，第二个。
> 
> 你将在本教程中创建应用程序将显示用户可以拖动的形状。 在所有其他连接的浏览器中的形状位置然后将更新以匹配使用定时的更新拖动形状的位置。
> 
> 在本教程中引入的概念具有实时游戏中的应用程序和其他模拟应用程序。
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
> ## <a name="tutorial-versions"></a>教程版本
> 
> 有关 SignalR 的早期版本的信息，请参阅[SignalR 较旧版本](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>问题和意见
> 
> 请留下反馈上如何喜欢本教程的方式，我们可以提高在页面底部的注释中。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>概述

本教程演示如何创建的应用程序与其他实时浏览共享对象的状态。 我们将创建应用程序称为 MoveShape。 MoveShape 页将显示用户可拖动; HTML Div 元素当用户拖动 Div，则其新位置将发送到服务器，然后通知所有其他连接的客户端以更新该形状的位置，以匹配。

![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

在本教程中创建的应用程序基于 Damian Edwards 一个演示。 包含此演示视频，可以查看[此处](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。

本教程将演示如何从每个事件而激发该形状拖动发送 SignalR 消息启动。 每个连接的客户端然后将更新该形状的本地版本的位置每次收到一条消息。

虽然应用程序将使用此方法起作用，这不是建议的编程模型，因为将获取发送，因此在客户端和服务器无法获取压倒消息和性能将会降低的消息数没有上限. 客户端上显示的动画也将不相互连接，因为每个方法中，将立即移动形状，而不是移动顺利到每个新的位置。 本教程的后面部分将演示如何创建限制由客户端或服务器，发送消息的最大速率的计时器函数以及如何位置之间平稳移动形状。 在本教程中创建的应用程序的最终版本可以从下载[代码库](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。

本教程包含以下各节：

- [先决条件](#prerequisites)
- [创建项目并添加 SignalR 和 JQuery.UI NuGet 包](#createtheproject2013)
- [创建基本应用程序](#baseapp)
- [应用程序启动时启动中心](#startup2013)
- [添加客户端循环](#clientloop)
- [添加服务器循环](#serverloop)
- [客户端上添加平滑动画](#animation)
- [后续步骤](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>系统必备

本教程需要 Visual Studio 2013。

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>创建项目并添加 SignalR 和 JQuery.UI NuGet 包

在此部分中，我们将创建 Visual Studio 2013 中的项目。

以下步骤使用 Visual Studio 2013 创建 ASP.NET 空 Web 应用程序并添加 SignalR 和 jQuery.UI 库：

1. 在 Visual Studio 中创建 ASP.NET Web 应用程序。

    ![创建 web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. 在**新建 ASP.NET 项目**窗口中，保留**空**选择和单击**创建项目**。

    ![创建空 web](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. 在**解决方案资源管理器**，右键单击项目，选择**添加 |SignalR Hub 类 (v2)**。 将类**MoveShapeHub.cs**并将其添加到项目。 此步骤将创建**MoveShapeHub**类并将一组的脚本文件和支持 SignalR 的程序集引用添加到项目。

    > [!NOTE]
    > 你还可以将 SignalR 通过单击添加到项目**工具 |库包管理器 |程序包管理器控制台**并运行命令：

    `install-package Microsoft.AspNet.SignalR`。 

    如果你使用控制台添加 SignalR，单独的步骤创建 SignalR hub 类后添加 SignalR。
4. 单击**工具 |库包管理器 |程序包管理器控制台**。 在包管理器窗口中，运行以下命令：

    `Install-Package jQuery.UI.Combined`

    这将安装 jQuery UI 库中，你将使用来对形状进行动画处理。
5. 在**解决方案资源管理器**展开脚本节点。 JQuery、 jQueryUI，和 SignalR 的脚本库是在项目中可见。

    ![脚本库引用](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>创建基本应用程序

在此部分中，我们将创建的浏览器应用程序在每个鼠标移动事件向服务器发送的形状的位置。 接收到它时，服务器然后广播到所有其他连接的客户端此信息。 我们将在后面的部分中的此应用程序上展开。

1. 如果你尚未创建 MoveShapeHub.cs 类中，在**解决方案资源管理器**，右键单击该项目并选择**添加**，**类...**.将类**MoveShapeHub**单击**添加**。
2. 将在新代码**MoveShapeHub**类替换为以下代码。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub`上面类是 SignalR hub 的实现。 作为 in [Getting Started with SignalR](tutorial-getting-started-with-signalr.md)教程中，中心有一个客户端将直接调用的方法。 在这种情况下，客户端将发送一个对象，包含新的到服务器，然后获取已将广播到所有其他连接的客户端的形状的 X 和 Y 坐标。 SignalR 将自动进行此对象使用 JSON 序列化。

    将发送到客户端的对象 (`ShapeModel`) 包含成员来存储形状的位置。 在服务器上对象的版本还包含成员来跟踪正在存储哪种客户端的数据，以便给定客户端将不会发送自己的数据。 此成员使用`JsonIgnore`属性以防止其进行序列化并发送到客户端。

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>应用程序启动时启动中心

1. 接下来，我们将设置到中心的映射，应用程序启动时。 在 SignalR 2 中，这可通过添加 OWIN 启动类，该类将调用`MapSignalR`时 startup 类的`Configuration`OWIN 启动时执行方法。 类将添加到 OWIN 的启动处理使用`OwinStartup`程序集特性。

    在**解决方案资源管理器**，右键单击项目，然后单击**添加 |OWIN Startup 类**。 将类*启动*单击**确定**。
2. 将 Startup.cs 的内容更改为以下：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>添加客户端

1. 接下来，我们将添加客户端。 在**解决方案资源管理器**，右键单击项目，然后单击**添加 |新项**。 在**添加新项**对话框中，选择**Html 页**。 将该页命名为**Default.html**单击**添加**。
2. 在**解决方案资源管理器**，右键单击你刚刚创建的页面，然后单击**设为起始页**。
3. 在 HTML 页中的默认代码替换为下面的代码段。

    > [!NOTE]
    > 验证脚本引用匹配下面添加到你项目的脚本文件夹中的包。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    上面的 HTML 和 JavaScript 代码创建红色 Div 调用形状、 启用形状的拖动行为使用 jQuery 库，并使用形状的`drag`事件，以向服务器发送该形状的位置。
4. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>添加客户端循环

由于发送的每个鼠标移动事件上的形状的位置将创建不必要大量网络流量，从客户端的消息需要被阻止。 我们将使用 javascript`setInterval`函数设置向按固定费率服务器发送新的位置信息的循环。 此循环是非常基本的表示形式"游戏循环"，驱动器的所有功能的游戏或其他模拟被反复调用函数。

1. 更新要匹配下面的代码段的 HTML 页面中的客户端代码。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    上面的更新将添加`updateServerModel`函数，该固定频率会被调用函数。 此函数将位置数据发送到服务器时`moved`标志指示没有要发送的新位置数据。
2. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。 获取发送到服务器的消息数将被阻止，因为动画将不会出现顺利如前一节中所示。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>添加服务器循环

在当前应用程序，从服务器到客户端发送的消息转通常它们被接收。 这显示类似的问题，如客户端; 上发现通常比它们必需的和连接可能会变得溢满因此可以将消息发送。 本部分介绍如何更新服务器以实现的计时器，限制的传出消息的速率。

1. 内容替换`MoveShapeHub.cs`替换为以下代码片段。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    上面的代码中扩展客户端添加`Broadcaster`类，该类限制传出消息使用`Timer`从.NET framework 的类。

    由于集线器本身是暂时性 （它在每次创建需要它），`Broadcaster`将被创建为单一实例。 （在.NET 4 中引入） 的延迟初始化用于其创建的延迟，直到它需要时，确保计时器启动之前完全创建第一个中心实例。

    客户端的调用`UpdateShape`函数然后移出中心的`UpdateModel`方法，因此它不再称为立即每当接收传入消息。 相反，将每秒，25 调用的速率发送到客户端的消息由`_broadcastLoop`中的计时器`Broadcaster`类。

    最后，而不是调用客户端方法从中心直接，`Broadcaster`类需要获取对当前正在运行的中心的引用 (`_hubContext`) 使用`GlobalHost`。
2. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。 不会在浏览器中从上一节中明显的差异，但将被阻止到客户端发送的消息数。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>客户端上添加平滑动画

应用程序即将完成，但我们无法使一个详细改善，客户端上的形状的动态移动以响应服务器消息。 而不是将形状的位置设置为给定服务器的新位置，我们将使用 JQuery UI 库`animate`函数用于顺利翻其当前和新位置的形状。

1. 更新客户端的`updateShape`方法看起来像下面突出显示的代码：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    上面的代码将从旧位置形状移到新服务器获得的高于动画间隔 （以这种情况下，100 毫秒） 的过程。 此新动画在开始之前清除在形状上运行任何前一动画。
2. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。 在其他窗口中的形状的移动应显示为其移动插值通过时间，而不是每个传入消息一次设置不太平稳。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>后续步骤

在本教程中，你已了解如何发送客户端和服务器之间的高频率消息 SignalR 应用程序进行编程。 此通信范例可用于开发联机游戏和其他模拟，如[ShootR 游戏使用 SignalR 创建](http://shootr.signalr.net)。

在本教程中创建的完整应用程序可以从下载[代码库](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。

若要了解有关 SignalR 开发概念的详细信息，请访问以下站点 SignalR 源代码和资源：

- [SignalR 项目](http://signalr.net)
- [SignalR Github 和示例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

有关如何部署到 Azure 的 SignalR 应用程序的演练，请参阅[将 signalr 与在 Azure App Service Web Apps 配合](../deployment/using-signalr-with-azure-web-sites.md)。 有关如何将 Visual Studio web 项目部署到 Windows Azure 网站的详细信息，请参阅[在 Azure App Service 中创建 ASP.NET web 应用](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。
