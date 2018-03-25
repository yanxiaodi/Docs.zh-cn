---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: "教程： 开始使用 SignalR 2 |Microsoft 文档"
author: pfletcher
description: "本教程介绍如何使用 SignalR 创建实时聊天应用程序。 将 SignalR 添加空的 ASP.NET web 应用程序，并创建 HTML pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a>教程： 开始使用 SignalR 2
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

[下载已完成的项目](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> 本教程介绍如何使用 SignalR 创建实时聊天应用程序。 你将添加到空的 ASP.NET web 应用程序的 SignalR 和创建 HTML 页以便发送并显示消息。 
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

本教程介绍如何构建一个简单的基于浏览器的聊天应用程序通过引入了 SignalR 开发。 将 SignalR 库添加到空的 ASP.NET web 应用程序，创建一个中心类，将消息发送到客户端，并创建 HTML 页，从而让用户发送和接收聊天消息。 有关演示如何在 MVC 5 中创建的聊天应用程序使用的 MVC 视图的类似教程，请参阅[SignalR 2 和 MVC 5 入门](tutorial-getting-started-with-signalr-and-mvc.md)。

> [!NOTE]
> 本教程演示如何在版本 2 中创建 SignalR 应用程序。 有关 SignalR 之间的更改的详细信息 1.x 和 2，请参阅[升级 SignalR 1.x 项目](../releases/upgrading-signalr-1x-projects-to-20.md)和[Visual Studio 2013 发行说明](../../../visual-studio/overview/2013/release-notes.md#TOC13)。

SignalR 是用于构建 web 应用程序需要实时用户交互或实时数据更新一个开放源代码.NET 库。 示例包括社交应用程序、 多用户游戏、 业务协作和新闻，天气或财务更新应用程序。 这些通常称为实时应用程序。

SignalR 简化了生成实时应用程序的过程。 它包括 ASP.NET server 库和一个要更加轻松地管理客户端-服务器连接并将内容更新推送到客户端的 JavaScript 客户端库。 可以将 SignalR 库添加到现有的 ASP.NET 应用程序，以获取实时功能。

本教程演示了下列 SignalR 开发任务：

- 将 SignalR 库添加到 ASP.NET web 应用程序。
- 创建用于将内容推送到客户端的中心类。
- 创建配置该应用程序的 OWIN 启动类。
- 使用 SignalR jQuery 库在网页上发送消息并显示从中心的更新。

下面的屏幕截图显示在浏览器中运行的聊天应用程序。 每个新用户可以发表评论，并查看添加用户加入聊天后的注释。

![聊天实例](tutorial-getting-started-with-signalr/_static/image1.png)

各节：

- [将项目设置](#setup)
- [运行示例](#run)
- [检查代码](#code)
- [后续步骤](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>将项目设置

本部分演示如何使用 Visual Studio 2013 和 SignalR 版本 2 创建空的 ASP.NET web 应用程序，添加 SignalR，并创建聊天应用程序。

先决条件：

- Visual Studio 2013. 如果未安装 Visual Studio，请参阅[ASP.NET 下载](https://www.asp.net/downloads)若要获取免费 Visual Studio 2013 Express 开发工具。

以下步骤使用 Visual Studio 2013 创建 ASP.NET 空 Web 应用程序并添加 SignalR 库：

1. 在 Visual Studio 中，创建 ASP.NET Web 应用程序。

    ![创建 web](tutorial-getting-started-with-signalr/_static/image2.png)
2. 在**新建 ASP.NET 项目**窗口中，保留**空**选择和单击**创建项目**。

    ![创建空 web](tutorial-getting-started-with-signalr/_static/image3.png)
3. 在**解决方案资源管理器**，右键单击项目，选择**添加 |SignalR Hub 类 (v2)**。 将类**ChatHub.cs**并将其添加到项目。 此步骤将创建**ChatHub**类并将一组的脚本文件和支持 SignalR 的程序集引用添加到项目。

    > [!NOTE]
    > 你还可以将 SignalR 通过打开添加到项目**工具 |库包管理器 |程序包管理器控制台**并运行命令：

    `install-package Microsoft.AspNet.SignalR`

    如果你使用控制台添加 SignalR，单独的步骤创建 SignalR hub 类后添加 SignalR。

    > [!NOTE]
    > 如果你正在使用 Visual Studio 2012， **SignalR Hub Class (v2)**模板将不可用。 你可以添加普通**类**调用`ChatHub`相反。
4. 在**解决方案资源管理器**，展开脚本节点。 用于 jQuery 和 SignalR 的脚本库是在项目中可见。
5. 将在新代码**ChatHub**类替换为以下代码。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. 在**解决方案资源管理器**，右键单击项目，然后单击**添加 |OWIN Startup 类**。 将新类`Startup`并单击确定。

    > [!NOTE]
    > 如果你正在使用 Visual Studio 2012， **OWIN Startup 类**模板将不可用。 你可以添加普通**类**调用`Startup`相反。
7. 将新的启动类的内容更改为以下。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 在**解决方案资源管理器**，右键单击项目，然后单击**添加 |HTML 页**。 将新该页命名为`index.html`。
    >[!NOTE]
    >你可能需要更改的对 JQuery 和 SignalR 的库的引用的版本号
9. 在**解决方案资源管理器**，右键单击你刚刚创建的 HTML 页面，然后单击**设为起始页**。
10. 在 HTML 页中的默认代码替换为以下代码。

    > [!NOTE]
    > 可能的包管理器安装 SignalR 脚本的更高版本。 验证以下脚本参考对应于在项目中 （它们将位于不同，如果你添加了 SignalR 使用 NuGet，而不是添加 hub。） 的脚本文件的版本

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **保存所有**项目。

<a id="run"></a>

## <a name="run-the-sample"></a>运行示例

1. 按 F5 以在调试模式下运行该项目。 HTML 页加载中的浏览器实例并提示输入用户名。

    ![输入用户名](tutorial-getting-started-with-signalr/_static/image4.png)
2. 输入用户名称。
3. 从浏览器的地址行中复制 URL，并使用它来打开两个多个浏览器实例。 在每个浏览器实例中，输入唯一的用户名。
4. 在每个浏览器实例中，添加注释并单击**发送**。 注释应该显示在所有浏览器实例。

    > [!NOTE]
    > 此简单的聊天应用程序不维护服务器上的讨论上下文。 中心广播到所有当前用户的注释。 加入这次聊天更高版本的用户将看到消息从时间添加加入。

    下面的屏幕快照显示三个浏览器实例，所有这些更新时将消息发送的一个实例中运行的聊天应用程序：

    ![聊天浏览器](tutorial-getting-started-with-signalr/_static/image5.png)
5. 在**解决方案资源管理器**，检查**脚本文档**运行的应用程序的节点。 没有名为的脚本文件**中心**在运行时动态生成 SignalR 库。 此文件管理 jQuery 脚本和服务器端代码之间的通信。

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>检查代码

SignalR 聊天应用程序演示两个基本的 SignalR 开发任务： 与主协调对象在服务器上，创建一个中心并使用 SignalR jQuery 库来发送和接收消息。

### <a name="signalr-hubs"></a>SignalR 集线器

中的代码示例**ChatHub**类派生自**Microsoft.AspNet.SignalR.Hub**类。 派生自**中心**类是提供有用的方法来构建 SignalR 应用程序。 你可以在你的中心类上创建公共方法，然后通过调用这些方法从 web 页中的脚本中访问这些方法。

在聊天代码中，客户端调用**ChatHub.Send**方法发送一封新邮件。 中心反过来将消息发送到所有客户端，通过调用**Clients.All.broadcastMessage**。

**发送**方法演示了多个中心概念：

- 公共方法声明的中心，使客户端可以调用它们。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**动态属性来访问所有客户端连接到此集线器。
- 客户端上调用的函数 (如`broadcastMessage`函数) 来更新客户端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

HTML 页中的代码示例演示如何使用 SignalR jQuery 库与 SignalR hub 进行通信。 在代码中的基本任务声明一个代理，以引用的中心，声明该服务器可以将内容推送到客户端，调用的函数和启动的连接将消息发送到中心。

下面的代码声明对中心代理的引用。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> 在 JavaScript 到的服务器类及其成员的引用位于 camel 大小写。 引用的代码示例的 C# **ChatHub**类中作为**chatHub**。


下面的代码是如何在脚本中创建一个回调函数。 在服务器上的中心类调用此函数可将内容更新推送到每个客户端。 HTML 对内容编码显示之前的两行都是可选的并显示一种简单的方法，以防止脚本注入。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

下面的代码演示如何使用中心打开连接。 代码启动连接，然后将它传递函数来处理 click 事件上**发送**在 HTML 页的按钮。

> [!NOTE]
> 此方法确保事件处理程序执行之前建立连接。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>后续步骤

你已了解 SignalR 是一个框架，用于构建实时 web 应用程序。 你还了解了多个 SignalR 开发任务： 如何将 SignalR 添加到 ASP.NET 应用程序、 如何创建一个中心类，以及如何发送和从中心接收消息。

有关如何部署到 Azure 的示例 SignalR 应用程序的演练，请参阅[将 signalr 与在 Azure App Service Web Apps 配合](../deployment/using-signalr-with-azure-web-sites.md)。 有关如何将 Visual Studio web 项目部署到 Windows Azure 网站的详细信息，请参阅[在 Azure App Service 中创建 ASP.NET web 应用](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

若要了解更多高级的 SignalR 最新开发进展概念，请访问以下站点 SignalR 源代码和资源：

- [SignalR 项目](http://signalr.net)
- [SignalR Github 和示例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
