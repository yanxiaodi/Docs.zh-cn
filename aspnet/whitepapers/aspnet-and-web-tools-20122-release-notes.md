---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: "ASP.NET 和 Web Tools 2012.2 的发行说明 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET 和 Web Tools 2012.2 的发行说明。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 52559a47f86e572f873d4eaaab50e87eb51722fd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET 和 Web Tools 2012.2 发行说明
====================
> 本文档介绍 ASP.NET 和 Web Tools 2012.2 发行的版本。 它是对 Visual Studio Web 工具和 ASP.NET 的更新。


- [安装说明](#_Installation)
- [文档](#_Documentation)
- [支持](#_Support)
- [软件要求](#_Software_Requirements)
- [ASP.NET 和 Web Tools 2012.2 中的新增功能](#_New_Features_in)

    - [工具](#_Tooling)
    - [Web 发布](#_Web_Publishing)
    - [ASP.NET MVC 模板](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET 友好的 Url](#_ASP.NET_Friendly_URLs)
- [已知的问题和重大更改](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>安装说明

ASP.NET 和 Web Tools 2012.2 for Visual Studio 2012 可以使用安装[Web 平台安装程序](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)。 这是对 Visual Studio 2012 或 Visual Studio Express 2012 for Web，这是必需的更新。 如果你没有安装 Visual Studio，则将安装 Visual Studio Express 2012 for Web。

你可以手动安装 ASP.NET 和 Web Tools 2012.2。 你必须具有 Visual Studio 2012 或 Visual Studio Express 2012 for Web 安装。 然后，使用以下说明： 

1. 下载[ASP.NET 和 Web 框架 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe)从下载中心安装程序。
2. 当提示的单击运行。 你还可以保存该文件以更高版本运行。
3. 验证你将更新的 Visual Studio 的版本。 可以通过启动 Visual Studio 你想要更新执行此操作。 然后单击帮助菜单项。   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. 如果你看到的菜单项&quot;有关 Microsoft Visual Studio 2012 for Web&quot;然后下载[Web 开发人员工具 2012.2-Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228)。 否则下载[Web 开发人员工具 2012.2-Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228)。
5. 当提示的单击运行。 你还可以保存该文件以更高版本运行。

> [!NOTE]
> ASP.NET 和 Web Tools 2012.2 版不包括 SQL Server Data Tools。 SQL Server 和 Windows Azure SQL 数据库提供更丰富的数据库工具包括脱机支持项目的开发，架构比较和增强的数据库部署功能集。 有关详细信息或要安装 SQL Server Data Tools，请访问[https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127)。

<a id="_Documentation"></a>
## <a name="documentation"></a>文档

ASP.NET web 站点 (https://www.asp.net) 提供了教程和有关 ASP.NET 和 Web Tools 2012.2 的其他信息。

<a id="_Support"></a>
## <a name="support"></a>支持

ASP.NET 和 Web Tools 2012.2 是正式发布，支持。 你可以使用你正常支持渠道。 此外可以将问题发布到 ASP.NET 论坛 ([https://forums.asp.net/](https://forums.asp.net/))，其中 ASP.NET 社区的成员都是经常能够提供非正式的支持。

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>软件要求

ASP.NET 和 Web Tools 2012.2 需要 Visual Studio 2012 或 Visual Studio Express 2012 for Web。

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>ASP.NET 和 Web Tools 2012.2 中的新增功能

本部分介绍已在 ASP.NET 和 Web Tools 2012.2 版本中引入的功能。

<a id="_Tooling"></a>
### <a name="tooling"></a>工具

- Page Inspector 

    - 支持 JavaScript 选择映射允许 Page inspector 设置为将动态添加到页面回发到相应的 JavaScript 代码的项映射。
    - 请参阅 CSS 的能力会在实时更新。
    - 详细信息，请阅读[CSS 自动同步和在 Page Inspector 的 JavaScript 选择映射](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx)。
- 编辑器 

    - 支持语法突出显示的 CoffeeScript、 Mustache、 车和 JsRender。
    - HTML 编辑器提供 Intellisense Knockout 绑定。
    - 较少的编辑和编译器支持启用生成使用小于动态 CSS。
    - 将 JSON 粘贴为一个.NET 类。 使用此特殊粘贴命令来将 JSON 粘贴到 C# 或 VB.NET 代码文件中和 Visual Studio 将自动生成从 JSON 推断出的.NET 类。
- 移动模拟器支持添加扩展性挂钩，以便可以作为 VSIX 安装第三方仿真程序。 安装仿真程序将显示在 F5 下拉列表中，以便开发人员可以预览自己的网站上的各种移动设备。 阅读有关在上 Scott Hanselman 的博客条目中此功能的详细信息[与 Visual Studio 的新 BrowserStack 集成](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx)。

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Web 发布

- 网站项目现在具有包括发布到 Windows Azure 网站的 Web 应用程序项目与相同的发布体验。
- 选择性发布 &#8211;一个或多个文件可以 （在发布到 Web 部署的终结点） 执行以下操作： 

    - 发布所选的文件。
    - 请参阅本地文件和远程文件之间的差异。
    - 使用远程文件更新本地文件或使用本地文件更新远程文件。

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>ASP.NET MVC 模板

- 新的 Facebook 应用程序模板可以编写简单的应用程序的 Facebook 画布。 在几个简单步骤，你可以创建的 Facebook 应用程序在已登录用户从获取数据并与他们的朋友集成。 该模板包含新的库来处理无足轻重的所有生成 Facebook 应用程序，包括身份验证，访问 Facebook 数据以及更多的权限，所涉及的管道。 使用 Facebook 应用程序模板的详细信息请参阅[https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921)。
- 新的单页面应用程序 MVC 模板允许开发人员可以构建使用 HTML 5、 CSS 3 和常用 Knockout 和 jQuery JavaScript 库，基于 ASP.NET Web API 的客户端的交互式 web 应用。 该模板包含的"todo"列表应用程序演示如何构建使用 RESTful 服务器 API 的 JavaScript HTML5 应用程序的常见做法。 你可以阅读更多信息，请访问[https://www.asp.net/single-page-application](../single-page-application/index.md)。
- 你现在可以创建 VSIX 中，以便将新模板添加到 ASP.NET MVC 新项目对话框。 此处了解操作方法： [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- FixedDisplayModes 包 （&#8211;MVC 项目模板已更新以包括新的 FixedDisplayModes NuGet 包，其中包含一个 MVC 4 中的 bug 的解决方法。 有关包中包含此修补程序的详细信息，请参阅这篇博客文章 ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) 从 MVC 团队。

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API 进行了增强的几个新功能：

- ASP.NET Web API OData
- ASP.NET Web API 跟踪
- ASP.NET Web API 帮助页

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData，可通过任何数据源生成具有丰富的业务逻辑的 OData 终结点所需的灵活性。 使用 ASP.NET Web API OData 中，您将控制你想要公开的 OData 语义的量。 ASP.NET Web API OData 是附带的 ASP.NET MVC 4 项目模板，也可以从 NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata))。

ASP.NET Web API OData 当前支持以下功能：

- 通过将 [Queryable] 特性应用启用 OData 查询语义。
- 轻松验证 OData 查询和限制的一套支持的查询选项、 运算符和函数。
- 参数绑定到 ODataQueryOptions 直接以获取的抽象语法树表示形式的查询，然后可以验证并应用于 IQueryable 或 IEnumerable。
- 通过指定 [Queryable] 属性的结果限制启用服务驱动的分页和下一步生成的页面链接。
- 请求的使用 $inlinecount 的匹配资源总数的内联的计数。
- 控制 null 传播。
- $Filter 中的任何/所有运算符。
- 按照约定推断实体数据模型或显式自定义的方式类似到实体框架代码的第一个模型。
- 公开的实体集从 EntitySetController 派生。
- 用于公开导航属性，操作链接和实现 OData 操作的简单的自定义约定。
- 简化使用 MapODataRoute 扩展方法进行路由。
- 通过公开多个 EDM 模型的版本控制支持。
- 为你的 Web API 公开的服务文档和 $metadata 以便你可以生成客户端 （.NET、 Windows Phone、 Windows 应用商店等）。
- 对 OData Atom、 JSON 和 JSON 详细格式的支持。
- 创建、 更新、 部分更新 (PATCH) 和删除实体。
- 查询和操作实体之间的关系。
- 创建连接路由到的关系链接。
- 复杂类型。
- 实体类型继承。
- 集合属性。
- 枚举。
- OData 操作。
- 作为 WCF 数据服务，即 ODataLib 相同的基础之上构建 ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata))。

有关 ASP.NET Web API OData 的详细信息请参阅[https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141)。

#### <a name="aspnet-web-api-tracing"></a>ASP.NET Web API 跟踪

ASP.NET Web API 跟踪与.NET 跟踪集成，从你的 web Api 的跟踪数据。 默认情况下，Web API 项目模板中现在启用它。 跟踪数据的你的 web Api 发送到输出窗口和通过 IntelliTrace。 ASP.NET Web API Tracing 使您能够了解你的 Web API 时通过与集成在 Windows Azure 上托管的跟踪信息[Windows Azure 诊断](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx)。 此外可以安装并启用在使用 ASP.NET Web API 跟踪 NuGet 包的任何应用程序中的 ASP.NET Web API Tracing ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing))。

有关配置和使用 ASP.NET Web API Tracing 的详细信息请参阅[https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874)。

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web API 帮助页

默认情况下，Web API 项目模板中现在包含 ASP.NET Web API 帮助页。 ASP.NET Web API 帮助页自动生成 web Api 包括 HTTP 终结点、 支持的 HTTP 方法、 参数和示例请求和响应消息负载的文档。 从代码中的注释自动拉取文档。 此外可以向任何应用程序使用 ASP.NET Web API 帮助页 NuGet 包添加 ASP.NET Web API 帮助页 ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage))。

有关详细信息来设置和自定义 ASP.NET Web API 帮助页，请参阅[https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140)。

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR 非常容易地将实时 web 功能添加到 ASP.NET 应用程序，使用 Websocket，如果可用，并自动回退到其他技术时它不可以。

有关使用 ASP.NET SignalR 的详细信息请参阅[https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271)。

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET 友好的 Url

ASP.NET FriendlyURLs 使非常简便 web 窗体开发人员能够生成清除器正在 Url 查找 （无.aspx 扩展名）。 它需要的没有配置少，并可与现有的 ASP.NET v4.0 应用程序使用。 FriendlyURLs 功能还便于开发人员能够将移动支持添加到其应用程序，通过支持桌面和移动视图之间切换。

安装和使用 ASP.NET 友好 Url 的详细信息请参阅[http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)。

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

本部分介绍已知的问题和 ASP.NET 和 Web Tools 2012.2 版本中的重大更改。

### <a name="installation-issues"></a>安装问题

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>无序安装的 Visual Studio 2012

在安装 ASP.NET 和 Web Tools 2012.2 需要修复操作之后，请安装其他 SKU 的 Visual Studio 2012。 请考虑按以下顺序：

1. 安装 Visual Studio 2012 Express for Web
2. 安装 ASP.NET 和 Web Tools 2012.2
3. 安装 Visual Studio 2012 Professional、 Premium 或 Ultimate

步骤 2 将仅导致为 Express for Web 安装更新。 若要确保在步骤 3 中安装的其他 SKU 包含更新将需要修复 ASP.NET 和 Web Tools 2012.2 安装的最后一个 sku 安装更新。 如果在步骤 1 中的 Sku 和 3 会反转也适用。

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>打开 Visual Studio 时，请安装 Microsoft ASP.NET 和 Web Tools 2012.2

如果在安装的 Microsoft ASP.NET 和 Web Tools 2012.2 过程打开 VS，Visual Studio 可能结束处于错误的状态。 建议用户关闭继续安装之前的 Visual Studio 的所有实例。

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>如果取消安装 ASP.NET 和 Web Tools 2012.2 的安装程序

取消 ASP.NET 和 Web Tools 2012.2 安装的安装程序将处于错误状态保留 Visual Studio。 若要解决此问题，请执行以下步骤： 

- 转到添加/删除程序
- 如果存在，请卸载 Microsoft ASP.NET 和 Web Tools 2012.2。
- 重新安装 Microsoft ASP.NET 和 Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>在卸载 ASP.NET 和 Web Tools 2012.2 ASP.NET MVC 4 后模板和 Razor v2 网站模板是缺少

卸载 ASP.NET 和 Web Tools 2012.2 则也将卸载所有的 ASP.NET MVC 4 和 Razor v2 网站模板从 Visual Studio 2012。

解决方法是来修复 Visual Studio 2012 安装用于重新安装 ASP.NET MVC 4 和 Razor v2 网站模板。

### <a name="tooling-issues"></a>工具问题

#### <a name="nuget-error-reported-during-project-creation"></a>在项目创建期间报告的 NuGet 错误

在安装 ASP.NET 和 Web Tools 2012.2 你可能会看到以下错误时创建的 MVC 4 项目

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

ASP.NET 和 Web Tools 2012.2 附带 NuGet 2.1，并将升级 Visual Studio 2012 中的扩展。 在某些情况下，将无法正确更新 VSIX VSIX 安装程序。 以下步骤将允许你解决此问题：

1. 以管理员身份启动 Visual Studio 2012
2. 转到 Tools-&gt;扩展和更新和卸载 NuGet。
3. 关闭 Visual Studio
4. 导航到 ASP.NET 和 Web Tools 2012.2 安装文件夹：

    1. 对于 Visual Studio 2012:**程序 Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. 有关 Visual Studio 2012 Express for Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**
5. 双击 NuGet.Tools.vsix 重新安装 NuGet

### <a name="web-api-issues"></a>Web API 问题

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>分析 $filter 和日期时间文本中的问题

OData URI 分析器无法正确分析部分的日期时间文本中。 例如，$filter = 开始 eq 日期时间"2012年-12-31T12:00 无法正确分析。 一种解决方法是使用完整文本 $filter = 开始 eq 日期时间"2012年-12-31T12:00:00。

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData 不支持不区分大小写的属性名称。

OData 不支持 OData 查询和 odata 路径中的不区分大小写的属性名称。 工作项，请参阅：

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

如果用户具有 javascript 客户端和服务器端上的不同大小写，它们可能会遇到此问题。 此问题是 odata 协议中的设计。 但是，许多用户会报告此问题。 若要解决这个问题，用户必须更正它们在 URL 中的用例。

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>默认 OData 路由约定不支持 POST/PUT 导航属性。

默认 OData 路由约定不支持 POST/PUT 导航属性。 请参阅工作项[http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)。 我们缺少默认的约定中此常用的约定。

若要解决这个问题，用户需要扩展新种路由约定无法支持它。

### <a name="facebook-template-issues"></a>Facebook 模板问题

#### <a name="facebook-application-template-only-works-using-net-45"></a>Facebook 应用程序模板只能是使用.NET 4.5

你必须在新项目对话框以查看 ASP.NET MVC 4 中的 Facebook 应用程序模板中的框架下拉列表中选择.NET 4.5。

#### <a name="real-time-update-controller"></a>实时更新控制器

Facebook 应用程序模板允许用户轻松地创建一个 Web API 控制器来处理从 Facebook 的实时更新。 如果你的开发计算机位于 NAT 后面，你的控制器可能无法工作而无需进一步网络配置。 有关详细信息请参阅本文： [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>查询与 Facebook OAuth 参数的字符串值冲突

以下字段冲突 Facebook OAuth 对话框中的调用返回 URL。 不添加你自己的查询字符串值具有以下名称： 代码、 错误、 错误\_说明、 错误\_原因。

#### <a name="using-page-inspector-with-facebook-template"></a>Page Inspector 使用 Facebook 模板

在调试 Facebook 应用程序时，不能在 Visual Studio 2012 中使用 Page Inspector 功能。 Page Inspector 当前不支持 iframe。

### <a name="single-page-application-template-issues"></a>单页面应用程序模板问题

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>使用 JQuery 1.9/Knockout 2.2.1 更新，运行默认 MVC SPA 项目中，新的 todo 项编辑时输入焦点事件不正确处理。

与 JQuery 1.9/Knockout 2.2.1 更新，当运行默认 MVC SPA 项目时，新的 todo 项编辑不再焦点回新的 todo 项编辑框后输入到 todo 列表中输入新的 todo 项。

解决方法引用[http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)，并对下面的示例代码进行类似的修复：

文件 todo.model.js  
 函数 todolist(data) 中，添加以下：  
 **self.isSelected = ko.observable(false);**

函数 todoList.prototype.addTodo 中，添加以下 blacked 的文本：  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Index.cshtml 文件中，添加以下 blacked 的文本：  
 &lt;窗体数据绑定 =&quot;提交： addTodo&quot;&gt;  
 &lt;输入类 =&quot;addTodo&quot;类型 =&quot;文本&quot;数据绑定 =&quot;值： newTodoTitle，占位符: Type 此处以添加、 blurOnEnter： 为 true，**有焦点： isSelected**，事件: {模糊： addTodo}&quot; /&gt;  
 &lt;/form&gt;
