---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "教程： 开始使用 SignalR 1.x |Microsoft 文档"
author: pfletcher
description: "使用 ASP.NET SignalR 生成 HTML 页中的实时聊天应用程序。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a>教程： 开始使用 SignalR 1.x
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)

> 本教程介绍如何使用 SignalR 创建实时聊天应用程序。 你将添加到空的 ASP.NET web 应用程序的 SignalR 和创建 HTML 页以便发送并显示消息。


## <a name="overview"></a>概述

本教程介绍如何构建一个简单的基于浏览器的聊天应用程序通过引入了 SignalR 开发。 将 SignalR 库添加到空的 ASP.NET web 应用程序，创建一个中心类，将消息发送到客户端，并创建 HTML 页，从而让用户发送和接收聊天消息。 有关演示如何在 MVC 4 中创建的聊天应用程序使用的 MVC 视图的类似教程，请参阅[开始使用 SignalR 和 MVC 4](index.md)。

> [!NOTE]
> 本教程使用 SignalR 的版本 (1.x) 版本。 有关 SignalR 之间的更改的详细信息 1.x 和 2.0，请参阅[升级 SignalR 1.x 项目](../releases/upgrading-signalr-1x-projects-to-20.md)。

SignalR 是用于构建 web 应用程序需要实时用户交互或实时数据更新一个开放源代码.NET 库。 示例包括社交应用程序、 多用户游戏、 业务协作和新闻，天气或财务更新应用程序。 这些通常称为实时应用程序。

SignalR 简化了生成实时应用程序的过程。 它包括 ASP.NET server 库和一个要更加轻松地管理客户端-服务器连接并将内容更新推送到客户端的 JavaScript 客户端库。 可以将 SignalR 库添加到现有的 ASP.NET 应用程序，以获取实时功能。

本教程演示了下列 SignalR 开发任务：

- 将 SignalR 库添加到 ASP.NET web 应用程序。
- 创建用于将内容推送到客户端的中心类。
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

本部分演示如何创建空的 ASP.NET web 应用程序，添加 SignalR，并创建聊天应用程序。

先决条件：

- Visual Studio 2010 SP1 或 2012年。 如果未安装 Visual Studio，请参阅[ASP.NET 下载](https://www.asp.net/downloads)若要获取免费 Visual Studio 2012 Express 开发工具。
- [Microsoft ASP.NET 和 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941)。 对于 Visual Studio 2012，此安装程序将添加新的 ASP.NET 功能包括到 Visual Studio 的 SignalR 模板。 对于 Visual Studio 2010 SP1，安装程序不可用，但你可以通过安装 SignalR NuGet 包的安装步骤中所述完成本教程。

以下步骤使用 Visual Studio 2012 创建 ASP.NET 空 Web 应用程序并将添加 SignalR 库：

1. 在 Visual Studio 中创建 ASP.NET 空 Web 应用程序。

    ![创建空 web](tutorial-getting-started-with-signalr/_static/image2.png)
2. 打开**程序包管理器控制台**通过选择**工具 |库包管理器 |程序包管理器控制台**。 控制台窗口中输入以下命令：

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    此命令将安装 SignalR 的最新版本 1.x。
3. 在**解决方案资源管理器**，右键单击项目，选择**添加 |类**。 将新类**ChatHub**。
4. 在**解决方案资源管理器**展开脚本节点。 用于 jQuery 和 SignalR 的脚本库是在项目中可见。

    ![库引用](tutorial-getting-started-with-signalr/_static/image3.png)
5. 中的代码替换**ChatHub**类替换为以下代码。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. 在**解决方案资源管理器**，右键单击项目，然后单击**添加 |新项**。 在**添加新项**对话框中，选择**全局应用程序类**单击**添加**。

    ![添加全局](tutorial-getting-started-with-signalr/_static/image4.png)
7. 添加以下`using`后面提供语句`using`Global.asax.cs 类中的语句。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 添加以下行中的代码`Application_Start`要注册的默认路径为 SignalR 集线器的全局类的方法。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. 在**解决方案资源管理器**，右键单击项目，然后单击**添加 |新项**。 在**添加新项**对话框，选择 Html 页，单击**添加**。
10. 在**解决方案资源管理器**，右键单击你刚刚创建的 HTML 页面，然后单击**设为起始页**。
11. 在 HTML 页中的默认代码替换为以下代码。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **保存所有**项目。

<a id="run"></a>

## <a name="run-the-sample"></a>运行示例

1. 按 F5 以在调试模式下运行该项目。 HTML 页加载中的浏览器实例并提示输入用户名。

    ![输入用户名](tutorial-getting-started-with-signalr/_static/image5.png)
2. 输入用户名称。
3. 从浏览器的地址行中复制 URL，并使用它来打开两个多个浏览器实例。 在每个浏览器实例中，输入唯一的用户名。
4. 在每个浏览器实例中，添加注释并单击**发送**。 注释应该显示在所有浏览器实例。

    > [!NOTE]
    > 此简单的聊天应用程序不维护服务器上的讨论上下文。 中心广播到所有当前用户的注释。 加入这次聊天更高版本的用户将看到消息从时间添加加入。

    下面的屏幕快照显示三个浏览器实例，所有这些更新时将消息发送的一个实例中运行的聊天应用程序：

    ![聊天浏览器](tutorial-getting-started-with-signalr/_static/image6.png)
5. 在**解决方案资源管理器**，检查**脚本文档**运行的应用程序的节点。 没有名为的脚本文件**中心**在运行时动态生成 SignalR 库。 此文件管理 jQuery 脚本和服务器端代码之间的通信。

    ![生成的中心脚本](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>检查代码

SignalR 聊天应用程序演示两个基本的 SignalR 开发任务： 与主协调对象在服务器上，创建一个中心并使用 SignalR jQuery 库来发送和接收消息。

### <a name="signalr-hubs"></a>SignalR 集线器

中的代码示例**ChatHub**类派生自**Microsoft.AspNet.SignalR.Hub**类。 派生自**中心**类是提供有用的方法来构建 SignalR 应用程序。 你可以在您的中心类上创建公共方法，然后通过调用这些方法从 web 页中的 jQuery 脚本中访问这些方法。

在聊天代码中，客户端调用**ChatHub.Send**方法发送一封新邮件。 中心反过来将消息发送到所有客户端，通过调用**Clients.All.broadcastMessage**。

**发送**方法演示了多个中心概念：

- 公共方法声明的中心，使客户端可以调用它们。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**动态属性来访问所有客户端连接到此集线器。
- 客户端上调用 jQuery 函数 (如`broadcastMessage`函数) 来更新客户端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

HTML 页中的代码示例演示如何使用 SignalR jQuery 库与 SignalR hub 进行通信。 在代码中的基本任务声明一个代理，以引用的中心，声明该服务器可以将内容推送到客户端，调用的函数和启动的连接将消息发送到中心。

下面的代码声明一个中心的代理。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> 在 jQuery 到的服务器类及其成员的引用位于 camel 大小写。 引用的代码示例的 C# **ChatHub**类中作为 jQuery **chatHub**。


下面的代码是如何在脚本中创建一个回调函数。 在服务器上的中心类调用此函数可将内容更新推送到每个客户端。 HTML 对内容编码显示之前的两行都是可选的并显示一种简单的方法，以防止脚本注入。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

下面的代码演示如何使用中心打开连接。 代码启动连接，然后将它传递函数来处理 click 事件上**发送**在 HTML 页的按钮。

> [!NOTE]
> 此方法将确保事件处理程序执行之前建立连接。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>后续步骤

你已了解 SignalR 是一个框架，用于构建实时 web 应用程序。 你还了解了多个 SignalR 开发任务： 如何将 SignalR 添加到 ASP.NET 应用程序、 如何创建一个中心类，以及如何发送和从中心接收消息。

你可以使示例应用程序在本教程或其他 SignalR 应用程序可通过 Internet 将它们部署到托管提供商。 Microsoft 提供了可用的 web 宿主中可用的最多 10 个网站[Windows Azure 试用帐户](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 有关如何部署示例 SignalR 应用程序的演练，请参阅[发布 SignalR 入门示例作为 Windows Azure 网站](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx)。 有关如何将 Visual Studio web 项目部署到 Windows Azure 网站的详细信息，请参阅[部署 ASP.NET 应用程序到 Windows Azure 网站](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。 (注意： WebSocket 传输目前不支持为 Windows Azure 网站。 当 WebSocket 传输不可用，SignalR 使用其他可用传输的传输部分中所述[简介 SignalR 主题](index.md)。)

若要了解更多高级的 SignalR 最新开发进展概念，请访问以下站点 SignalR 源代码和资源：

- [SignalR 项目](http://signalr.net)
- [SignalR Github 和示例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
