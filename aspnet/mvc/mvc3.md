---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: "(包括 2011 年 4 月工具更新)ASP.NET MVC 3 是一个框架，用于构建使用成熟设计模式的可缩放的、 基于标准的 web 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: c7eee987b28a5d7f8b40fe89a7bf7517ec06646f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(包括 2011 年 4 月工具更新)*
> 
> ASP.NET MVC 3 是一个框架，用于构建可缩放的、 基于标准的 web 应用程序使用成熟设计模式以及 ASP.NET 和.NET Framework 的能力。
> 
> ASP.NET MVC 2，以便开始使用它现在安装并排显示 ！
> 
> 下载[此处安装程序](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>靠前的特征

- 通过 NuGet 可扩展的集成基架系统
- HTML 5 启用的项目模板
- 包括新的 Razor 视图引擎的表达视图
- 与依赖关系注入和全局操作筛选器的功能强大挂钩
- 丰富的 JavaScript 支持非介入式 JavaScript、 jQuery 验证，以及 JSON 绑定
- *读取完整功能列表[下面](#overview)*

## <a name="top-links"></a>顶部的链接

什么是 ASP.NET MVC 3 中的新增功能

- Phil Haack: [ASP.NET MVC 3 发布](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.NET MVC3、 WebMatrix、 NuGet、 IIS Express 和 Orchard 发布的上下文中的 Microsoft 年 1 月 Web 版本](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie:[宣布的 ASP.NET MVC 3、 IIS Express、 SQL CE 4、 Web 场框架、 Orchard、 WebMatrix 发布](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [ASP.NET MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

安装和帮助

- 安装 ASP.NET MVC 3 使用[Web 平台安装程序 （推荐）](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- 安装 ASP.NET MVC 3 使用[可执行安装程序](https://go.microsoft.com/fwlink/?LinkID=208140)
- 安装[Visual Studio 11 开发者预览版的 ASP.NET MVC 3](https://go.microsoft.com/fwlink/?LinkID=208140)
- 读取[简介 ASP.NET MVC 3 教程](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- 获取帮助和讨论中的 ASP.NET MVC 3[论坛](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>ASP.NET MVC 3 概述

ASP.NET MVC 3 基于 ASP.NET MVC 1 和 2，添加两者都简化你的代码，让更深入地扩展性的强大功能。 本主题提供许多新功能包含在此版本中，已组织到以下各节的概述：

- [MvcScaffold 集成可扩展基架](#BM_MvcScaffolding)
- [HTML 5 启用的项目模板](#BM_HTML5)
- [Razor 视图引擎](#BM_TheRazorViewEngine)
- [对多个视图引擎的支持](#BM_Support_for_Multiple_View_Engines)
- [控制器改进](#BM_Controller_Improvements)
- [JavaScript 和 Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [模型验证的改进](#BM_Model_Validation_Improvements)
- [依赖关系注入改进](#BM_Dependency_Injection_Improvements)
- [其他新功能](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>MvcScaffold 集成可扩展基架

新的基架系统更加简单可以选取并开始高效使用，如果你是全新的 framework，并可自动执行常见的开发任务，如果你有经验和已知道你要做什么。

这支持新的 NuGet*基架*包调用**MvcScaffolding**。 术语"基架"由许多软件技术用于意味着"快速生成的基本概述的软件，你可以然后编辑和自定义"。 我们正在创建的 ASP.NET MVC 基架包是极大地有益在几种场景：

- **如果你首次了解 ASP.NET MVC**，因为它可用于获取一些有用的工作代码，，然后可以编辑，并根据需要调整的快捷方法。 它将保存你的看一看空白页，并且不知道从何处着手缩短从 ！
- **如果您非常熟悉 ASP.NET MVC，而且现在浏览某些新的外接程序技术**的对象关系映射器、 视图引擎、 测试的库，如等，因为该技术的创建者可能还创建了基架程序包它。
- **如果涉及到你的工作，重复创建相似的类或某种类型的文件**，这是因为你可以创建自定义 scaffolders 输出测试装置、 部署脚本或你需要的任何其他内容。 你的团队中的每个人都可以将你的自定义 scaffolders。

MvcScaffolding 中的其他功能包括：

- 对于 C# 和 VB 项目的支持
- 对 Razor 和 ASPX 视图引擎的支持
- 支持为 ASP.NET MVC 区域基架和使用自定义视图布局/母版
- 你可以轻松地输出通过编辑自定义 T4 模板
- 你可以添加全新 scaffolders 使用自定义 PowerShell 逻辑和自定义 T4 模板。 这些 （和你向用户提供了任何自定义参数） 会自动出现在控制台 tab 自动补全列表。
- 你可以包含不同技术的其他 scaffolders 的 NuGet 包 （例如，不存在的概念证明一个 linq to SQL 现在） 和混合和匹配它们在一起

ASP.NET MVC 3 Tools 更新包括全力 Visual Studio 支持此基架系统，如：

- 添加控制器对话框现在支持完全自动的基架创建、 读取、 更新和删除控制器操作和相应的视图。 默认情况下，这的基架使用 EF Code First 数据访问代码。
- 添加控制器对话框支持*可扩展的基架*通过 NuGet 包如*MvcScaffolding*。 这样将允许你使用适用于 ODBCDirect 创建 NHibernate 或甚至 JET 等其他数据访问技术的基架，如果你要因此并在对话框的自定义基架插入 ！

有关 ASP.NET MVC 3 中的基架的详细信息，请参阅以下资源：

- Steve Sanderson 文章系列，包括： 

    1. [简介： 搭建基架 ASP.NET MVC 3 项目与 MvcScaffolding 包](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [标准用法： 典型用例和选项](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [一个对多关系](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [基架操作和单元测试](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [重写 T4 模板](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [此文章： 创建自定义 scaffolders](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- 从在 PDC 2010 会话 Scott Hanselman post[构建与 Microsoft"未命名包的 Web Love"博客](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>HTML 5 项目模板

新项目对话框包括项目模板的复选框启用 HTML 5 版本。 这些模板利用 Modernizr 1.7，以提供对 HTML 5 和 CSS 3 在低级别浏览器中的兼容性支持。

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Razor 视图引擎

ASP.NET MVC 3 附带了名为提供以下好处的 Razor 新视图引擎：

- Razor 语法是干净和简洁，需最小数量的击键。
- Razor 很容易地了解，在某种因为它基于现有的语言，如 C# 和 Visual Basic。
- Visual Studio 包含 Razor 语法的 IntelliSense 和代码着色。
- Razor 视图可以是单元测试，而无需您运行应用程序或启动 web 服务器。

一些新的 Razor 功能包括：

- `@model`用于指定传递到该视图的类型的语法。
- `@* *@`注释语法。
- 能够指定默认值 (如`layoutpage`) 一次针对整个站点。
- `Html.Raw`用于显示不带 HTML 编码的文本的方法它。
- 对多个视图之间共享代码的支持 (*\_viewstart.cshtml*或 *\_viewstart.vbhtml*文件)。

Razor 还包括新的 HTML 帮助器，如下所示：

- `Chart`。 呈现图表中，产品与 ASP.NET 4 中的图表控件相同的功能。
- `WebGrid`。 呈现数据网格中，完成，但分页和排序功能。
- `Crypto`。 使用哈希算法来创建正确 salted 和哈希处理密码。
- `WebImage`。 呈现图像。
- `WebMail`。 发送电子邮件。

有关 Razor 的详细信息，请参阅以下资源：

- [引入 Razor Scott Guthrie 的博客文章](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Scott Guthrie 的博客文章简介@model关键字](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Scott Guthrie 的博客文章简介 Razor 布局](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Razor API 快速参考](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>对多个视图引擎的支持

**添加视图**ASP.NET MVC 3 中的对话框，可以选择你想要使用的视图引擎和**新项目**对话框中，可以指定项目的默认视图引擎。 你可以如选择 Web 窗体视图引擎 (ASPX)、 Razor 或开放源代码视图引擎[Spark](http://sparkviewengine.com/)， [NHaml](https://code.google.com/p/nhaml/)，或[NDjango](http://ndjango.org/)。

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>控制器改进

### <a name="global-action-filters"></a>全局操作筛选器

有时你想要执行逻辑操作方法运行之前或之后运行的操作方法。 若要支持此功能，ASP.NET MVC 2 提供的操作筛选器。 操作筛选器是提供一种将预操作和后操作行为添加到特定控制器操作方法的声明性方法的自定义属性。 但是，在某些情况下你可能想要指定适用于所有的操作方法的操作前行为或操作后行为。 MVC 3，可以通过将它们添加到指定全局筛选器`GlobalFilters`集合。 有关全局操作筛选器的详细信息，请参阅以下资源：

- [MVC 3 Preview 上的 Scott Guthrie 的博客](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [在 ASP.NET MVC 中筛选](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>新的"ViewBag"属性

MVC 2 控制器支持`ViewData`使您能够将数据传递给使用后期绑定字典 API 的视图模板的属性。 在 MVC 3，你还可以使用某种程度上更简单的语法与`ViewBag`属性来完成相同的目的。 例如，而不是编写`ViewData["Message"]="text"`，你可以编写`ViewBag.Message="text"`。 不需要定义任何强类型类使用`ViewBag`属性。 因为它是动态的属性，你可以转而只需获取或设置属性和它将解析它们动态在运行时。 在内部，`ViewBag`属性存储为名称/值对中`ViewData`字典。 (注意： 在大多数的预发行版本的 MVC 3`ViewBag`属性的名称为`ViewModel`属性。)

### <a name="new-actionresult-types"></a>新的"ActionResult"类型

以下`ActionResult`新增或增强在 MVC 3 是类型和相应的帮助器方法：

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx)。 向客户端返回 HTTP 状态代码为 404。
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx)。 返回临时重定向 （HTTP 302 状态代码） 或根据一个布尔型参数的永久重定向 （HTTP 301 状态代码）。 与此更改后，结合[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx)类现在具有执行永久重定向的三个方法： `RedirectPermanent`， `RedirectToRoutePermanent`，和`RedirectToActionPermanent`。 这些方法返回的实例`RedirectResult`与`Permanent`属性设置为`true`。
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx)。 返回用户指定的 HTTP 状态代码。

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>JavaScript 和 Ajax 改进

默认情况下，在 MVC 3 的 Ajax 和验证帮助器使用的非介入式 JavaScript 方法。 非介入式 JavaScript 可避免将内联 JavaScript 注入到 HTML。 这可以让你 HTML 较小、 更混乱，并使其更轻松地替换或自定义 JavaScript 库。 MVC 3 中的验证帮助器还使用`jQueryValidate`默认的插件。 如果你想 MVC 2 行为，则可以禁用非介入式 JavaScript 使用*web.config*文件设置。 有关 JavaScript 和 Ajax 改进的详细信息，请参阅以下资源：

- [Wikipedia 站点上的非介入式 JavaScript 的基本简介](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad wilson 制作的非介入式 JavaScript Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad wilson 制作的非介入式 JavaScript 验证 Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [使用 Razor 和非介入式 JavaScript 创建 MVC 3 应用程序](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md)（ASP.NET 站点上教程）
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>默认情况下启用的客户端验证

在早期版本的 MVC，你需要显式调用`Html.EnableClientValidation`从以启用客户端验证视图的方法。 在 MVC 3 这不再需要因为默认情况下启用客户端验证。 (你可以禁用此选项使用中的设置*web.config*文件。)

为了使客户端验证正常工作，你仍需要引用相应 jQuery 和你的站点中的 jQuery 验证库。 可以在你自己的服务器上托管这些库，也可以从 Microsoft 或 Google Cdn 等内容交付网络 (CDN) 中引用它们。

### <a name="remote-validator"></a>远程验证程序

ASP.NET MVC 3 支持新[RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx)使您能够利用 jQuery 验证即插即用中的类所提供的远程验证程序支持。 这使客户端验证库自动调用若要执行仅可完成的验证逻辑服务器定义的自定义方法服务器端。

在下面的示例中，`Remote`属性指定客户端验证将调用名为操作`UserNameAvailable`上`UsersController`若要验证的类`UserName`字段。

[!code-csharp[Main](mvc3/samples/sample1.cs)]

下面的示例显示相应的控制器。

[!code-csharp[Main](mvc3/samples/sample2.cs)]

有关如何使用`Remote`属性，请参阅[如何： 在 ASP.NET MVC 实现远程验证](https://msdn.microsoft.com/library/gg508808(VS.98).aspx)MSDN 库中。

### <a name="json-binding-support"></a>JSON 绑定支持

ASP.NET MVC 3 包括内置 JSON 绑定支持，使得操作接收 JSON 编码数据和模型它将与绑定操作方法参数的方法。 此功能是涉及客户端模板和数据绑定的方案中十分有用。 （客户端可以模板来格式化并使用在客户端执行的模板显示单个数据项的集。）MVC 3 可以轻松地将客户端模板连接使用的服务器上的操作方法，来发送和接收 JSON 数据。 有关 JSON 绑定支持的详细信息，请参阅**JavaScript 和 AJAX 改进**部分[Scott Guthrie 的博客文章 MVC 3 预览](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>模型验证的改进

### <a name="dataannotations-metadata-attributes"></a>"DataAnnotations"元数据属性

ASP.NET MVC 3 支持`DataAnnotations`元数据特性例如`DisplayAttribute`。

### <a name="validationattribute-class"></a>"ValidationAttribute"类

`ValidationAttribute`类也已经过改进在.NET Framework 4，以支持新`IsValid`提供了有关当前验证上下文，如哪些对象正在验证的详细信息的重载。 这使你可以在其中验证基于模型的另一个属性的当前值的更丰富方案。 例如，新`CompareAttribute`特性，可以比较两个模型属性的值。 在下面的示例中，`ComparePassword`属性必须与匹配`Password`字段才有效。

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>验证接口

[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx)界面使您可以执行模型级别验证，并且它使您能够提供特定于整个模型，或在模型内的两个属性之间的状态的错误消息的验证. MVC 3 现在检索从错误`IValidatableObject`接口时模型绑定，并自动标志或突出显示了影响使用的内置的 HTML 窗体的帮助器的视图中的字段。

[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx)接口使验证程序是否对客户端验证的支持在运行时发现的 ASP.NET MVC。 此接口已经过设计，因此，它可以与多种验证框架集成。

有关验证接口的详细信息，请参阅**模型验证改进**部分[Scott Guthrie 的博客文章 MVC 3 预览](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。 （但是，请注意，对"IValidateObject"博客中的引用应"IValidatableObject"。）

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>依赖关系注入改进

ASP.NET MVC 3 应用依赖关系注入 (DI) 和集成与依赖关系注入或控制反向 (IOC) 容器提供更好地支持。 以下区域中添加了对 DI 支持：

- （注册和将注入控制器工厂，将注入控制器） 的控制器。
- （注册以及将注入视图引擎，将依赖关系注入到视图页） 视图。
- 操作筛选器 （查找和将注入筛选器）。
- 模型联编程序 （注册和注入）。
- 为模型验证提供程序 （注册和注入）。
- 为模型元数据提供程序 （注册和注入）。
- 值提供程序 （注册和注入）。

MVC 3 还支持[常见服务定位器](https://github.com/unitycontainer/commonservicelocator)库和支持该库的任何 DI 容器`IServiceLocator`接口。 它还支持新`IDependencyResolver`轻松地集成 DI 框架的接口。

有关 DI MVC 3 中的详细信息，请参阅以下资源：

- [Brad wilson 制作的系列的服务位置上的博客文章](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>其他新功能

### <a name="nuget-integration"></a>NuGet 集成

ASP.NET MVC 3 自动安装，并作为其安装的一部分启用 NuGet。 NuGet 是一个免费的开源包管理器，可以轻松地查找、 安装和在你项目中使用.NET 库和工具。 它适用于所有 Visual Studio 项目类型 （包括 ASP.NET Web 窗体和 ASP.NET MVC）。

NuGet 让开发人员维护开放源代码项目 （例如，如 Moq、 NHibernate、 Ninject、 StructureMap、 NUnit、 Windsor、 RhinoMocks 和 Elmah 的项目） 以其库打包并在联机库中注册它们。 然后，它很容易的.NET 开发人员想要使用这些库之一来找到程序包，然后将它安装在他们正在处理的项目。

与 ASP.NET 3 Tools 更新，项目模板包括 JavaScript 库预安装的 NuGet 包，因此它们是通过 NuGet 可更新。 Entity Framework Code First 还预安装了作为 NuGet 程序包。

有关 NuGet 的详细信息，请参阅 [NuGet 文档](https://docs.microsoft.com/nuget/)。

### <a name="partial-page-output-caching"></a>局部页面输出缓存

ASP.NET MVC 具有支持版本 1 以来，输出缓存的完整的页面响应。 MVC 3 还支持局部页面输出缓存，使您能够轻松地缓存区域或的响应片段。 有关缓存的详细信息，请参阅**部分页面输出缓存**部分[Scott Guthrie 的博客文章在 MVC 3 候选发布版](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)和**子操作输出缓存**部分[MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)。

### <a name="granular-control-over-request-validation"></a>对请求验证进行精细控制

ASP.NET MVC 具有自动有助于保护计算机免受 XSS 和 HTML 注入攻击的内置请求验证。 但是，有时要显式禁用请求验证，例如，如果你想要让用户发布 HTML 内容 （例如，在博客条目或 CMS 内容）。 你现在可以添加[AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx)属性设为模型或查看模型禁用基于每个属性过程模型绑定中的请求验证。 有关请求验证的详细信息，请参阅以下资源：

- **非介入式 JavaScript 和验证**主题中[Scott Guthrie 的博客文章在 MVC 3 候选发布版](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)。
- [MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>可扩展的"新建项目"对话框

在 ASP.NET MVC 3 中，你可以添加项目模板，视图引擎和单元测试项目框架到**新项目**对话框。

### <a name="template-scaffolding-improvements"></a>模板基架改进

ASP.NET MVC 3 基架模板执行更好地标识模型上的主键属性并进行相应地比在早期版本的 MVC 处理。 （例如，基架模板现在确保为主键不为一个可编辑窗体字段基架。）

默认情况下，创建和编辑的基架现在使用`Html.EditorFor`帮助器而不是`Html.TextBoxFor`帮助器。 这可改进对形式的数据模型的元数据支持批注属性时**添加视图**对话框生成视图。

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>"Html.LabelFor"和"Html.LabelForModel"的新重载

为添加新的方法重载了`LabelFor`和`LabelForModel`帮助器方法。 新的重载使你能够指定或重写的标签文本。

### <a name="sessionless-controller-support"></a>无会话控制器支持

在 ASP.NET MVC 3 中你可以指示是否要在控制器类，以使用会话状态，并且如果是这样，是否会话状态应为读/写或只读的。 有关无会话控制器支持的详细信息，请参阅[MVC 3 发行说明](../whitepapers/mvc3-release-notes.md)。

### <a name="new-additionalmetadataattribute-class"></a>新的"AdditionalMetadataAttribute"类

你可以使用[AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx)特性来填充`ModelMetadata.AdditionalValues`模型属性的字典。 例如，如果视图模型具有一个属性，应仅向管理员显示，还可以批注该属性，如下面的示例中所示：

[!code-csharp[Main](mvc3/samples/sample4.cs)]

呈现产品视图模型时，此元数据将提供给任何显示或编辑器模板。 它是由您来解释的元数据信息。

### <a name="accountcontroller-improvements"></a>AccountController 改进

AccountController 中的 Internet 项目模板中已大大改进。

### <a name="new-intranet-project-template"></a>新建 Intranet 项目模板

新的 Intranet 项目模板是包含其启用 Windows 身份验证并删除 AccountController 中。
