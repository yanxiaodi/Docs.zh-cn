---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "本文档介绍 Visual Studio 2010 的 ASP.NET MVC 4 Beta 版。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: d6797d1dbacff7503f74782d325ff5a9598970c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> 本文档介绍 Visual Studio 2010 的 ASP.NET MVC 4 Beta 版。
> 
> > [!NOTE]
> > 这不是最新版本。 提供了 ASP.NET MVC 4 RC 发行说明[此处](mvc4-release-notes.md)。


- [安装说明](#_Toc303253802)
- [文档](#_Toc303253803)
- [支持](#_Toc303253804)
- [软件要求](#_Toc303253805)
- [将 ASP.NET MVC 3 项目升级到 ASP.NET MVC 4](#_Toc303253806)
- [ASP.NET MVC 4 Beta 中的新增功能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET 单页面应用程序](#_Toc317096198)
    - [默认的项目模板的增强功能](#_Toc303253808)
    - [移动项目模板](#_Toc303253809)
    - [显示模式](#_Toc303253810)
    - [jQuery Mobile，视图切换器和浏览器重写](#_Toc303253811)
    - [Visual Studio 中的代码生成的配方](#_Toc303253812)
    - [对异步控制器任务支持](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [已知的问题和重大更改](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>安装说明

可以从安装的 Visual Studio 2010 的 ASP.NET MVC 4 Beta [ASP.NET MVC 4 主页](../mvc/mvc4.md)使用 Web 平台安装程序。

你必须卸载安装 ASP.NET MVC 4 Beta 之前的 ASP.NET MVC 4 的任何以前安装的预览。

此版本不兼容与.NET Framework 4.5 开发者预览版。 安装 ASP.NET MVC 4 Beta 之前，必须卸载.NET 4.5 开发者预览版。

ASP.NET MVC 4 可以安装，并可以运行的并行与 ASP.NET MVC 3。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>文档

ASP.NET MVC 的文档可能会在以下 URL MSDN 网站上提供：

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

教程和有关 ASP.NET MVC 的其他信息页还提供 MVC 4 的 ASP.NET 网站 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>支持

这是预览版本，未正式受到支持。 如果你有关于使用此版本的问题，则将其发布到 ASP.NET MVC 论坛 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中 ASP.NET 社区的成员都是经常能够提供非正式的支持。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>软件要求

Visual Studio 的 ASP.NET MVC 4 组件需要 PowerShell 2.0 以及 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>将 ASP.NET MVC 3 项目升级到 ASP.NET MVC 4

ASP.NET MVC 4 可以在同一台计算机，这将使您能够灵活地选择何时升级到 ASP.NET MVC 4 ASP.NET MVC 3 应用程序上的与 ASP.NET MVC 3 并行安装。

升级的最简单方法是若要创建一个新的 ASP.NET MVC 4 项目并复制所有视图、 控制器、 代码和内容都文件从现有的 MVC 3 项目到新项目，然后以更新该程序集引用在新项目中为与旧项目匹配。 如果到 MVC 3 项目中的 Web.config 文件进行了更改，你必须还将这些更改合并到 MVC 4 项目中的 Web.config 文件。

若要手动升级到版本 4 现有的 ASP.NET MVC 3 应用程序，请执行以下操作：

1. 在项目 （还有一个项目，一个视图文件夹中，一个在你的项目中每个区域的 Views 文件夹的根目录中） 中的所有 Web.config 文件，请将以下文本的每个实例：

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    使用以下对应的文本：

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. 在根 Web.config 文件中，更新*webPages:Version* "2.0.0.0"的元素，并添加新*PreserveLoginUrl*具有值"true"的密钥：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. 在解决方案资源管理器，删除对引用*System.Web.Mvc* （它指向版本 3 DLL）。 然后添加对的引用*System.Web.Mvc* (v4.0.0.0)。 具体而言，进行以下更改，以便更新的程序集引用。 以下为详细信息：

    1. 在解决方案资源管理器，删除对以下程序集的引用： 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. 添加对以下程序集引用： 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. 在解决方案资源管理器，右键单击项目名称，然后选择卸载项目。 再次右键单击名称，然后选择编辑*ProjectName*.csproj。
5. 找到*ProjectTypeGuids*元素，并替换 {E53F8FEA-EAE0-44A6-8774-FFD645390401} 为 {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 保存所做的更改，关闭已编辑的项目 (.csproj) 文件，右键单击该项目，然后选择重新加载项目。
7. 如果该项目引用任何第三方库使用早期版本的 ASP.NET MVC，打开根 Web.config 文件并添加以下三种*bindingRedirect*下的元素*配置*部分： 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>ASP.NET MVC 4 Beta 中的新增功能

本部分介绍已引入的功能在 ASP.NET MVC 4 Beta 版本。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 现在包含 ASP.NET Web API，一个新的框架，用于创建 HTTP 服务，可以覆盖广泛的客户端包括浏览器和移动设备。 ASP.NET Web API 也是用于生成 RESTful 服务的理想平台。

ASP.NET Web API 包括以下功能的支持：

- **现代的 HTTP 编程模型：**直接访问和处理 HTTP 请求和响应中你使用新的强类型化的 HTTP 对象模型的 Web Api。 相同的编程模型和 HTTP 管道是在客户端通过新的 HttpClient 类型上所需可用。
- **完全支持路由**: Web Api 现在支持完整的一套路由功能，它们始终被 Web 堆栈，包括路由参数和约束的一部分。 此外，映射到操作具有完全支持约定，因此你不再需要将如 [HttpPost] 特性应用到您的类和方法。
- **内容协商**： 客户端和服务器可协同工作来确定正确的格式为从 API 返回的数据。 我们提供对 XML、 JSON 和窗体 URL 编码格式的默认支持并可以通过添加你自己格式化程序，扩展此支持或甚至替换默认内容协商策略。
- **模型绑定和验证：**模型联编程序提供了简便的方法，以从各个部分的 HTTP 请求中提取数据并将这些消息部分转换为.NET 对象以便可以由 Web API 操作。
- **筛选器：** Web Api 现在支持筛选器，包括已知的筛选器，如 [Authorize] 属性。 可以创作并插入自己的筛选器操作、 授权和异常处理。
- **查询组合：**通过只需返回 IQueryable&lt;T&gt;，你的 Web API 将支持的 OData URL 约定通过查询。
- **改进了 HTTP 详细信息的可测试性：**而不是静态的上下文对象中设置 HTTP 详细信息，Web API 操作现在可以使用 HttpRequestMessage 和 HttpResponseMessage 的实例。 这些对象的泛型版本还存在以便你可以使用你自定义类型，除了 HTTP 类型。
- **改进了控制反向 (IoC) 通过 DependencyResolver:** Web API 现在使用由 MVC 的依赖项解析程序实现的服务定位符模式来获取为许多不同设施的实例。
- **基于代码的配置：**只有通过代码来完成 Web API 配置、 离开 config 文件清理。
- **自承载：** Web Api 可以同时仍可使用的路由的完整功能和其他功能的 Web API 承载在 IIS 除了过程中。

有关 ASP.NET Web API 的详细信息，请访问[https://www.asp.net/web-api](../web-api/index.md)。

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET 单页面应用程序

ASP.NET MVC 4 现在包括用于与使用 JavaScript 和 Web Api 的重要客户端交互构建单页面应用程序的体验的早期预览版本。 这种支持包括：

- 一组更丰富的本地交互，与缓存的数据的 JavaScript 库
- 单元的工作和 DAL 支持的其他 Web API 组件
- 使用基架，若要快速开始 MVC 项目模板

为支持 ASP.NET MVC 4 中的单页面应用程序的详细信息，请访问[https://www.asp.net/single-page-application](../single-page-application/index.md)。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>默认的项目模板的增强功能

已更新用于创建新的 ASP.NET MVC 4 项目的模板来创建外观更现代的网站：

![](mvc4-beta-release-notes/_static/image1.png)

修饰除改进外，还提高了新的模板中的功能。 模板使用名为自适应呈现来显示在桌面浏览器和不带任何自定义的移动浏览器中良好的技术。

![](mvc4-beta-release-notes/_static/image2.png)

若要查看在操作中的自适应呈现，可以使用移动仿真程序或就是进行尝试调整桌面浏览器窗口较小的大小。 当浏览器窗口中获取足够小时，将发生更改页的布局。

向默认项目模板的另一增强功能是使用 JavaScript 以提供更丰富的 UI。 在模板中使用的登录和注册链接是如何使用 jQuery UI 对话提供丰富的登录屏幕的示例：

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>移动项目模板

如果你刚开始新项目，并想要创建专用于移动的站点和平板电脑浏览器，你可以使用新的移动应用程序项目模板。 这基于 jQuery Mobile，用于构建触控优化 UI 的开放源代码库：

![](mvc4-beta-release-notes/_static/image4.png)

此模板包含 Internet 应用程序模板相同的应用程序结构 （和控制器代码是几乎相同的），但它是风格使用 jQuery Mobile 外观精美，同时也在基于 touch 的移动设备上的行为。 若要了解有关如何构建并设置样式移动 UI 的详细信息，请参阅[jQuery 移动项目网站](http://jquerymobile.com/)。

如果你已有面向桌面的站点，你想要添加移动优化的视图，或者如果你想要创建的单个站点，以便提供对桌面和移动浏览器样式有所不同视图，你可以使用新的显示模式功能。 （请参阅下一节。）

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>显示模式

使用新的显示模式功能，应用程序选择具体取决于正在发出请求的浏览器的视图。 例如，如果桌面浏览器请求主页上，应用程序可能使用 Views\Home\Index.cshtml 模板。 如果移动浏览器请求主页上，该应用程序可能返回 Views\Home\Index.mobile.cshtml 模板。

布局和它们还可以覆盖特定浏览器类型。 例如:

- 如果你 Views\Shared 文件夹包含\_Layout.cshtml 和\_Layout.mobile.cshtml 模板，默认情况下应用程序将使用\_期间从移动浏览器和请求Layout.mobile.cshtml\_Layout.cshtml 期间其他请求。
- 如果一个文件夹包含\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指令@Html.Partial("\_MyPartial") 将呈现\_MyPartial.mobile.cshtml 期间从移动设备的请求浏览器中，和\_MyPartial.cshtml 期间其他请求。

如果你想要创建更具体的视图、 布局或为其他设备的分部视图，则可以注册一个新*DefaultDisplayMode*实例以指定要搜索请求满足特定条件的名称。 例如，可以添加到下面的代码*应用程序\_启动*Global.asax 文件以将字符串"iPhone"注册为适用于 Apple iPhone 浏览器发出请求的显示模式中的方法：

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

此代码运行时的 Apple iPhone 浏览器发出请求后，你的应用程序将使用 views/shared\\_Layout.iPhone.cshtml 布局 （如果存在）。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile，视图切换器和浏览器重写

jQuery Mobile 是用于构建触控优化 web UI 一个开放源代码库。 如果你想要使用 jQuery Mobile 与 ASP.NET MVC 4 应用程序，你可以下载并安装 NuGet 包，可帮助您开始。 若要从 Visual Studio 包管理器控制台中安装它，请键入以下命令：

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

这将安装 jQuery Mobile 和某些帮助程序文件，其中包括：

- 视图/共享/\_Layout.Mobile.cshtml，即 jQuery 基于移动的布局。
- 视图切换器组件，其中包括视图/共享/\_ViewSwitcher.cshtml 分部视图和 ViewSwitcherController.cs 控制器。

安装包后，运行应用程序使用移动浏览器 (或等效的如 Firefox[用户代理切换器](http://chrispederick.com/work/user-agent-switcher/)外接程序)。 你将看到： 看起来您的网页，差别很大，因为 jQuery Mobile 可处理的布局和样式。 若要利用此功能，请执行以下操作：

- 在所述创建移动特定视图替代[显示模式](#_Toc303253810)更早版本 （例如，创建 Views\Home\Index.mobile.cshtml 以移动浏览器的替代 Views\Home\Index.cshtml）。
- 读取[jQuery 移动文档](http://jquerymobile.com/)若要了解有关如何在移动视图中添加触控优化 UI 元素的详细信息。

移动优化网页中的约定是将添加其文本是如桌面视图或完整的站点模式，从而让用户切换到该页面的桌面版本的链接。 JQuery.Mobile.MVC 包包括用于执行此操作的示例视图切换器组件。 使用默认情况下 views/shared\\_Layout.Mobile.cshtml 视图，其呈现页时，其外观类似如下：

![](mvc4-beta-release-notes/_static/image5.png)

如果访问者单击此链接，它们正在切换到同一页上的桌面版本。

因为桌面布局并不包含视图切换器，默认情况下，访问者将不具有一种方法可用于访问移动模式。 若要启用此功能，将添加到以下引用 *\_ViewSwitcher*到桌面的布局，只在*&lt;正文&gt;*元素：

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

视图切换器使用称为重写浏览器的新增功能。 此功能允许你将视为它们已传入的请求的应用程序从不同浏览器 （用户代理） 比它们是实际从。 下表列出了浏览器重写提供的方法。

| `HttpContext.SetOverriddenBrowser(userAgentString)` | 使用指定的用户代理请求的实际用户代理值覆盖。 |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | 如果已指定不重写将返回请求的用户代理重写值或实际用户代理字符串。 |
| `HttpContext.GetOverriddenBrowser()` | 返回*HttpBrowserCapabilitiesBase*对应于当前为请求设置的用户代理的实例 （实际或重写）。 你可以使用此值以获取属性，如*IsMobileDevice*。 |
| `HttpContext.ClearOverriddenBrowser()` | 删除当前请求的任何重写的用户代理。 |

浏览器重写的 ASP.NET MVC 4 核心功能，即使你不安装 jQuery.Mobile.MVC 包也可用。 但是，它会影响仅视图、 布局和分部视图选择 — 它不会影响依赖于任何其他 ASP.NET 功能*Request.Browser*对象。

默认情况下，使用 cookie 存储用户代理重写。 如果你想要存储替代其他位置 （例如，在数据库中），你可以替换默认的提供程序 (*BrowserOverrideStores.Current*)。 为此提供程序的文档将可以附带 ASP.NET MVC 的更高版本。

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Visual Studio 中的代码生成的配方

使用新的配方功能，Visual Studio 生成基于你可以使用 NuGet 安装的包的特定于解决方案的代码。 配方 framework 轻松开发人员编写代码生成插件，你还可用于替换内置代码生成器添加区域、 添加控制器和添加视图。 因为配方作为 NuGet 包部署，可以轻松地签入源代码管理以及自动与项目的所有开发人员共享。 此外，还提供基于每个解决方案。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>对异步控制器任务支持

您现在可以编写异步操作方法的返回类型的对象为单个方法*任务*或*任务&lt;ActionResult&gt;*。

例如，如果你使用 Visual C# 5 (或使用[异步 CTP](https://msdn.microsoft.com/vstudio/async.aspx))，你可以创建的异步操作方法，如下所示：

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

在上一操作方法中，对的调用*newsService.GetHeadlinesAsync*和*sportsService.GetScoresAsync*异步调用，不会阻止线程池中的线程。

返回的异步操作方法*任务*实例还可以支持超时。 为可取消操作方法，请添加类型的参数*CancellationToken*到操作方法签名。 下面的示例演示了异步操作方法，它具有 2500年毫秒的超时，且显示*TimedOut*查看到客户端，如果发生超时。

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 Beta 支持 Windows Azure SDK 1.5 2011 年 9 月版本。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

- **安装后 ASP.NET MVC 4 Beta，Visual Studio 2010 Service Pack 1 CSHTML/VBHTML 编辑器中的 CSHTML/VBHTML 编辑器可能会暂停在 cshtml 或 vbhtml 文件中键入代码段或 JavaScript 后很长时间。** 仅在具有刚刚创建且尚未编译的 ASP.NET MVC 4 应用程序中发生这种情况。

    解决方法是以编译该项目的 bin 文件夹中获取程序集。 请注意，是否你清除该项目，该对话框从 bin 文件夹中移除程序集，编辑器问题还会回来。

    将下一个版本中更正此问题。
- **Visual Studio 11 Beta 的 C# 项目模板包含 Global.asax.cs 中的不正确的连接字符串。** 指定应用程序中的默认连接\_在 Visual Studio 11 Beta 中创建的项目包含 LocalDB 连接字符串，它包含非转义的反斜杠的启动方法 (\)字符。 这会导致在尝试访问实体框架 DbContext，后者将生成 SqlException 时出现连接错误。

    若要更正此问题，转义应用程序中的反斜杠字符\_开始 Global.asax.cs 方法，使它显示如下：

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **针对.NET 4.5 的 ASP.NET MVC 4 应用程序将引发 FileLoadException 后尝试访问在.NET 4.0 下运行时的 System.Net.Http.dll 程序集。** 在.NET 4.5 下创建的 ASP.NET MVC 4 应用程序包含绑定重定向将导致 FileLoadException 这指示"无法加载文件或程序集 System.Net.Http 或其依赖项之一。" 当执行的应用程序的系统上安装.NET 4.0。 若要更正此问题，请从 web.config 中删除以下绑定重定向：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    修改后的 web.config 中的程序集绑定元素应如下所示：

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **Visual Basic 项目中的"添加控制器"项模板生成不正确的命名空间时调用 * * * 从某个区域内。** 当控制器添加到 ASP.NET MVC 项目使用 Visual Basic 中的某个区域时，项模板中的错误的命名空间插入控制器。 导航到在控制器中的任何操作时，则结果将为"找不到文件"错误。  
  
 生成的命名空间的根命名空间后省略的所有内容。 例如，生成的命名空间是*RootNamespace*但应为*RootNamespace.Areas.AreaName.Controllers* 。
- **在 Razor 视图引擎中的重大更改。** 作为 Razor 分析器的重写的一部分，以下类型已从删除*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 此外已删除以下方法： 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **中的 ASP.NET MVC 4 应用的 /bin 目录包含 WebMatrix.WebData.dll，则它将接管窗体身份验证的 URL。** 将 WebMatrix.WebData.dll 程序集添加到你的应用程序，（例如，通过使用添加部署依赖性对话框时，请选中"Razor 语法的 ASP.NET 网页"） 将重写登录/帐户 / 身份验证登录重定向而非 /帐户/登录按照默认 ASP.NET MVC Account 控制器的期望。 若要防止此行为，并使用指定的 URL 已在 web.config 的身份验证部分中，可以添加调用 PreserveLoginUrl appSetting，并将其设置为 true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **NuGet 包管理器安装尝试并行安装的 Visual Studio 2010 和 Visual Web Developer 2010 安装 ASP.NET MVC 4 时失败。** 若要运行 Visual Studio 2010 和 Visual Web Developer 2010 与 ASP.NET MVC 4 并行必须已安装这两个版本的 Visual Studio 后安装 ASP.NET MVC 4。
- **如果已卸载系统必备组件，则无法卸载 ASP.NET MVC 4。** 若要完全卸载 ASP.NET MVC 4you 必须在卸载 Visual Studio 之前卸载 ASP.NET MVC 4。
- **运行默认的 Web API 项目中显示错误地定向用户将添加使用 RegisterApis 方法，不存在的路由的说明。** 应在使用 ASP.NET 路由表 RegisterRoutes 方法中添加路由。
- **安装 ASP.NET MVC 4 Beta 中断 ASP.NET MVC 3 RTM 应用程序。** 已创建的 ASP.NET MVC 3 应用程序与 RTM （不使用 ASP.NET MVC 3 Tools 更新版本中） 的版本所需要的才能正常使用 ASP.NET MVC 4 Beta 并行的以下更改。 无需在编译错误进行这些更新结果生成项目。 

    **所需的更新**

    1. 在根 Web.config 文件中，添加新 *&lt;appSettings&gt;* 具有键项*webPages:Version*和值*1.0.0.0*。

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. 在解决方案资源管理器，右键单击项目名称，然后选择卸载项目。 再次右键单击名称，然后选择编辑*ProjectName*.csproj。
    3. 找到以下程序集引用： 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        将它们替换为以下：

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. 保存所做的更改，关闭了编辑，然后右键单击项目并选择重新加载项目 (.csproj) 文件。
