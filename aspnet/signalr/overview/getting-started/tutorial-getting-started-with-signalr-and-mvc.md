---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "教程： 开始使用 SignalR 2 和 MVC 5 |Microsoft 文档"
author: pfletcher
description: "本教程演示如何使用 ASP.NET SignalR 2 创建实时聊天应用程序。 将 MVC 5 应用程序中添加 SignalR 和创建聊天视图..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>教程： 开始使用 SignalR 2 和 MVC 5
====================
通过[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)

[下载已完成的项目](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> 本教程演示如何使用 ASP.NET SignalR 2 创建实时聊天应用程序。 你将添加到 MVC 5 应用程序的 SignalR 和创建聊天视图来发送和显示的消息。 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

本教程向你介绍使用 ASP.NET SignalR 2 和 ASP.NET MVC 5 的实时 web 应用程序开发。 本教程使用的相同聊天应用程序代码[SignalR 入门教程](tutorial-getting-started-with-signalr.md)，但演示如何将其添加到一个 MVC 5 应用程序。

本主题中，你将学习以下 SignalR 开发任务：

- 将 SignalR 库添加到一个 MVC 5 应用程序。
- 创建中心和 OWIN 启动类，以将内容推送到客户端。
- 使用 SignalR jQuery 库在网页上发送消息并显示从中心的更新。

下面的屏幕截图显示在浏览器中运行的已完成的聊天应用程序。

![聊天实例](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

各节：

- [将项目设置](#setup)
- [运行示例](#run)
- [检查代码](#code)
- [后续步骤](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>将项目设置

先决条件：

- Visual Studio 2013. 如果未安装 Visual Studio，请参阅[ASP.NET 下载](https://www.asp.net/downloads)若要获取免费 Visual Studio 2013 Express 开发工具。

本部分演示如何创建一个 ASP.NET MVC 5 应用程序、 添加 SignalR 库，并创建聊天应用程序。

1. 在 Visual Studio 中，创建面向.NET Framework 4.5 的 C# ASP.NET 应用程序，将其命名为 SignalRChat，，单击确定。

    ![创建 web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. 在`New ASP.NET Project`对话框中，然后选择**MVC**，然后单击**更改身份验证**。

    ![创建 web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. 选择**无身份验证**中**更改身份验证**对话框，然后单击**确定**。

    ![选择无身份验证](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > 如果你选择不同的身份验证提供程序用于你的应用程序，`Startup.cs`类将为你创建; 你不需要创建你自己`Startup.cs`下面的步骤 10 中的类。
4. 单击**确定**中**新建 ASP.NET 项目**对话框。
5. 打开**工具 |库包管理器 |程序包管理器控制台**并运行以下命令。 此步骤将一组的脚本文件和启用 SignalR 功能的程序集引用添加到项目中。

    `install-package Microsoft.AspNet.SignalR`
6. 在**解决方案资源管理器**，展开脚本文件夹。 请注意，适用于 SignalR 的脚本库已添加到项目。

    ![脚本文件夹](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. 在**解决方案资源管理器**，右键单击项目，选择**添加 |新文件夹**，并添加一个名为的新文件夹**中心**。
8. 右键单击**中心**文件夹中，单击**添加 |新项**，选择**Visual C# |Web |SignalR**中的节点**已安装**窗格中，选择**SignalR Hub Class (v2)**从中间窗格中，并创建名为的新集线器**ChatHub.cs**。 你将使用此类作为将消息发送到所有客户端的 SignalR 服务器集线器。

    ![创建新的中心](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. 中的代码替换**ChatHub**类替换为以下代码。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. 创建一个名 Startup.cs 的新类。 更改为以下文件的内容。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. 编辑`HomeController`中找到该类**Controllers/HomeController.cs**并将以下方法添加到类。 此方法返回**聊天**将在稍后的步骤中创建的视图。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. 右键单击**视图/主页**文件夹，并选择**添加.|视图**。
13. 在**添加视图**对话框中，名称的新视图**聊天**。

    ![添加视图](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. 内容替换**Chat.cshtml**替换为以下代码。

    > [!IMPORTANT]
    > 当你将 SignalR 和其他脚本库添加到 Visual Studio 项目中时，包管理器可能会安装本主题中显示的版本比更新 SignalR 脚本文件的版本。 请确保你的代码中的脚本引用匹配在项目中安装的脚本库的版本。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **保存所有**项目。

<a id="run"></a>

## <a name="run-the-sample"></a>运行示例

1. 按 F5 以在调试模式下运行该项目。
2. 在浏览器地址行中，追加**/home/聊天**项目的默认页的 url。 聊天页面加载的浏览器实例并提示输入用户名。

    ![输入用户名](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. 输入用户名称。
4. 从浏览器的地址行中复制 URL，并使用它来打开两个多个浏览器实例。 在每个浏览器实例中，输入唯一的用户名。
5. 在每个浏览器实例中，添加注释并单击**发送**。 注释应该显示在所有浏览器实例。

    > [!NOTE]
    > 此简单的聊天应用程序不维护服务器上的讨论上下文。 中心广播到所有当前用户的注释。 加入这次聊天更高版本的用户将看到消息从时间添加加入。
6. 下面的屏幕截图显示在浏览器中运行的聊天应用程序。

    ![聊天浏览器](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. 在**解决方案资源管理器**，检查**脚本文档**运行的应用程序的节点。 如果你使用的 Internet Explorer 作为你的浏览器，此节点会显示在调试模式下。 没有名为的脚本文件**中心**在运行时动态生成 SignalR 库。 此文件管理 jQuery 脚本和服务器端代码之间的通信。 如果你使用非 Internet Explorer 浏览器，你还可以访问动态**中心**通过浏览到它直接，例如 http://mywebsite/signalr/hubs 的文件。

<a id="code"></a>

## <a name="examine-the-code"></a>检查代码

SignalR 聊天应用程序演示两个基本的 SignalR 开发任务： 与主协调对象在服务器上，创建一个中心并使用 SignalR jQuery 库来发送和接收消息。

### <a name="signalr-hubs"></a>SignalR 集线器

中的代码示例**ChatHub**类派生自**Microsoft.AspNet.SignalR.Hub**类。 派生自**中心**类是提供有用的方法来构建 SignalR 应用程序。 你可以在你的中心类上创建公共方法，然后通过调用这些方法从 web 页中的脚本中访问这些方法。

在聊天代码中，客户端调用**ChatHub.Send**方法发送一封新邮件。 中心反过来将消息发送到所有客户端，通过调用**Clients.All.addNewMessageToPage**。

**发送**方法演示了多个中心概念：

- 公共方法声明的中心，使客户端可以调用它们。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**属性来访问所有客户端连接到此集线器。
- 客户端上调用的函数 (如`addNewMessageToPage`函数) 来更新客户端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

**Chat.cshtml**视图文件中的代码示例演示如何使用 SignalR jQuery 库与 SignalR hub 进行通信。 在代码中的基本任务创建时自动生成的代理的中心，声明该服务器可以将内容推送到客户端，调用的函数和启动连接将消息发送到中心的引用。

下面的代码声明对中心代理的引用。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> 在 JavaScript 到的服务器类及其成员的引用位于 camel 大小写。 引用的代码示例的 C# **ChatHub**类中作为**chatHub**。 如果你想要引用`ChatHub`类在 jQuery 与传统 Pascal 大小写就像在 C# 中，编辑 ChatHub.cs 类文件。 添加`using`语句来引用`Microsoft.AspNet.SignalR.Hubs`命名空间。 然后添加`HubName`属性设为`ChatHub`类，例如`[HubName("ChatHub")]`。 最后，更新对你 jQuery 引用`ChatHub`类。


下面的代码演示如何在脚本中创建的回调函数。 在服务器上的中心类调用此函数可将内容更新推送到每个客户端。 可选调用`htmlEncode`函数显示了一个 html 的方式显示在页中，作为一种方法，以防止脚本注入前对进行编码的消息内容。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

下面的代码演示如何使用中心打开连接。 代码启动连接，然后将它传递函数来处理 click 事件上**发送**聊天页中的按钮。

> [!NOTE]
> 此方法确保事件处理程序执行之前建立连接。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>后续步骤

你已了解 SignalR 是一个框架，用于构建实时 web 应用程序。 你还了解了多个 SignalR 开发任务： 如何将 SignalR 添加到 ASP.NET 应用程序、 如何创建一个中心类，以及如何发送和从中心接收消息。

有关如何部署到 Azure 的示例 SignalR 应用程序的演练，请参阅[将 signalr 与在 Azure App Service Web Apps 配合](../deployment/using-signalr-with-azure-web-sites.md)。 有关如何将 Visual Studio web 项目部署到 Windows Azure 网站的详细信息，请参阅[在 Azure App Service 中创建 ASP.NET web 应用](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

若要了解更多高级的 SignalR 最新开发进展概念，请访问以下站点 SignalR 源代码和资源：

- [SignalR 项目](http://signalr.net)
- [SignalR Github 和示例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
