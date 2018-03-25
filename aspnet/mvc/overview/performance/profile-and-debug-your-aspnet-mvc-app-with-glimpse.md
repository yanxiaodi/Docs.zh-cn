---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "配置文件和调试使用 Glimpse 对 ASP.NET MVC 应用程序 |Microsoft 文档"
author: Rick-Anderson
description: "Glimpse 是一个获得成功和增长的开放源 NuGet 包的系列，提供详细的性能、 调试和诊断信息的 ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 9cfdced21251b482ca527dda9c3a698de77cc8ca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>配置文件和调试使用 Glimpse 对 ASP.NET MVC 应用程序
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse 是获得成功和增长的开放源 NuGet 包的系列，提供详细的性能、 调试和诊断信息的 ASP.NET 应用程序。 它是普通安装、 轻量、 超快速，并在每一页的底部显示关键性能指标。 它允许你向下钻取到你的应用时需要先找出在服务器上正在运行的内容。 Glimpse 提供更重要的信息，我们建议你在整个开发周期中，包括你的 Azure 测试环境使用。 虽然[Fiddler](http://www.telerik.com/fiddler)和[F-12 开发工具](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)提供客户端视图，Glimpse 提供从服务器的详细的视图。 本教程将着重介绍如何使用 Glimpse ASP.NET MVC 和 EF 包，但其他许多包都可用。 尽可能将链接到相应[转让文档](http://getglimpse.com/Docs/)其中我要帮助维护。 Glimpse 是一个开源项目，你太可参与源代码和文档。


- [安装 Glimpse](#ig)
- [为本地主机启用 Glimpse](#eg)
- [时间线选项卡](#Time)
- [模型绑定](#mb)
- [路由](#route)
- [在 Azure 上使用 Glimpse](#da)
- [其他资源](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>安装 Glimpse

你可以从 NuGet 包管理器控制台或从安装 Glimpse**管理 NuGet 包**控制台。 对于本演示中，我将的 Mvc5 和 ef6 更高版本的包进行安装：

![从 NuGet Dlg 安装 Glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

搜索*Glimpse.EF*

![从 NuGet 安装 dlg Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

通过选择**安装包**，你可以查看安装的 Glimpse 依赖模块：

![从 DLg 安装 Glimpse 包](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

以下命令从包管理器控制台安装 Glimpse MVC5 和从 EF6 模块：

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>为本地主机启用 Glimpse

导航到 http://localhost:&lt;端口号&gt;/glimpse.axd 和单击**打开 Glimpse 上**按钮。

![Glimpse axd 页](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

如果您有收藏夹栏显示，可以拖放 Glimpse 按钮，将其添加为 bookmarklets:

![与 Glimpse boookmarklets 的 IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

你现在可以导航到你的应用，和**磁头向上显示**(HUD) 显示在页面底部。

![HUD 联系人管理器页](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse HUD 页](http://getglimpse.com/Docs/Heads-up-Display)详细说明了上面所示的计时信息。 非介入式性能数据 HUD 显示可以通知你的问题立即-之前到测试周期，将出现。 单击&quot;g&quot;右下角中会显示 Glimpse 面板：

![Glimpse 面板](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

在上图中，[执行选项卡](http://getglimpse.com/Docs/Execution-Tab)选择时，它显示在管道中的操作和筛选器的计时详细信息。 你可以看到我[停止监视筛选器计时器](http://www.nuget.org/packages/StopWatch/)开始管道的阶段 6。 虽然我轻型计时器都可以提供有用的配置文件/计时数据，则它未命中所用的授权和呈现的视图的所有时间。 你可以阅读我计时器在[配置文件和时间 ASP.NET MVC 应用程序向 Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)。 [选项卡](http://getglimpse.com/Docs/Tabs)页提供了每个选项卡的详细信息的链接。

<a id="Time"></a>
## <a name="the-timeline-tab"></a>时间线选项卡

我已修改 Tom Dykstra 的未完成[EF 6/MVC 5 教程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)替换为以下代码将更改为教师控制器：

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

上面的代码中允许我在查询字符串中传递 (`eager`) 控制 eager 或显式加载的数据。 在下图中，使用显式加载和计时页显示中加载每个注册`Index`操作方法：

![Explicit Loading — 显式加载](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

在下面的代码中，指定 eager，并且每个注册提取后`Index`视图称为：

![指定 eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

你可以将鼠标悬停在某个时间段，可获取详细的计时信息：

![悬停鼠标以查看详细的计时](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>模型绑定

[模型绑定选项卡](http://getglimpse.com/Docs/Model-Binding-Tab)提供丰富的信息来帮助你了解为何某些未被绑定如所料，窗体变量的绑定方式。 下面的图像显示**？** 图标，你可以单击以打开该功能的 glimpse 帮助页。

![glimpse 模型绑定视图](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>路由

 Glimpse 路由选项卡将可以帮助你调试和了解路由。 在下图中，将选择的产品路由 （以及它会显示为绿色，Glimpse 约定）。 ![所选的产品名称](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)路由约束、 区域和数据令牌也会显示。 请参阅[Glimpse 路由](http://getglimpse.com/Docs/Routes-Tab)和[ASP.NET MVC 5 中的属性路由](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)有关详细信息。 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>在 Azure 上使用 Glimpse

Glimpse 默认安全策略只允许 Glimpse 数据显示本地主机。 你可以更改此安全策略，以便你可以查看此数据 （例如，在 Azure 上 web 应用程序） 的远程服务器上。 在 Azure 上的测试环境，添加到底部的突出显示的标记*web.confg*文件以启用 Glimpse:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

进行单独此更改后，任何用户都可以在远程站点上看到你 Glimpse 的数据。 考虑将上面的标记添加到发布配置文件，以便它仅部署应用时使用该发布配置文件 (例如，你 Azure 测试 proifle。)若要限制 Glimpse 数据，我们将添加`canViewGlimpseData`角色，并仅允许此角色来查看 Glimpse 数据中的用户。

删除从注释*GlimpseSecurityPolicy.cs*文件并将更改[IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)从调用`Administrator`到`canViewGlimpseData`角色：

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> 安全-Glimpse 提供的丰富数据无法公开您的应用程序的安全性。 Microsoft 不具有对生产应用程序用于执行 Glimpse 安全审核。


有关添加角色的信息，请参阅我[将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 5 web 应用程序部署到 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)教程。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他资源

- [将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 5 应用程序部署到 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [转让配置](http://getglimpse.com/Docs/Configuration)-配置选项卡、 运行时策略、 日志记录和的详细信息的文档页。
