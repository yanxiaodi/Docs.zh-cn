---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "本文档介绍 ASP.NET MVC 4 发行的版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: bea6f6112388290a2c6b5ed267626ba28fc36671
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> 本文档介绍 ASP.NET MVC 4 发行的版本。


- [安装说明](#_Toc303253802)
- [文档](#_Toc303253803)
- [支持](#_Toc303253804)
- [软件要求](#_Toc303253805)
- [ASP.NET MVC 4 中的新增功能](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [默认的项目模板的增强功能](#_Toc303253808)
    - [移动项目模板](#_Toc303253809)
    - [显示模式](#_Toc303253810)
    - [jQuery Mobile，视图切换器和浏览器重写](#_Toc303253811)
    - [对异步控制器任务支持](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [数据库迁移](#_Toc303253818)
    - [空项目模板](#_Toc303253819)
    - [将控制器添加到任何项目文件夹](#_Toc303253820)
    - [捆绑和缩小](#_Toc303253821)
    - [启用从 Facebook 和其他站点使用 OAuth 和 OpenID 登录名](#_Toc303253822)
- [将 ASP.NET MVC 3 项目升级到 ASP.NET MVC 4](#_Toc303253806)
- [从 ASP.NET MVC 4 候选发布版的更改](#_Toc303253817)
- [已知的问题和重大更改](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>安装说明

可以从安装 Visual Studio 2010 的 ASP.NET MVC 4 [ASP.NET MVC 4 主页](../mvc/mvc4.md)使用 Web 平台安装程序。

我们建议卸载安装 ASP.NET MVC 4 之前的 ASP.NET MVC 4 的任何以前安装的预览。 可以在不卸载的情况下的 ASP.NET MVC 4 Beta 和候选发布版升级到 ASP.NET MVC 4。

此版本不兼容与任何预览版本的.NET Framework 4.5。 到最终版本在安装 ASP.NET MVC 4 之前，必须单独升级的任何已安装的预览版本的.NET Framework 4.5。

ASP.NET MVC 4 可以安装和运行的并行与 ASP.NET MVC 3。

<a id="_Toc303253803"></a>
## <a name="documentation"></a>文档

ASP.NET MVC 的文档可能会在以下 URL MSDN 网站上提供：

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

教程和有关 ASP.NET MVC 的其他信息页还提供 MVC 4 的 ASP.NET 网站 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。

<a id="_Toc303253804"></a>
## <a name="support"></a>支持

完全支持 ASP.NET MVC 4。 如果你有关于使用此版本问题你可以还将其发布到 ASP.NET MVC 论坛 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))，其中 ASP.NET 社区的成员都是经常能够提供非正式的支持。

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>软件要求

Visual Studio 的 ASP.NET MVC 4 组件需要 PowerShell 2.0 以及 Service Pack 1 的 Visual Studio 2010 或 Visual Web Developer Express 2010 Service Pack 1。

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>ASP.NET MVC 4 中的新增功能

本部分介绍已引入的功能在 ASP.NET MVC 4 版本。

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 包括 ASP.NET Web API，新的框架，用于创建可以覆盖广泛的客户端包括浏览器和移动设备的 HTTP 服务。 ASP.NET Web API 也是用于生成 RESTful 服务的理想平台。

ASP.NET Web API 包括以下功能的支持：

- **现代的 HTTP 编程模型：**直接访问和处理 HTTP 请求和响应中你使用新的强类型化的 HTTP 对象模型的 Web Api。 相同的编程模型和 HTTP 管道是通过新客户端上所需有*HttpClient*类型。
- **完全支持路由：** ASP.NET Web API 支持的 ASP.NET 路由，包括路由参数和约束的路由功能的完整集。 此外，使用简单的约定操作映射到 HTTP 方法。
- **内容协商：**客户端和服务器可协同工作来确定从 web API 返回的数据的正确格式。 ASP.NET Web API 提供对于 XML，JSON，默认支持和窗体 URL 编码格式，并且你可以通过添加你自己格式化程序，扩展此支持或甚至替换默认内容协商策略。
- **模型绑定和验证：**模型联编程序提供了简便的方法，以从各个部分的 HTTP 请求中提取数据并将这些消息部分转换为.NET 对象以便可以由 Web API 操作。 基于数据注释的 action 参数还执行验证。
- **筛选器：** ASP.NET Web API 支持筛选器，如包括的已知的筛选器*[Authorize]*属性。 可以创作并插入自己的筛选器操作、 授权和异常处理。
- **查询组合：**使用*[Queryable]*上返回的操作的筛选器属性*IQueryable*查询你的 web API 通过 OData 查询约定的支持。
- **改进的可测试性：**而不是静态的上下文对象中设置 HTTP 详细信息，web API 操作工作的实例，并用*HttpRequestMessage*和*HttpResponseMessage*。 创建单元测试项目以及你的 Web API 项目，若要开始快速编写你的 Web API 功能的单元测试。
- **基于代码的配置：**只有通过代码实现 ASP.NET Web API 配置、 离开 config 文件清理。 使用提供的服务定位符模式配置扩展点。
- **改进了对控制反向 (IoC) 容器支持：** ASP.NET Web API 提供通过改进的依赖项解析程序抽象 IoC 容器的全力支持
- **自承载：** Web Api 可以同时仍可使用的路由的完整功能和其他功能的 Web API 承载在 IIS 除了过程中。
- **创建自定义帮助和测试页：**您现在可以轻松地生成自定义帮助和你的 web Api 使用新的测试页*IApiExplorer*要得到你的 web Api 的完整运行时说明的服务。
- **监视和诊断：** ASP.NET Web API 现在提供轻型跟踪基础结构，可以轻松地将现有的日志记录解决方案，例如 System.Diagnostics、 ETW 和第三方日志记录框架与相集成。 你可以通过提供启用跟踪*ITraceWriter*实现并将其添加到你的 web API 配置。
- **链接生成：**使用 ASP.NET Web API *UrlHelper*生成在同一应用程序的相关资源链接。
- **Web API 项目模板：**选择新的 Web API 项目表单新的 MVC 4 项目向导快速使用 ASP.NET Web API 将启动并正在运行。
- **基架：**使用**添加控制器**对话框，以快速创建基于实体框架的 web API 控制器的基架基于模型类型。

有关 ASP.NET Web API 的详细信息，请访问[https://www.asp.net/web-api](../web-api/index.md)。

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>默认的项目模板的增强功能

已更新用于创建新的 ASP.NET MVC 4 项目的模板来创建外观更现代的网站：

![](mvc4-release-notes/_static/image1.png)

修饰除改进外，还提高了新的模板中的功能。 模板使用名为自适应呈现来显示在桌面浏览器和不带任何自定义的移动浏览器中良好的技术。

![](mvc4-release-notes/_static/image2.png)

若要查看在操作中的自适应呈现，可以使用移动仿真程序或就是进行尝试调整桌面浏览器窗口较小的大小。 当浏览器窗口中获取足够小时，将发生更改页的布局。

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>移动项目模板

如果你刚开始新项目，并想要创建专用于移动的站点和平板电脑浏览器，你可以使用新的移动应用程序项目模板。 这基于 jQuery Mobile，用于构建触控优化 UI 的开放源代码库：

![](mvc4-release-notes/_static/image3.png)

此模板包含 Internet 应用程序模板相同的应用程序结构 （和控制器代码是几乎相同的），但它是风格使用 jQuery Mobile 外观精美，同时也在基于 touch 的移动设备上的行为。 若要了解有关如何构建并设置样式移动 UI 的详细信息，请参阅[jQuery 移动项目网站](http://jquerymobile.com/)。

如果你已有面向桌面的站点，你想要添加移动优化的视图，或者如果你想要创建的单个站点，以便提供对桌面和移动浏览器样式有所不同视图，你可以使用新的显示模式功能。 （请参阅下一节。）

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>显示模式

使用新的显示模式功能，应用程序选择具体取决于正在发出请求的浏览器的视图。 例如，如果桌面浏览器请求主页上，应用程序可能使用 Views\Home\Index.cshtml 模板。 如果移动浏览器请求主页上，该应用程序可能返回 Views\Home\Index.mobile.cshtml 模板。

布局和它们还可以覆盖特定浏览器类型。 例如:

- 如果你 Views\Shared 文件夹包含\_Layout.cshtml 和\_Layout.mobile.cshtml 模板，默认情况下应用程序将使用\_期间从移动浏览器和请求Layout.mobile.cshtml\_Layout.cshtml 期间其他请求。
- 如果一个文件夹包含\_MyPartial.cshtml 和\_MyPartial.mobile.cshtml，指令@Html.Partial("\_MyPartial") 将呈现\_MyPartial.mobile.cshtml 期间从移动设备的请求浏览器中，和\_MyPartial.cshtml 期间其他请求。

如果你想要创建更具体的视图、 布局或为其他设备的分部视图，则可以注册一个新*DefaultDisplayMode*实例以指定要搜索请求满足特定条件的名称。 例如，可以添加到下面的代码*应用程序\_启动*Global.asax 文件以将字符串"iPhone"注册为适用于 Apple iPhone 浏览器发出请求的显示模式中的方法：

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

此代码运行时的 Apple iPhone 浏览器发出请求后，你的应用程序将使用 views/shared\\_Layout.iPhone.cshtml 布局 （如果存在）。 显示模式的详细信息，请参阅[ASP.NET MVC 4 移动功能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)。 应用程序使用 DisplayModeProvider 应安装[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet 包。 [ASP.NET 秋季 2012年更新](https://go.microsoft.com/fwlink/?LinkID=271322)包括[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes)新的项目模板中的 NuGet 包。 请参阅[ASP.NET 的 MVC 已由 4 移动缓存的 Bug Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)有关此修补程序的详细信息。

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile 和移动功能

有关生成与 ASP.NET MVC 4 移动应用程序信息使用 jQuery Mobile，请参阅教程[ASP.NET MVC 4 移动功能](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md)。

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>对异步控制器任务支持

您现在可以编写异步操作方法的返回类型的对象为单个方法*任务*或*任务&lt;ActionResult&gt;*。

 有关详细信息请参阅[使用 ASP.NET MVC 4 中的异步方法](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)。

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 支持的 Windows Azure sdk 1.6 和更高版本。

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>数据库迁移

ASP.NET MVC 4 项目现在包括实体框架 5。 在实体框架 5 的强大功能之一是支持数据库迁移。 此功能，可轻松地发展你使用代码为中心的迁移，同时保留在数据库中的数据的数据库架构。 有关数据库迁移的详细信息，请参阅[电影模型和下表中添加新字段](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md)中[简介 ASP.NET MVC 4 教程](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>空项目模板

空 MVC 项目模板现在为真正空的以便你可以开始完全干净状态。 空项目模板的早期版本已更名为基本。

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>将控制器添加到任何项目文件夹

现在，可以右键单击并选择**添加控制器**MVC 项目中的任何文件夹中。 这样，可以更大的灵活性来组织你的控制器，但你想，包括将 MVC 和 Web API 控制器保留在单独的文件夹。

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>绑定和缩减

绑定和缩减框架，可减少的网页需要通过将单个文件组合到单个脚本和 CSS 捆绑文件进行的 HTTP 请求数。 然后，它可以减少这些请求的总体大小由贴图层捆绑包的内容。 贴图层可以包括诸如消除缩短甚至折叠基于其语义的 CSS 选择器的变量名的空格等活动。 捆绑包是声明并在中配置代码，并轻松引用视图帮助器方法可以生成或者通过在对绑定的单个链接，或在调试时，多个链接到单个捆绑包的内容。 有关详细信息请参阅[捆绑和缩减](../mvc/overview/performance/bundling-and-minification.md)。

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>启用从 Facebook 和其他站点使用 OAuth 和 OpenID 登录名

ASP.NET MVC 4 Internet 项目模板中的默认模板现在包括对使用 DotNetOpenAuth 库的 OAuth 和 OpenID 登录的支持。 有关配置 OAuth 或 OpenID 提供程序的信息，请参阅[OAuth/OpenID 支持 WebForms、 MVC 和网页](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx)和[OAuth 和 OpenID 功能文档中的 ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup)。

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>将 ASP.NET MVC 3 项目升级到 ASP.NET MVC 4

ASP.NET MVC 4 可以在同一台计算机，这将使您能够灵活地选择何时升级到 ASP.NET MVC 4 ASP.NET MVC 3 应用程序上的与 ASP.NET MVC 3 并行安装。

升级的最简单方法是若要创建一个新的 ASP.NET MVC 4 项目并复制所有视图、 控制器、 代码和内容都文件从现有的 MVC 3 项目到新项目，然后以更新该程序集引用在新项目中以匹配任何非 MVC 模板中你使用 cluded 组件保存。 如果到 MVC 3 项目中的 Web.config 文件进行了更改，你必须还将这些更改合并到 MVC 4 项目中的 Web.config 文件。

若要手动升级到版本 4 现有的 ASP.NET MVC 3 应用程序，请执行以下操作：

1. 在所有的 Web.config 文件在项目中 （其中一个项目，一个视图文件夹中，一个在你的项目中每个区域的 Views 文件夹的根目录中没有），将以下文本的每个实例 (注意： System.Web.WebPages，Version = 1.0.0.0 中未找到项目使用 Visual Studio 2012 创建的）： 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    使用以下对应的文本：

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. 在根 Web.config 文件中，更新*webPages:Version* "2.0.0.0"的元素，并添加新*PreserveLoginUrl*具有值"true"的密钥： 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. 在解决方案资源管理器，右键单击引用，然后选择管理 NuGet 包。 在左窗格中，选择**Online\NuGet 正式程序包源**，然后更新以下：

    - ASP.NET MVC 4
    - （可选） jQuery，jQuery 验证和 jQuery UI
    - （可选）实体框架
    - (Optonal)Modernizr
4. 在解决方案资源管理器，右键单击项目名称，然后选择卸载项目。 再次右键单击名称，然后选择编辑*ProjectName*.csproj。
5. 找到*ProjectTypeGuids*元素，并替换 {E53F8FEA-EAE0-44A6-8774-FFD645390401} 为 {E3E379DF-F4C6-4180-9B81-6769533ABE47}。
6. 保存所做的更改，关闭已编辑的项目 (.csproj) 文件，右键单击该项目，然后选择重新加载项目。
7. 如果该项目引用任何第三方库使用早期版本的 ASP.NET MVC，打开根 Web.config 文件并添加以下三种*bindingRedirect*下的元素*配置*部分： 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>从 ASP.NET MVC 4 候选发布版的更改

ASP.NET MVC 4 预发布版的发行说明可以在此处找到：

下面概括了 ASP.NET MVC 4 预发布版中此版本中的主要更改：

- **每个控制器配置：** ASP.NET Web API 控制器可以使用实现的自定义特性进行特性化*IControllerConfiguration*如何设置其自己的格式化程序，操作选择器和参数联编程序. *HttpControllerConfigurationAttribute*已删除。
- **每个路由消息处理程序：**现在可以为给定路由请求链中指定的最后一条消息处理程序。 这样，对于持续一段时间沿框架若要使用路由来调度到其自身的支持 (非*IHttpController*) 终结点。
- **进度通知：** *ProgressMessageHandler*生成的请求实体正在上载和下载的响应实体的进度通知。 使用此处理程序可跟踪的距离要上载的请求正文或正在下载响应正文。
- **将内容推送：** *PushStreamContent*类使数据制造者要直接写入请求或响应 （同步或异步） 使用一个流方案。 当*PushStreamContent*已准备好接受它调用至授权操作的输出流的数据。 只要有必要和关闭流编写时已完成，开发人员然后可以写入到流。 *PushStreamContent*检测流在关闭并完成基础异步*任务*用于写出内容。
- **创建错误响应：**使用*HttpError*类型来一致地表示错误信息从验证错误和异常等时仍遵循*IncludeErrorDetailPolicy*. 使用新*CreateErrorResponse*扩展方法来轻松地创建使用的错误响应*HttpError*作为内容。 *HttpError*内容是完全内容协商。
- **删除 MediaRangeMapping:**现在由默认内容 negotiator 处理媒体类型范围。
- **简单类型参数的默认参数绑定现 [FromUri]:**在以前版本的默认参数绑定，简单类型参数使用模型绑定的 ASP.NET Web API。 简单类型参数的默认参数绑定现*[FromUri]*。
- **操作选择具有所需的参数：** ASP.NET Web API 中的操作选择将现在仅选择操作，如果提供来自 URI 的所有必需的参数。 可以通过提供有关操作方法签名中的自变量的默认值为可选指定的参数。
- **自定义 HTTP 参数绑定：**使用*ParameterBindingAttribute*自定义特定的操作参数的参数绑定，或使用*ParameterBindingRules*上*HttpConfiguration*可以自定义参数绑定范围更加广泛。
- **MediaTypeFormatter 改进：**格式化程序现在可以访问完整*HttpContent*实例。
- **缓冲策略选择的主机：**实现和配置*IHostBufferPolicySelector*服务在 ASP.NET Web API 以启用主机来确定缓冲时要使用的策略。
- **主机不可知的方式访问客户端证书：**使用*GetClientCertificate*扩展方法，以从请求消息获取提供的客户端证书。
- **内容协商扩展性：**通过从派生自定义内容协商*DefaultContentNegotiator*和重写你想要的内容协商的任何方面。
- **用于返回 406 不可接受的响应的支持：**你现在可以返回 406 不可接受的响应在 ASP.NET Web API 中通过创建找不到合适的格式化程序时*DefaultContentNegotiator*与*excludeMatchOnTypeOnly*参数设置为*true*。
- **窗体数据作为 NameValueCollection 或 JToken 读取：**可以读取 URI 查询字符串中或作为请求正文中的窗体数据*NameValueCollection*使用*ParseQueryString*和*ReadAsFormDataAsync*扩展方法分别。 同样，你可以读取 URI 查询字符串中或作为请求正文中的窗体数据*JToken*使用*TryReadQueryAsJson*和*ReadAsAsync*&lt;T&gt;扩展方法分别。
- **多部分改进：**现可以用来编写*MultipartStreamProvider*完全适合多部分 MIME 数据其能够读取并以最佳方式向用户显示结果的类型。 您还可以通过以下方式挂钩后续处理步骤上*MultipartStreamProvider*允许实现来完成任何后期处理它在 MIME 多部分正文部分的需要。 例如， *MultipartFormDataStreamProvider*实现读取数据部分的 HTML 窗体并将它们添加到*NameValueCollection*以便它们可轻松地从调用方处获取。
- **链接生成改进：** *UrlHelper*不再依赖于*HttpControllerContext*。 你现在可以访问*UrlHelper*任何上下文中其中*HttpRequestMessage*可用。
- **消息处理程序执行顺序更改：**消息处理程序现在它们进行配置而不是相反的顺序的顺序执行。
- **帮助器方式设置消息处理程序：**新*HttpClientFactory* ，可以连接*DelegatingHandlers*并创建*HttpClient*与准备就绪所需的管道。 它还提供功能的方式与备用的内部处理程序设置 (默认值是*HttpClientHandler*) 以及使用时执行连接*HttpMessageInvoker*或另一个*DelegatingHandler*而不是*HttpClient*作为顶部调用程序。
- **在 ASP.NET Web 优化的 Cdn 的支持：** ASP.NET Web 优化现在提供支持 CDN 备用路径，使你能够指定每个捆绑附加 URL 用于指向该内容传送网络上的相同资源。 支持 Cdn，可获取脚本和样式绑定在地理上靠近 Web 应用程序的结束使用者。
- **ASP.NET Web API 将路由并配置移动到*WebApiConfig.Register*可以是 resused 在测试代码中的静态方法。** ASP.NET Web API 路由之前已在中添加*RouteConfig.RegisterRoutes*以及标准 MVC 将路由。 在单独现在处理的默认 ASP.NET Web API 将路由和配置*WebApiConfig.Register*方法以便于测试。

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

- **应返回移动视图时，ASP.NET MVC 4 的 RC 和 RTM 版本不正确地返回缓存的桌面视图。**

    - 请参阅[ASP.NET MVC 4 移动缓存 Bug 固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)有关此修补程序的详细信息。 可以从安装该修补程序[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet 包。
- **在 Razor 视图引擎中的重大更改**。 以下类型已从删除*System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 此外已删除以下方法： 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **中的 ASP.NET MVC 4 应用的 /bin 目录包含 WebMatrix.WebData.dll，则它将接管窗体身份验证的 URL。** 将 WebMatrix.WebData.dll 程序集添加到你的应用程序，（例如，通过使用添加部署依赖性对话框时，请选中"Razor 语法的 ASP.NET 网页"） 将重写登录/帐户 / 身份验证登录重定向而非 /帐户/登录按照默认 ASP.NET MVC Account 控制器的期望。 若要防止此行为，并使用指定的 URL 已在 web.config 的身份验证部分中，可以添加调用 PreserveLoginUrl appSetting，并将其设置为 true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **NuGet 包管理器安装尝试并行安装的 Visual Studio 2010 和 Visual Web Developer 2010 安装 ASP.NET MVC 4 时失败。** 若要运行 Visual Studio 2010 和 Visual Web Developer 2010 与 ASP.NET MVC 4 并行必须已安装这两个版本的 Visual Studio 后安装 ASP.NET MVC 4。
- **如果已卸载系统必备组件，则无法卸载 ASP.NET MVC 4。** 若要完全卸载 ASP.NET MVC 4you 必须在卸载 Visual Studio 之前卸载 ASP.NET MVC 4。
- **安装 ASP.NET MVC 4 中断 ASP.NET MVC 3 RTM 应用程序。** 使用 RTM 创建的 ASP.NET MVC 3 应用程序版本 (不能与[ASP.NET MVC 3 Tools 更新](https://www.microsoft.com/download/details.aspx?id=1491)发行) 若要使用 ASP.NET MVC 4 并行需要以下更改。 无需在编译错误进行这些更新结果生成项目。 

    **所需的更新**

    1. 在根 Web.config 文件中，添加新 *&lt;appSettings&gt;* 具有键项*webPages:Version*和值*1.0.0.0*。 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. 在解决方案资源管理器，右键单击项目名称，然后选择卸载项目。 再次右键单击名称，然后选择编辑*ProjectName*.csproj。
    3. 找到以下程序集引用： 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        将它们替换为以下：

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. 保存所做的更改，关闭了编辑，然后右键单击项目并选择重新加载项目 (.csproj) 文件。
- **更改为目标 4.0 的 ASP.NET MVC 4 项目，从 4.5 不会更新 EntityFramework 程序集引用：**如果你的 ASP.NET MVC 4 项目后更改为目标 4.0 针对 4.5 对 EntityFramework 程序集的引用将仍指向4.5 版本。 若要解决此问题卸载并重新安装 EntityFramework NuGet 包。
- **从 4.5 更改为目标 4.0 后，在 Azure 上运行 ASP.NET MVC 4 应用程序时的 403 禁止访问：**如果你之后针对 4.5 更改为目标 4.0 的 ASP.NET MVC 4 项目并随后部署到 Azure 可能会看到在运行时一个 403 禁止访问错误。 解决此问题添加到 web.config 以下项：`<modules runAllManagedModulesForAllRequests="true" />`
- **当您键入时，visual Studio 2012 崩溃\'Razor 文件中的文本字符串中。** 若要解决此问题第一次输入的字符串文本右引号。
- **浏览到&quot;帐户/管理&quot;CHS、 TRK 和 CHT 语言运行时错误中的 Internet 模板结果中。** 若要解决此问题修改页后，可以分离出 *@User.Identity.Name* 置于内仅有的内容作为*&lt;强&gt;*标记。
- **Google 和 LinkedIn 提供程序不支持 Azure 网站中。** 在部署到 Azure 网站时，请使用备用身份验证提供程序。
- **当使用 UriPathExtensionMapping 与 IIS 8 Express/IIS，您将收到 404 未找到错误，当你尝试使用该扩展。** 静态文件处理程序到 web Api 使用的请求将会妨碍*UriPathExtensionMappings*。 设置*runAllManagedModulesForAllRequests = true*中 web.config 以解决该问题。
- **不再调用 Controller.Execute 方法。** 现在始终以异步方式执行所有的 MVC 控制器。
