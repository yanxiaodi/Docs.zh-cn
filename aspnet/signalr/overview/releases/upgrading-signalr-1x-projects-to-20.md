---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "将 SignalR 1.x 项目升级到版本 2 |Microsoft 文档"
author: pfletcher
description: "本主题介绍如何将现有 SignalR 1.x 项目升级到 SignalR 2.x，以及如何解决在升级过程中可能出现的问题..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>将 SignalR 1.x 项目升级到版本 2
====================
通过[Patrick Fletcher](https://github.com/pfletcher)

> 本主题介绍如何将现有 SignalR 1.x 项目升级到 SignalR 2.x，以及如何解决在升级过程中可能出现的问题。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 版本 1 和 2
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


SignalR 2 提供一种一致的开发体验跨服务器平台使用[OWIN](http://owin.org)。 本文介绍几个需要更新到版本 2 提供 SignalR 1.x 应用程序的步骤。

尽管它鼓励升级应用程序迁移至 SignalR 2，SignalR 1.x 仍支持。

本教程介绍如何升级到 SignalR 2 web 承载的应用程序。 下 SignalR 2 中，现在支持自承载应用程序 （那些承载控制台应用程序、 Windows 服务或其他进程中的服务器）。 有关如何开始使用 SignalR 2 创建自承载的应用程序的信息，请参阅[教程： 自承载的 SignalR](../deployment/tutorial-signalr-self-host.md)。

## <a name="contents"></a>内容

下列各节描述升级 SignalR 项目，以及如何解决可能出现的问题所涉及的任务。

- [示例： 升级到 SignalR 2 入门教程](#example)
- [在升级过程中遇到的错误故障排除](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>示例： 升级入门教程应用程序为 SignalR 2

在此部分中，你将更新应用程序中创建[SignalR 1.x 版入门教程](../older-versions/index.md)使用 SignalR 2。

1. 完成入门教程后，右键单击项目，然后选择**属性**。 验证**目标框架**设置为**.NET Framework 4.5。**
2. 打开程序包管理器控制台。 删除 SignalR 1.x 从项目使用以下命令：

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. 安装 SignalR 2 使用以下命令：

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. 在 HTML 页中，更新 SignalR 以匹配现在包含项目中的脚本的版本的脚本引用。

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. 在全局应用程序类中，删除对 MapHubs 的调用。

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. 右键单击该解决方案，然后选择**添加**，**新建项...**.在对话框中，选择**Owin Startup 类**。 将新类**Startup.cs**。

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. 将 Startup.cs 的内容替换为以下代码：

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    程序集特性将类添加到 Owin 的启动过程中，后者执行`Configuration`Owin 启动时的方法。 这反过来调用`MapSignalR`方法，该应用程序中创建路由所有 SignalR 集线器的方法。
8. 运行该项目，并将主页的 URL 复制到另一个浏览器或浏览器窗格中，前面所述。 每个页面将要求提供用户名，并从每个页面发送的消息应在这两个浏览器窗格中可见。

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>在升级过程中遇到的错误故障排除

本部分介绍在升级过程中可能出现的问题。 有关的错误和 SignalR 应用程序，可能导致的问题的更完整列表，请参阅[SignalR 疑难解答](../testing-and-debugging/troubleshooting.md)。

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>调用之间不明确的以下方法或属性

如果对的引用，将出现此错误`Microsoft.AspNet.SignalR.Owin`不会删除。 此包已弃用;必须删除的引用，必须卸载 SelfHost 包的 1.x 版。

### <a name="hub-methods-fail-silently"></a>中心的方法以静默方式失败

验证你的客户端中的脚本引用都由日期，并且决定`OwinStartup`属性 Startup 类具有正确的类和程序集名称为你的项目。 此外，请试着打开你的浏览器; 中的中心地址 （/signalr/中心）出现任何错误中将提供有关了什么问题的详细信息。
