---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: "高频率实时使用 SignalR 1.x |Microsoft 文档"
author: pfletcher
description: "本教程演示如何创建的 web 应用程序使用 ASP.NET SignalR 来提供高频率的消息传递功能。 高频率在消息传递..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="high-frequency-realtime-with-signalr-1x"></a>高频率实时使用 SignalR 1.x
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本教程演示如何创建的 web 应用程序使用 ASP.NET SignalR 来提供高频率的消息传递功能。 在这种情况下消息传送高频率意味着发送，按固定费率; 的更新如果此应用程序，最多 10 条消息，第二个。
> 
> 你将在本教程中创建应用程序将显示用户可以拖动的形状。 在所有其他连接的浏览器中的形状位置然后将更新以匹配使用定时的更新拖动形状的位置。
> 
> 在本教程中引入的概念具有实时游戏中的应用程序和其他模拟应用程序。
> 
> 在本教程的注释是欢迎。 如果你有与本教程不直接相关的问题，你可以发布到[ASP.NET SignalR 论坛](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com)。


## <a name="overview"></a>概述

本教程演示如何创建的应用程序与其他实时浏览共享对象的状态。 我们将创建应用程序称为 MoveShape。 MoveShape 页将显示用户可拖动; HTML Div 元素当用户拖动 Div，则其新位置将发送到服务器，然后通知所有其他连接的客户端以更新该形状的位置，以匹配。

![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

在本教程中创建的应用程序基于 Damian Edwards 一个演示。 包含此演示视频，可以查看[此处](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。

本教程将演示如何从每个事件而激发该形状拖动发送 SignalR 消息启动。 每个连接的客户端然后将更新该形状的本地版本的位置每次收到一条消息。

虽然应用程序将使用此方法起作用，这不是建议的编程模型，因为将获取发送，因此在客户端和服务器无法获取压倒消息和性能将会降低的消息数没有上限. 客户端上显示的动画也将不相互连接，因为每个方法中，将立即移动形状，而不是移动顺利到每个新的位置。 本教程的后面部分将演示如何创建限制由客户端或服务器，发送消息的最大速率的计时器函数以及如何位置之间平稳移动形状。 在本教程中创建的应用程序的最终版本可以从下载[代码库](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。

本教程包含以下各节：

- [先决条件](#prerequisites)
- [创建项目](#createtheproject)
- [添加 ASP.NET SignalR 和 JQuery.UI NuGet 包](#nugetpackages)
- [创建基本应用程序](#baseapp)
- [添加客户端循环](#clientloop)
- [添加服务器循环](#serverloop)
- [客户端上添加平滑动画](#animation)
- [后续步骤](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>先决条件

本教程需要 Visual Studio 2012 或 Visual Studio 2010。 如果使用 Visual Studio 2010，则该项目将使用.NET Framework 4 而不是.NET Framework 4.5。

如果你正在使用 Visual Studio 2012，建议你安装[ASP.NET 和 Web Tools 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。 此更新包含新功能，如发布、 新功能的增强功能和新模板。

如果你有 Visual Studio 2010，请确保[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安装。

<a id="createtheproject"></a>

## <a name="create-the-project"></a>创建项目

在此部分中，我们将创建 Visual Studio 中的项目。

1. 从**文件**菜单上，单击**新项目**。
2. 在**新项目**对话框框中，展开**C#**下**模板**和选择**Web**。
3. 选择**ASP.NET 空 Web 应用程序**模板，将项目*MoveShapeDemo*，然后单击**确定**。

    ![创建新项目](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>添加 SignalR 和 JQuery.UI NuGet 包

你可以通过安装 NuGet 包添加到项目的 SignalR 功能。 本教程还将使用 JQuery.UI 包允许该形状以拖放，进行动画处理。

1. 单击**工具 |库包管理器 |程序包管理器控制台**。
2. 在程序包管理器中输入以下命令。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    SignalR 包安装大量其他 NuGet 包依赖关系。 完成安装后你将具有所有 ASP.NET 应用程序中使用 SignalR 所需的服务器和客户端组件。
3. 在包管理器控制台，若要安装的 JQuery 和 JQuery.UI 包输入以下命令。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>创建基本应用程序

在此部分中，我们将创建的浏览器应用程序在每个鼠标移动事件向服务器发送的形状的位置。 接收到它时，服务器然后广播到所有其他连接的客户端此信息。 我们将在后面的部分中的此应用程序上展开。

1. 在**解决方案资源管理器**，右键单击该项目并选择**添加**，**类...**.将类**MoveShapeHub**单击**添加**。
2. 将在新代码**MoveShapeHub**类替换为以下代码。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub`上面类是 SignalR hub 的实现。 作为 in [Getting Started with SignalR](index.md)教程中，中心有一个客户端将直接调用的方法。 在这种情况下，客户端将发送一个对象，包含新的到服务器，然后获取已将广播到所有其他连接的客户端的形状的 X 和 Y 坐标。 SignalR 将自动进行此对象使用 JSON 序列化。

    将发送到客户端的对象 (`ShapeModel`) 包含成员来存储形状的位置。 在服务器上对象的版本还包含成员来跟踪正在存储哪种客户端的数据，以便给定客户端将不会发送自己的数据。 此成员使用`JsonIgnore`属性以防止其进行序列化并发送到客户端。
3. 接下来，我们将设置中心应用程序启动时。 在**解决方案资源管理器**，右键单击项目，然后单击**添加 |全局应用程序类**。 接受默认名称的*全局*单击**确定**。

    ![添加全局应用程序类](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. 添加以下`using`后提供语句**使用**Global.asax.cs 类中的语句。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. 添加以下行中的代码`Application_Start`全局类，以便为 SignalR 注册默认路由的方法。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Global.asax 文件应如下所示：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. 接下来，我们将添加客户端。 在**解决方案资源管理器**，右键单击项目，然后单击**添加 |新项**。 在**添加新项**对话框中，选择**Html 页**。 为页面提供一个适当的名称 (如**Default.html**)，然后单击**添加**。
7. 在**解决方案资源管理器**，右键单击你刚刚创建的页面，然后单击**设为起始页**。
8. 在 HTML 页中的默认代码替换为下面的代码段。

    > [!NOTE]
    > 验证脚本引用匹配下面添加到你项目的脚本文件夹中的包。 在 Visual Studio 2010 中，JQuery 和 SignalR 添加到项目的版本可能与下面的版本编号不匹配。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    上面的 HTML 和 JavaScript 代码创建红色 Div 调用形状、 启用形状的拖动行为使用 jQuery 库，并使用形状的`drag`事件，以向服务器发送该形状的位置。
9. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>添加客户端循环

由于发送的每个鼠标移动事件上的形状的位置将创建不必要大量网络流量，从客户端的消息需要被阻止。 我们将使用 javascript`setInterval`函数设置向按固定费率服务器发送新的位置信息的循环。 此循环是非常基本的表示形式"游戏循环"，驱动器的所有功能的游戏或其他模拟被反复调用函数。

1. 更新要匹配下面的代码段的 HTML 页面中的客户端代码。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    上面的更新将添加`updateServerModel`函数，该固定频率会被调用函数。 此函数将位置数据发送到服务器时`moved`标志指示没有要发送的新位置数据。
2. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。 获取发送到服务器的消息数将被阻止，因为动画将不会出现顺利如前一节中所示。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>添加服务器循环

在当前应用程序，从服务器到客户端发送的消息转通常它们被接收。 这显示类似的问题，如客户端; 上发现通常比它们必需的和连接可能会变得溢满因此可以将消息发送。 本部分介绍如何更新服务器以实现的计时器，限制的传出消息的速率。

1. 内容替换`MoveShapeHub.cs`替换为以下代码片段。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    上面的代码中扩展客户端添加`Broadcaster`类，该类限制传出消息使用`Timer`从.NET framework 的类。

    由于集线器本身是暂时性 （它在每次创建需要它），`Broadcaster`将被创建为单一实例。 （在.NET 4 中引入） 的延迟初始化用于其创建的延迟，直到它需要时，确保计时器启动之前完全创建第一个中心实例。

    客户端的调用`UpdateShape`函数然后移出中心的`UpdateModel`方法，因此它不再称为立即每当接收传入消息。 相反，将每秒，25 调用的速率发送到客户端的消息由`_broadcastLoop`中的计时器`Broadcaster`类。

    最后，而不是调用客户端方法从中心直接，`Broadcaster`类需要获取对当前正在运行的中心的引用 (`_hubContext`) 使用`GlobalHost`。
2. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。 不会在浏览器中从上一节中明显的差异，但将被阻止到客户端发送的消息数。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>客户端上添加平滑动画

应用程序即将完成，但我们无法使一个详细改善，客户端上的形状的动态移动以响应服务器消息。 而不是将形状的位置设置为给定服务器的新位置，我们将使用 JQuery UI 库`animate`函数用于顺利翻其当前和新位置的形状。

1. 更新客户端的`updateShape`方法看起来像下面突出显示的代码：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    上面的代码将从旧位置形状移到新服务器获得的高于动画间隔 （以这种情况下，100 毫秒） 的过程。 此新动画在开始之前清除在形状上运行任何前一动画。
2. 按 F5 启动应用程序。 复制页面的 URL，并将其粘贴到另一个浏览器窗口。 在浏览器窗口中; 之一中拖动形状在其他浏览器窗口中的形状应将移动。 在其他窗口中的形状的移动应显示为其移动插值通过时间，而不是每个传入消息一次设置不太平稳。

    ![应用程序窗口](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>后续步骤

在本教程中，你已了解如何发送客户端和服务器之间的高频率消息 SignalR 应用程序进行编程。 此通信范例可用于开发联机游戏和其他模拟，如[ShootR 游戏使用 SignalR 创建](http://shootr.signalr.net)。

在本教程中创建的完整应用程序可以从下载[代码库](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。

若要了解有关 SignalR 开发概念的详细信息，请访问以下站点 SignalR 源代码和资源：

- [SignalR 项目](http://signalr.net)
- [SignalR Github 和示例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
