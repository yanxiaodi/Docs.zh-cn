---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: "什么是 ASP.NET MVC 5.2 中的新增功能 |Microsoft 文档"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a>什么是 ASP.NET MVC 5.2 中的新增功能
====================
通过[Microsoft](https://github.com/microsoft)

本主题介绍什么是新的 ASP.NET MVC 5.2 [Microsoft.AspNet.MVC 5.2.2](#52)和[ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [软件要求](#softRequire)
- [下载](#download)
- [文档](#documentation)
- [ASP.NET MVC 5.2 中的新增功能](#new-features)

    - [属性路由改进](#attributerouting)
- [已知的问题和重大更改](#knownbreakingchanges)
- [Bug 修复](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>软件要求

- Visual Studio 2012： 下载[ASP.NET 和 Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)。
- Visual Studio 2013： 下载[Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)或更高版本。 编辑 ASP.NET MVC 5.2 Razor 视图需要此更新。

<a id="download"></a>
## <a name="download"></a>下载

运行时功能将发布为 NuGet 库上的 NuGet 包。 所有运行时包按照[语义版本控制](http://semver.org/)规范。 最新的 ASP.NET MVC 5.2 包具有以下版本:"5.2.0"。 你可以安装或更新这些包通过[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。 版本还包括在 NuGet 上的相应本地化的包。

你可以安装或使用 NuGet 程序包管理器控制台更新到已发布的 NuGet 程序包：

安装包 Microsoft.AspNet.Mvc-Version 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>文档

教程和有关 ASP.NET MVC 5.2 的其他信息可以从 ASP.NET web 站点 ([https://www.asp.net/mvc](../../index.md))。

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>ASP.NET MVC 5.2 中的新增功能

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>属性路由改进

现在的属性路由提供调用 IDirectRouteProvider，允许完全控制属性路由如何发现和配置一个扩展点。 IDirectRouteProvider 负责提供的操作和以及关联的路由信息以指定哪些路由配置所需的这些操作完全的控制器的列表。 调用 MapAttributes/MapHttpAttributeRoutes 时，可能指定 IDirectRouteProvider 实现。

自定义 IDirectRouteProvider 是最简单的扩展了默认实现，DefaultDirectRouteProvider。 此类提供单独可重写虚拟方法，若要更改用于发现属性、 创建的路由项和发现的路由前缀和区域前缀的逻辑。

使用 IDirectRouteProvider 的新属性路由扩展性，用户无法执行以下操作：

1. 支持属性路由的继承。 例如，在以下方案博客和存储控制器中使用由 BaseController 定义的属性路由约定。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. 自动生成属性路由的路由的名称。 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. 先获取将路由添加到路由表，请修改在一个中心位置中的路由前缀。
4. 筛选出你想要查找的属性路由的控制器。 我们希望推出 3 和 4 上的博客。

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook 修补已更改的 API 图面

MVC Facebook 包[已断开](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)由于 Facebook 所做的一些 API 更改。 我们即将发布新的 Facebook 包 (Microsoft.AspNet.Facebook 1.0.0) 来解决这些问题。

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>到 5.2.0 与项目的基架 MVC/Web API 包导致 5.1.2 打包以一种是在项目中尚不存在

有关 ASP.NET MVC 5.2.0 更新 NuGet 程序包不会更新 Visual Studio 工具，例如 ASP.NET 基架或 ASP.NET Web 应用程序项目模板。 它们使用以前版本的 ASP.NET 运行时包 (例如 5.1.2 Update 2 中)。 因此，ASP.NET 基架将安装所需的包的以前版本 (例如 5.1.2 Update 2 中)，如果它们尚不存在在你项目中可用。 但是，在 Visual Studio 2013 RTM 或 Update 1 中的 ASP.NET 基架不会覆盖你的项目中的最新包。 如果您使用 ASP.NET Web API 2.2 或 ASP.NET MVC 5.2 到更新你的项目的包之后的基架，请确保 Web API 和 ASP.NET MVC 的版本都一致。

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Microsoft.jQuery.Unobtrusive.Validation NuGet 软件包安装失败，因为它是找不到 Microsoft.jQuery.Unobtrusive.Validation 的版本兼容 jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation 需要 jQuery &gt;= 1.8 和 jQuery.Validation &gt;= 1.8。 But,jQuery.Validation (1.8) 需要 jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6)。 因此，NuGet 安装一次的 JQuery 1.8 和 jQuery.Validation 1.8 时就会失败。 当你看到此问题时，你可以只需更新的版本到 jQuery.Validation &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)具有固定的 jQuery cap 首先，你应能够安装Microsoft.jQuery.Unobtrusive.Validation。

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery。验证 nuget 程序包版本 1.13.0 无法识别某些国际电子邮件地址

jQuery.Validation nuget 程序包版本 1.11.1 是识别以下有效的电子邮件地址的最后一个已知的版本。 任何更高版本可能不能识别它们。 例如: 

电子邮件地址国际化 (EAI) 标准 (例如， [&#29992; &#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized 资源标识符 (IRIs) (例如。， [（& a) #29992; &#25143; @&#1076; （& a) #1086; （& a) #1084; &#1077; （& a) #1085;。&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

在报告问题[https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Visual Studio 2013 中的 Razor 视图的语法突出显示

如果你更新到 ASP.NET MVC 5.2 而无需更新 Visual Studio 2013，你将获取 Visual Studio 编辑器支持语法突出显示编辑 Razor 视图时。 你将需要更新 Visual Studio 2013，若要获得此支持。

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Bug 修复和细微的功能更新

此版本还包括几个 bug 修复和细微的功能更新。 您可以找到完整的列表：

- [5.2 包](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

此版本不具有任何新功能或 bug 修复的 MVC。 我们进行了[Web 页中更改](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)显著的性能改进，并且随后更新我们拥有取决于此新版本的网页的所有其他从属包。

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

你可以阅读有关版本[此处](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。 此版本包含仅 bug 修复。 你可以使用[此查询](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修复的问题的列表。
