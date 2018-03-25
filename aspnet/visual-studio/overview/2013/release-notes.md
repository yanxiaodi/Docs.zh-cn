---
uid: visual-studio/overview/2013/release-notes
title: "ASP.NET 和 Web Tools for Visual Studio 2013 发行说明 |Microsoft 文档"
author: microsoft
description: "本文档介绍 ASP.NET 和 Web Tools for Visual Studio 2013 的版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 7f38a0f2693aeb2a4884b9c03719b583423957a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET 和 Web Tools for Visual Studio 2013 发行说明
====================
by [Microsoft](https://github.com/microsoft)

> 本文档介绍 ASP.NET 和 Web Tools for Visual Studio 2013 的版本。


## <a name="contents"></a>内容

- [安装说明](#TOC1)
- [文档](#TOC2)
- [软件要求](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET 和 Web Tools for Visual Studio 2013 中的新增功能

- [一个 ASP.NET](#TOC6)
- [新的 Web 项目体验](#newproj)
- [ASP.NET 基架](#scaffold)
- [浏览器链接](#browser-link)
- [Visual Studio Web 编辑器增强功能](#web-editor)
- [Visual Studio 中的 azure App Service Web Apps 支持](#waws)
- [Web 发布增强功能](#publish)
- [NuGet 2.7](#nuget)
- [ASP.NET Web 窗体](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [ASP.NET 标识](#TOC8)
- [Microsoft OWIN 组件](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [ASP.NET 应用挂起](#TOC15)
- [已知的问题和重大更改](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>安装说明

ASP.NET 和 Web Tools for Visual Studio 2013 在主安装程序中捆绑在一起，可以下载[此处](https://www.asp.net/downloads)。

<a id="TOC2"></a>
## <a name="documentation"></a>文档

教程和有关 ASP.NET 和 Web Tools for Visual Studio 2013 的其他信息可以从[ASP.NET 网站](https://www.asp.net/)。

<a id="TOC4"></a>
## <a name="software-requirements"></a>软件要求

ASP.NET 和 Web 工具需要 Visual Studio 2013。

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>ASP.NET 和 Web Tools for Visual Studio 2013 中的新增功能

以下各节描述了已在版本中引入的功能。

<a id="TOC6"></a>
## <a name="one-aspnet"></a>一个 ASP.NET

在 Visual Studio 2013 的版本中，我们已经实现统一的体验，以便可以轻松混合和匹配的选出您希望使用 ASP.NET 技术的步。 例如，你可以启动使用 MVC 的项目和轻松地更高版本，向项目中添加 Web 窗体页或 Web 窗体项目中创建的 Web Api 基架。 一个 ASP.NET 是核心重点更容易地为你作为开发人员如何在 ASP.NET 中喜欢的内容。 无论你选择哪种技术，你可以要在一个 ASP.NET 的受信任基础框架生成的置信度。

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>新的 Web 项目体验

我们已增强，在 Visual Studio 2013 中创建新的 web 项目的体验。 在**新的 ASP.NET Web 项目**对话框，你可以选择想、 配置技术 (Web 窗体，MVC、 Web API) 的任意组合，配置身份验证选项和添加单元测试项目的项目类型。

![新建 ASP.NET 项目](release-notes/_static/image1.png)

新建对话框可以更改模板中的许多的默认身份验证选项。 例如，创建 ASP.NET Web 窗体项目时可以选择任意下列选项：

- 无身份验证
- 单个用户帐户 （ASP.NET 成员资格或社交提供程序登录）
- 组织帐户 (internet 应用程序中的 Active Directory)
- Windows 身份验证 (intranet 应用程序中的 Active Directory)

![身份验证选项](release-notes/_static/image2.png)

有关用于创建 web 项目的新过程的详细信息，请参阅[在 Visual Studio 2013 中创建 ASP.NET Web 项目](creating-web-projects-in-visual-studio.md)。 有关新的身份验证选项的详细信息，请参阅[ASP.NET 标识](#TOC8)本文档后面部分。

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>ASP.NET 基架

ASP.NET 基架是用于 ASP.NET Web 应用程序的代码生成框架。 它可以轻松将样板文件代码添加到您的项目，与数据模型进行交互。

在以前版本的 Visual Studio 中，基架只限于 ASP.NET MVC 项目。 使用 Visual Studio 2013，现在可以用于任何 ASP.NET 项目，包括 Web 窗体使用基架。 Visual Studio 2013 当前不支持生成页对于 Web 窗体项目，但你仍可以使用基架，Web 窗体通过向项目添加 MVC 依赖项。 将在将来更新中添加对生成的 Web 窗体页支持。

在使用基架，我们确保所有必需的项目中安装依赖关系。 例如，如果您首先 ASP.NET Web 窗体项目，然后使用基架添加一个 Web API 控制器，所需的 NuGet 程序包和引用将自动添加到你的项目。

若要将 MVC 基架添加到 Web 窗体项目，添加**新建基架项**和选择**MVC 5 依赖项**对话框窗口中。 有两个选项的基架 MVC;最小和完整。 如果你选择最小，只有的 NuGet 包和 ASP.NET MVC 的引用添加到项目中。 如果选择完整选项，添加最小依赖关系，以及对于 MVC 项目所需的内容文件。

对基架异步控制器支持使用 Entity Framework 6 中的新异步功能。

有关详细信息和教程，请参阅[ASP.NET 基架概述](aspnet-scaffolding-overview.md)。

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>浏览器链接-浏览器和 Visual Studio 之间的 SignalR 通道

新[Browser Link](using-browser-link.md)功能可让你连接到 Visual Studio 的多个浏览器并刷新它们所有通过单击工具栏中的按钮。 可以将多个浏览器连接到开发站点，其中包括移动模拟器，并单击全部在同一时间刷新到刷新所有浏览器。 浏览器链接还公开的 API，使开发人员编写 Browser Link 扩展。

![](release-notes/_static/image3.png)

通过使开发人员能够利用浏览器链接 API，它将成为可以创建跨边界的 Visual Studio 和任何连接的浏览器之间的非常高级的方案。 Web Essentials 利用 API 创建 Visual Studio 和浏览器的开发人员工具，远程控制移动模拟器和更多操作之间的集成的体验。

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web 编辑器增强功能

Visual Studio 2013 web 应用程序中包括新的 HTML 编辑器中为 Razor 文件和 HTML 文件。 新的 HTML 编辑器提供了基于 HTML5 的单个统一的架构。 它包含自动括号补全、 jQuery UI 和 AngularJS 特性 IntelliSense，特性 IntelliSense 分组、 ID 和类名 Intellisense、 和其他改进包括更好的性能，格式设置和智能标记。

下面的屏幕截图演示如何使用在 HTML 编辑器中的 Bootstrap 特性 IntelliSense。

![HTML 编辑器中的 Intellisense](release-notes/_static/image4.png)

Visual Studio 2013 还附带与这两个 CoffeeScript 和更低的内置的编辑器。 LESS 编辑器与所有冷的功能来自 CSS 编辑器，而在中的所有更少文档中具有特定 Intellisense 变量和 mixins@import链。

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Visual Studio 中的 azure App Service Web Apps 支持

在 Azure SDK for.NET 2.2 与 Visual Studio 2013，你可以使用**服务器资源管理器**直接与你的远程 web 应用进行交互。 你可以登录到你的 Azure 帐户、 创建新 web 应用、 配置应用、 查看实时日志等等。 SDK 2.2 之后不久即将发布，你将能够在 Azure 中远程调试模式下运行。 当你安装最新版本的 Azure sdk for.NET 时，大部分 Azure App Service Web Apps 的新功能也适用于 Visual Studio 2012。

有关更多信息，请参见以下资源：

- [在 Azure App Service 中创建 ASP.NET web 应用](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [对 Azure App Service 中使用 Visual Studio 中的 web 应用进行故障排除](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Web 发布增强功能

Visual Studio 2013 包含的新功能和增强 Web 发布功能。 下面是其中一些：

- 轻松[自动执行 Web.config 文件加密](https://go.microsoft.com/fwlink/?LinkId=325529)。 （此链接和以下两个点可能不可用之前在 10/17 当天的后期的 MSDN 上的文档。）
- 轻松[自动采用应用程序脱机在部署过程中](https://go.microsoft.com/fwlink/?LinkId=325530)。
- 配置 Web 部署到[而上次更改日期不是使用文件校验和](https://go.microsoft.com/fwlink/?LinkId=325531)用于确定哪些文件应复制到服务器。
- 要使用 FTP 或文件系统发布方法时，以及使用 Web 部署，快速发布单个所选的文件 （包括 Web.config）。

有关 ASP.NET web 部署的详细信息，请参阅[ASP.NET 站点](https://go.microsoft.com/fwlink/?LinkId=322027)。

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 详细的介绍包含一组丰富的新的特征描述了这些[NuGet 2.7 发行说明](http://docs.nuget.org/docs/release-notes/nuget-2.7)。

此版本的 NuGet 还会删除，需要为提供明确同意的 NuGet 包还原功能，若要下载包。 现在通过安装 NuGet 授予同意的情况下 （和 NuGet 的首选项对话框中的关联复选框）。 现在程序包还原只需默认的工作方式。

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web 窗体

### <a name="one-aspnet"></a>一个 ASP.NET

Web 窗体项目模板与新的一个 ASP.NET 体验无缝集成。 你可以添加 MVC 和 Web API 支持为 Web 窗体项目，并且你可以配置身份验证使用一个 ASP.NET 项目创建向导。 有关详细信息，请参阅[在 Visual Studio 2013 中创建 ASP.NET Web 项目](creating-web-projects-in-visual-studio.md)。

### <a name="aspnet-identity"></a>ASP.NET 标识

Web 窗体项目模板支持新的 ASP.NET Identity framework。 此外，模板现在支持创建 Web 窗体 intranet 项目。 有关详细信息，请参阅[身份验证方法](creating-web-projects-in-visual-studio.md#auth)中**在 Visual Studio 2013 中创建 ASP.NET Web 项目**。

### <a name="bootstrap"></a>Bootstrap

Web 窗体模板使用[Bootstrap](http://twitter.github.io/bootstrap/)提供作为时尚且高度可响应外观和感觉，你可以轻松自定义。 有关详细信息，请参阅[Visual Studio 2013 web 项目模板中的 Bootstrap](creating-web-projects-in-visual-studio.md#bootstrap)。

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>一个 ASP.NET

Web MVC 项目模板与新的一个 ASP.NET 体验无缝集成。 可以自定义 MVC 项目，并使用一个 ASP.NET 项目创建向导配置身份验证。 处找不到 ASP.NET MVC 5 的介绍性教程[Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

有关升级到 MVC 5 的 MVC 4 项目的信息，请参阅[如何将 ASP.NET MVC 4 和 Web API 项目升级到 ASP.NET MVC 5 和 Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。

### <a name="aspnet-identity"></a>ASP.NET 标识

MVC 项目模板已更新为使用 ASP.NET 标识进行身份验证和标识管理。 处找不到具有 Facebook 和 Google 身份验证和新的成员资格 API 教程，说明如何[创建 ASP.NET MVC 5 应用程序使用 Facebook 和 Google OAuth2 和 OpenID 登录](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)和[使用身份验证创建的 ASP.NET MVC 应用程序和SQL 数据库并将其部署到 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)。

### <a name="bootstrap"></a>Bootstrap

MVC 项目模板已更新为使用[Bootstrap](http://getbootstrap.com/)提供作为时尚且高度可响应外观和感觉，你可以轻松自定义。 有关详细信息，请参阅[Visual Studio 2013 web 项目模板中的 Bootstrap](creating-web-projects-in-visual-studio.md#bootstrap)。

### <a name="authentication-filters"></a>身份验证筛选器

身份验证筛选器是一种新的筛选器在 ASP.NET MVC，在 ASP.NET MVC 管道中的授权筛选器之前运行并且使您能够指定身份验证逻辑每个操作，每个控制器或全局范围内的所有控制器。 身份验证筛选器处理请求中的凭据，并提供相应的用户权限。 身份验证筛选器还可以添加身份验证质询响应未经授权的请求。

### <a name="filter-overrides"></a>筛选器将重写

现在，你可以重写筛选器应用于给定的操作方法或控制器通过指定一个替代的筛选器。 重写筛选器指定一的组不应为给定作用域 （操作或控制器） 运行的筛选器类型。 这允许您配置执行全局应用，但然后从将应用于特定操作或控制器中排除某些全局筛选器的筛选器。

### <a name="attribute-routing"></a>属性路由

ASP.NET MVC 现在支持的属性路由，感谢通过 Tim McCall 的作者贡献[http://attributerouting.net](http://attributerouting.net)。 使用的属性路由可以对你的操作和控制器进行批注来指定路由。

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>属性路由

ASP.NET Web API 现在支持的属性路由，感谢通过 Tim McCall 的作者贡献[http://attributerouting.net](http://attributerouting.net)。 使用的属性路由，您可以指定 Web API 路由进行批注你的操作和控制器如下：

[!code-csharp[Main](release-notes/samples/sample1.cs)]

属性路由可更好地控制 Uri 在你的 web API。 例如，你可以轻松地定义资源层次结构中使用一个 API 控制器：

[!code-csharp[Main](release-notes/samples/sample2.cs)]

属性的路由还提供一种方便语法，用于指定可选参数、 默认值和路由约束：

[!code-csharp[Main](release-notes/samples/sample3.cs)]

有关的属性路由的详细信息，请参阅[Web API 2 中的属性路由](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md)。

### <a name="oauth-20"></a>OAuth 2.0

Web API 和单页面应用程序项目模板现在支持使用 OAuth 2.0 的授权。 OAuth 2.0 是一个框架，用于授予对受保护资源的客户端访问权限。 它适用于各种客户端，包括浏览器和移动设备。

OAuth 2.0 的支持取决于新安全中间件持有者身份验证提供的 Microsoft OWIN 组件和实现授权服务器角色。 或者，可以使用组织的授权服务器，如 Azure Active Directory 或 Windows Server 2012 R2 中的 ad FS 授权客户端。

### <a name="odata-improvements"></a>OData 改进

**支持 $select，$ 展开，$batch 和 $value**

ASP.NET Web API OData 现在有 $select 的完全支持、 $expand、 和 $value。 你可以还 $batch 用于批处理和处理变更集的请求。

$Select 和 $展开选项使您可以更改从 OData 终结点返回的数据的形状。 有关详细信息，请参阅[简介 $select 和 $展开 Web API OData 中的支持](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md)。

**改进的可扩展性**

OData 格式化程序现在是可扩展的。 可以添加 Atom 条目元数据、 支持命名的流和媒体链接项、 添加实例批注和自定义链接的生成方式。

**类型的支持**

现在可以生成 OData 服务，而无需定义实体类型的 CLR 类型。 相反，你的 OData 控制器可以使用或不返回的实例**IEdmObject**，后者是 OData 格式化程序序列化/反序列化。

**重用现有模型**

如果你已有现有的实体数据模型 (EDM)，你可以现在重用它直接，而无需生成一个新。 例如，如果使用实体框架，你可以使用为您建立 EF EDM。

### <a name="request-batching"></a>请求批处理

请求批处理将合并到单个 HTTP POST 请求，以减少网络流量，并提供流畅些，小于聊天式用户界面的多个操作。 ASP.NET Web API 现在支持用于请求批处理的几种策略：

- 使用 OData 服务的 $batch 终结点。
- 打包成单个多部分 MIME 请求多个请求。
- 使用自定义批处理格式。

若要启用批处理的请求，只需将批处理的处理程序的路由添加到你的 Web API 配置：

[!code-csharp[Main](release-notes/samples/sample4.cs)]

此外可以控制是否请求或按顺序或按任意顺序执行。

### <a name="portable-aspnet-web-api-client"></a>可移植的 ASP.NET Web API 客户端

现在可以使用 ASP.NET Web API 客户端要在 Windows 应用商店和 Windows Phone 8 应用程序之间创建的工作的可移植类库。 你还可以创建可在客户端和服务器之间共享的可移植格式化程序。

### <a name="improved-testability"></a>改进的可测试性

Web API 2 使它更容易单元测试你的 API 控制器。 只需实例化请求消息和配置，你的 API 控制器，然后调用你想要测试的操作方法。 它很容易模拟**UrlHelper**类，用于执行链接生成的操作方法。

### <a name="ihttpactionresult"></a>IHttpActionResult

现在，可以实现 IHttpActionResult 来封装你 Web API 的操作方法的结果。 从 Web API 操作方法返回 IHttpActionResult 执行由 ASP.NET Web API 运行时来生成结果的响应消息中。 可以从任何 Web API 操作来简化单元返回 IHttpActionResult 测试你的 Web API 实现。 为方便起见多 IHttpActionResult 实现提供在初始状态下包括用于返回特定的状态代码的结果，格式的内容或内容协商响应。

### <a name="httprequestcontext"></a>HttpRequestContext

新**HttpRequestContext**跟踪限于请求而不是从请求立即可用的任何状态。 例如，你可以使用**HttpRequestContext**获取路由数据，与请求中，客户端证书，关联的主体**UrlHelper**和根虚拟路径。 你可以轻松创建**HttpRequestContext**为单元测试。

因为请求的主体进行流处理请求而不是依靠**Thread.CurrentPrincipal**，Web API 管道中时，主体现在是可用的整个生存期内请求。

### <a name="cors"></a>CORS

感谢从 Brock Allen 的另一个良好发布内容，ASP.NET 现在完全支持跨域请求共享 (CORS)。

浏览器安全阻止到另一个域进行 AJAX 请求 web 页。 [CORS](http://www.w3.org/TR/cors/)是一种 W3C 标准允许放宽同源策略的服务器。 使用 CORS，服务器可以显式允许某些跨源请求时拒绝其他人。

Web API 2 现在支持 CORS，包括自动处理的预检请求。 有关详细信息，请参阅[ASP.NET Web API 中启用跨域请求](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md)。

### <a name="authentication-filters"></a>身份验证筛选器

身份验证筛选器是一种新的 ASP.NET Web API 管道中的授权筛选器之前运行并且使您能够指定身份验证逻辑每个操作，ASP.NET Web API 中的筛选器的每个控制器或全局范围内的所有控制器。 身份验证筛选器处理请求中的凭据，并提供相应的用户权限。 身份验证筛选器还可以添加身份验证质询响应未经授权的请求。

### <a name="filter-overrides"></a>筛选器替代

现在，你可以重写筛选器应用于给定的操作方法或控制器，通过指定一个替代的筛选器。 重写筛选器指定一的组不应为给定作用域 （操作或控制器） 运行的筛选器类型。 这样，你可以添加全局筛选器，但然后排除一些特定操作或控制器。

### <a name="owin-integration"></a>OWIN 集成

ASP.NET Web API 现在完全支持 OWIN，并可以在任何 OWIN 支持主机上运行。 此外包含**HostAuthenticationFilter** ，它提供与 OWIN 身份验证系统的集成。

通过 OWIN 集成，你可以自承载 Web API，在与其他 OWIN 中间件，例如 SignalR 一起过程中。 有关详细信息，请参阅[使用 OWIN 自承载 ASP.NET Web API 来](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

下列各节描述 SignalR 2.0 的功能。

- [基于 OWIN](#builtonowin)
- [MapHubs 和 MapConnection 现 MapSignalR](#MapSignalR)
- [跨域支持](#crossdomain)
- [iOS 和 Android 支持通过 MonoTouch 和 MonoDroid](#mobile)
- [可移植.NET 客户端](#portable)
- [新的自承载的包](#selfhost)
- [向后兼容服务器支持](#backwardcompat)
- [删除 server 对.NET 4.0 的支持](#remove40)
- [将一条消息发送到客户端和组的列表](#messagelist)
- [向特定用户发送消息](#sendtouser)
- [更好的错误处理支持](#errorhandling)
- [更轻松的单元测试的中心](#unittesting)
- [JavaScript 错误处理](#javascripterror)

有关如何将现有 1.x 项目升级到 SignalR 2.0 的示例，请参阅[升级 SignalR 1.x 项目](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md)。

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>基于 OWIN

完全上创建 SignalR 2.0 [OWIN （适用于.NET 的打开 Web 接口）](http://owin.org/)。 此更改使适用于 SignalR 的安装过程更 web 承载和自承载 SignalR 应用程序之间保持一致，但也需要大量的 API 更改此标识。

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs 和 MapConnection 现 MapSignalR

为了与 OWIN 标准兼容，这些方法具有已重命名为`MapSignalR`。 `MapSignalR`调用没有参数将映射所有中心 (作为`MapHubs`在版本 1.x); 如果要都映射单独**PersistentConnection**对象，指定连接类型作为类型参数中，和作为连接的 URL 扩展第一个参数。

`MapSignalR` Owin 启动类中调用方法。 Visual Studio 2013 包含的 Owin 启动类; 的新模板若要使用此模板，请执行以下操作：

1. 右键单击该项目
2. 选择**添加**，**新建项...**
3. 选择**Owin 启动类**。 将新类**Startup.cs**。

在**Web 应用程序，** Owin 启动类包含`MapSignalR`方法然后添加到 Owin 的启动过程中的 Web.Config 文件中，应用程序设置节点中使用的项，如下所示。

在**自承载应用程序**，Startup 类传递的类型参数作为`WebApp.Start`方法。

**映射中心和在 SignalR 连接 1.x （从 web 应用程序中的全局应用程序文件）：** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**映射中心和 SignalR 2.0 （从 Owin 启动类文件） 中的连接：** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

在**自承载应用程序**，Startup 类传递的类型参数作为`WebApp.Start`方法，如下所示。

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>跨域支持

在 SignalR 1.x，跨域请求已由单个 EnableCrossDomain 标志控制。 此标志控制 JSONP 和 CORS 请求。 所有 CORS 都支持更大的灵活性，已从 SignalR 的服务器组件 （JavaScript lients 仍使用 CORS 通常如果检测到由浏览器支持它），和新的 OWIN 中间件已可用于都支持这些方案。

在 SignalR 2.0 中，如果 JSONP 客户端上 （支持需要跨域请求在较旧的浏览器中），它将需要通过设置显式启用`EnableJSONP`上`HubConfiguration`对象传递给`true`，如下所示。 JSONP 已禁用默认情况下，因为它比 CORS 不太安全。

若要添加新的 CORS 中间件 SignalR 2.0 中，添加`Microsoft.Owin.Cors`库连接到你的项目，然后调用`UseCors`之前 SignalR 中间件，如下面的部分中所示。

**向项目中添加 Microsoft.Owin.Cors**： 若要安装此库，包管理器控制台中运行以下命令：

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

此命令将添加 2.0.0 到你的项目的包版本。

**调用 UseCors**

以下代码段演示如何在 SignalR 中实现跨域连接 1.x 和 2.0 版。

**在 SignalR 中实现跨域请求 1.x （从全局应用程序文件中）**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**在 SignalR 2.0 （来自 C# 代码文件） 中实现跨域请求**

下面的代码演示如何在 SignalR 2.0 项目中启用 CORS 或 JSONP。 此代码示例使用`Map`和`RunSignalR`而不是`MapSignalR`，以便 CORS 中间件运行仅对于需要 CORS 支持 SignalR 请求 (而不是对在指定的路径上的所有流量`MapSignalR`。)`Map`还用于运行特定的 URL 前缀，而不是整个应用程序所需要的任何其他中间件。

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS 和 Android 支持通过 MonoTouch 和 MonoDroid

已为 iOS 和 Android 客户端都使用 MonoTouch 和 MonoDroid 组件添加支持[Xamarin 库](https://xamarin.com/)。 有关如何使用它们的详细信息，请参阅[使用 Xamarin 组件](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln)。 这些组件可在[Xamarin 应用商店](https://store.xamarin.com/)SignalR RTW 版本时可用。

<a id="portable"></a># # # 可移植.NET 客户端

更好地便于实现跨平台开发、 Silverlight、 WinRT 并 Windows Phone 客户端已替换为单个可移植.NET 客户端支持以下平台：

- NET 4.5
- Silverlight 5
- WinRT (Windows 应用商店应用的.NET)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>新的自承载的包

现在有了 NuGet 包，以使其更轻松地开始使用 SignalR 自承载 （的进程中承载的 SignalR 应用程序或其他应用程序，而非托管的 web 服务器中）。 若要升级使用 SignalR 构建的自承载项目 1.x，删除 Microsoft.AspNet.SignalR.Owin 包，并添加 Microsoft.AspNet.SignalR.SelfHost 包。 有关如何开始使用的自承载的包的详细信息，请参阅[教程： 自承载的 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>向后兼容服务器支持

在以前版本的 SignalR，需要进行相同的服务器和使用在客户端的 SignalR 包的版本。 为了支持粗客户端应用程序将会很难更新，SignalR 2.0 现在支持使用旧版客户端的较新的服务器版本。 **注意： SignalR 2.0 不支持与较新的客户端生成较旧版本的服务器。**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>删除 server 对.NET 4.0 的支持

SignalR 2.0 已删除，对与.NET 4.0 的服务器互操作性的支持。 与 SignalR 2.0 服务器，必须使用.NET 4.5。 仍是 SignalR 2.0 的.NET 4.0 客户端。

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>将一条消息发送到客户端和组的列表

在 SignalR 2.0 中，则可以使用客户端和组 Id 的列表发送邮件。 下面的代码段演示如何执行此操作。

**将一条消息发送到客户端和使用 PersistentConnection 组的列表**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**将一条消息发送到客户端和使用中心的组的列表**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>向特定用户发送消息

此功能允许用户指定用户 Id 是基于通过新接口 IUserIdProvider IRequest:

**IUserIdProvider 接口**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

默认情况下将使用作为用户名的用户的 IPrincipal.Identity.Name 的实现。

在中心，你将能够将消息发送到一个新的 API 通过这些用户：

**使用 Clients.User API**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>更好的错误处理支持

用户可以立即引发**HubException**从任何中心调用。 构造函数**HubException**可以采用字符串消息和一个对象额外的错误数据。 SignalR 将自动序列化异常，并将其发送到客户端，它将用于拒绝/失败中心方法调用。

**显示详细的中心异常**设置没有任何影响**HubException**发送回客户端或不; 它始终发送。

**演示将 HubException 发送到客户端的服务器端代码**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**演示对从服务器发送 HubException 作出响应的 JavaScript 客户端代码**

[!code-html[Main](release-notes/samples/sample16.html)]

**演示对从服务器发送 HubException 作出响应的.NET 客户端代码**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>更轻松的单元测试的中心

SignalR 2.0 包括一个接口，称为`IHubCallerConnectionContext`上轻松地创建端调用的模拟客户端的中心。 下面的代码段演示如何使用常用的测试工具使用此接口[xUnit.net](https://github.com/xunit/xunit)和[moq](https://code.google.com/p/moq/)。

**使用单元测试 SignalR xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**使用单元测试 SignalR moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>JavaScript 错误处理

在 SignalR 2.0 中，所有 JavaScript 错误处理回调都返回而不是原始字符串的 JavaScript 错误对象。 这允许 SignalR 流动到错误处理程序更加丰富的信息。 你可以从内部异常`source`的错误的属性。

**处理 Start.Fail 异常的 JavaScript 客户端代码**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET 标识

### <a name="new-aspnet-membership-system"></a>新的 ASP.NET 成员资格系统

ASP.NET 标识是为 ASP.NET 应用程序的新成员身份系统。 ASP.NET 标识容易地将特定于用户的配置文件的数据集成与应用程序数据。 ASP.NET 标识还可选择你的应用程序中的用户配置文件的持久性模型。 你可以将数据存储在 SQL Server 数据库或其他数据存储，包括 Azure 存储表等 NoSQL 数据存储区。 有关详细信息，请参阅[单个用户帐户](creating-web-projects-in-visual-studio.md#indauth)中**在 Visual Studio 2013 中创建 ASP.NET Web 项目**。

### <a name="claims-based-authentication"></a>基于声明的身份验证

ASP.NET 现在支持基于声明的身份验证，其中用户的标识表示为一组从受信任的颁发者的声明。 用户可以是经过身份验证的使用用户名和密码在应用程序数据库中维护或使用社交标识提供程序 (例如： Microsoft 帐户、 Facebook、 Google、 Twitter)，或使用通过 Azure Active Directory 的组织帐户或Active Directory 联合身份验证服务 (ADFS)。

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>与 Azure Active Directory 和 Windows Server Active Directory 集成

你现在可以创建使用 Azure Active Directory 或 Windows Server Active Directory (AD) 进行身份验证的 ASP.NET 项目。 有关详细信息，请参阅[组织帐户](creating-web-projects-in-visual-studio.md#orgauth)中**在 Visual Studio 2013 中创建 ASP.NET Web 项目**。

### <a name="owin-integration"></a>OWIN 集成

ASP.NET 身份验证现在基于可在任何基于 OWIN 的主机的 OWIN 中间件。 有关 OWIN 的详细信息，请参阅以下[Microsoft OWIN 组件](#TOC7)部分。

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Microsoft OWIN 组件

[打开针对.NET 的 Web 界面](http://owin.org/)(OWIN) 定义.NET web 服务器和 web 应用程序之间的抽象。 OWIN 将脱耦 web 应用程序在服务器上，使 web 应用程序主机无关。 例如，你可以托管在 IIS 中基于 OWIN 的 web 应用程序或自承载服务中自定义过程。

Microsoft OWIN 组件 （也称为 Katana 项目） 中引入的更改包括新的服务器和主机组件、 新的帮助程序库和中间件和新的身份验证中间件。

有关 OWIN 和 Katana 的详细信息，请参阅[OWIN 和 Katana 中的新增](../../../aspnet/overview/owin-and-katana/index.md)。

**注意： [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)应用程序无法在 IIS 经典模式下运行; 必须在集成模式中运行时。**

**注意： [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)应用程序必须运行在完全信任。**

### <a name="new-servers-and-hosts"></a>新的服务器和主机

此版本中，添加了新组件以启用自承载方案。 这些组件包括以下 NuGet 包：

- **Microsoft.Owin.Host.HttpListener**. 提供使用 OWIN 服务器**HttpListener**以侦听 HTTP 请求并将它们定向到 OWIN 管道。
- **Microsoft.Owin.Hosting**适用于想要自承载于自定义过程中，如控制台应用程序或 Windows 服务的一个 OWIN 管道开发人员提供了一个库。
- **OwinHost**。 提供独立的可执行文件，用于包装`Microsoft.Owin.Hosting`，可以在自承载 OWIN 管道，而无需编写自定义主机应用程序。

此外，`Microsoft.Owin.Host.SystemWeb`包现在可让中间件来提供对提示**SystemWeb**服务器，它指示应在特定的 ASP.NET 管道阶段调用中间件。 此功能很适合用于身份验证中间件，应尽早在 ASP.NET 管道中运行。

### <a name="helper-libraries-and-middleware"></a>帮助程序库和中间件

尽管您可以编写 OWIN 组件使用仅中的函数和类型定义 OWIN 规范中，新`Microsoft.Owin`包提供了抽象更加友好的用户组。 此产品包结合了多个更早版本的包 (例如， `Owin.Extensions`， `Owin.Types`) 到单一、 明确的结构化对象模型，然后可以由其他 OWIN 组件轻松地使用。 事实上，Microsoft OWIN 组件的大部分现在使用此包。

> [!NOTE]
> [OWIN](http://www.owin.org)应用程序无法在 IIS 经典模式下运行; 必须在集成模式中运行时。

> [!NOTE]
> [OWIN](http://www.owin.org)应用程序必须运行在完全信任。

此版本还包括 Microsoft.Owin.Diagnostics 程序包，其中包括中间件验证正在运行的 OWIN 应用程序，以及有助于调查失败的错误页中间件。

### <a name="authentication-components"></a>身份验证组件

以下的身份验证组件可用。

- **Microsoft.Owin.Security.ActiveDirectory**. 启用使用本地或基于云的目录服务的身份验证。
- **Microsoft.Owin.Security.Cookies**启用身份验证使用 cookie。 此包之前名为`Microsoft.Owin.Security.Forms`。
- **Microsoft.Owin.Security.Facebook**启用身份验证使用 Facebook 的基于 OAuth 的服务。
- **Microsoft.Owin.Security.Google**启用身份验证使用 Google 的基于 OpenID 的服务。
- **Microsoft.Owin.Security.Jwt**启用身份验证使用 JWT 令牌。
- **Microsoft.Owin.Security.MicrosoftAccount**启用身份验证使用 Microsoft 帐户。
- **Microsoft.Owin.Security.OAuth**. 提供对持有者令牌进行身份验证的 OAuth 授权服务器以及中间件。
- **Microsoft.Owin.Security.Twitter**启用身份验证使用 Twitter 的基于 OAuth 的服务。

此版本还包括`Microsoft.Owin.Cors`包，其中包含用于处理跨域 HTTP 请求的中间件。

> [!NOTE]
> 在 Visual Studio 2013 的最终版本中删除了为 JWT 签名的支持。

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

有关新功能和 Entity Framework 6 中的其他更改的列表，请参阅[实体框架版本历史记录](https://msdn.com/data/jj574253)。

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 包括以下新功能：

- 支持选项卡上编辑。 Preivously，**格式文档**命令、 自动缩进，并自动在 Visual Studio 中设置格式未正常工作时使用**保留选项卡**选项。 此更改将更正的格式设置的选项卡的 Razor 代码格式设置的 Visual Studio。
- 对 URL 重写规则生成的链接时的支持。
- 删除的安全透明特性。
 > [!NOTE]
 > 这是一项重大更改，并使 Razor 3 不兼容与 MVC4 及更早版本，而 Razor 2 与 MVC5 或针对 MVC5 编译的程序集不兼容。

找不到 Visual Studio 2013 中已修复从预发行版本的 razor 3 问题[此处](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0)。

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>ASP.NET 应用挂起

ASP.NET 应用挂起是从根本上更改的用户体验和经济的承载大量的一台计算机上的 ASP.NET 站点模型的.NET Framework 4.5.1 中的改变游戏规则的功能。 有关详细信息，请参阅[.NET web 承载的 ASP.NET 应用挂起 – 响应共享](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx)。

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>已知的问题和重大更改

本节介绍用于 Visual Studio 2013 的已知的问题和 ASP.NET 和 Web Tools 中的重大更改。

### <a name="nuget"></a>NuGet

- [使用 SLN 文件时，新的程序包还原不能在 Mono](https://nuget.codeplex.com/workitem/3596) – 将在即将到来的 nuget.exe 下载中修复和[NuGet.CommandLine 包](http://www.nuget.org/packages/NuGet.CommandLine/)更新。
- [新的程序包还原不能使用 Wix 项目](https://nuget.codeplex.com/workitem/3598)– 将在即将到来的 nuget.exe 下载中修复和[NuGet.CommandLine 包](http://www.nuget.org/packages/NuGet.CommandLine/)更新。
- [自动包还原不起作用的解决方案文件夹下的项目](https://nuget.codeplex.com/workitem/3625)– 将在 NuGet 2.8 中修复。

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)`不返回`IQueryable<T>`如我们增加了对支持始终`$select`和`$expand`。

    我们前面的示例为`ODataQueryOptions<T>`始终强制转换的返回值从`ApplyTo`到`IQueryable<T>`。 此查询选项，因为以前工作正常我们前面要支持 (`$filter`， `$orderby`， `$skip`， `$top`) 不会更改查询的形状。 现在，我们支持`$select`和`$expand`中的返回值`ApplyTo`将不会`IQueryable<T>`始终。

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    如果你使用的从前面的示例代码，它将继续工作，如果客户端不会发送`$select`和`$expand`。 但是，如果你想要支持`$select`和`$expand`你需要将该代码更改为。

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **在批处理请求过程的 Request.Url 或 RequestContext.Url 为 null**

    在批处理方案中， **UrlHelper**为 null 时从访问**Request.Url**或**RequestContext.Url**。

    此问题当前在此处跟踪： [BatchRequestContext.Url 不适用于批处理请求](http://aspnetwebstack.codeplex.com/workitem/1301)。

    此问题的解决方法是创建的新实例**UrlHelper**，下面的示例所示：

    **创建 UrlHelper 的新实例**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. 使用 MVC5 和时 OrgAuth，如果具有 AntiForgerToken 验证来执行此操作的视图，您可能遇到以下错误，当你将数据发布到该视图：

    **错误**:

    *'/' 应用程序中的服务器错误。*

    *一个声明的类型 http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier 或 http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider 未提供 ClaimsIdentity 上存在。若要启用防伪令牌支持基于声明的身份验证，请验证配置的声明提供程序提供这两个在它生成 ClaimsIdentity 实例上的这些声明。如果配置的声明提供方改为使用不同的声明类型，以唯一标识符形式，这可以通过设置静态属性 AntiForgeryConfig.UniqueClaimTypeIdentifier 进行配置。*

    **解决方法**:

    若要修复此错误的 Global.asax 中添加以下行：

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    在下一个版本将解决此问题。
2. 在升级到 MVC5 MVC4 应用程序之后, 生成解决方案并启动它。 你应看到以下错误：

    [A]System.Web.WebPages.Razor.Configuration.HostSection 不能强制转换为 [B]System.Web.WebPages.Razor.Configuration.HostSection。 A 类型是来自 System.Web.WebPages.Razor，Version = 2.0.0.0，区域性 = neutral，PublicKeyToken = 31bf3856ad364e35' 默认位置处的上下文中 C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll。 类型 B 源自 System.Web.WebPages.Razor，Version = 3.0.0.0，区域性 = neutral，PublicKeyToken = 31bf3856ad364e35' 默认位置处的上下文中 C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll。

    若要修复上述错误，请打开*所有*中你的项目和执行以下的 Web.config 文件 （包括视图文件夹中的）：

    1. 更新版本"4.0.0.0"的"System.Web.Mvc"到"5.0.0.0"的所有匹配的项。
    2. 更新的"System.Web.Helpers"版本"2.0.0.0"的所有匹配项&quot;System.Web.WebPages&quot;和&quot;System.Web.WebPages.Razor&quot;到"3.0.0.0"

    例如，进行上述更改后，程序集绑定应如下所示：

    [!code-xml[Main](release-notes/samples/sample24.xml)]

    有关升级到 MVC 5 的 MVC 4 项目的信息，请参阅[如何将 ASP.NET MVC 4 和 Web API 项目升级到 ASP.NET MVC 5 和 Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。
3. 当使用 jQuery 非介入式验证客户端验证，验证消息有时是 HTML 输入元素类型不正确 = number。 验证错误的所需的值 （"年龄字段必填") 显示数量无效而不是正确的消息有效的号码是必填的输入时。

    此问题通常找到了具有基架代码具有整数属性上创建和编辑视图模型。

    若要解决此问题，请更改从的编辑器帮助程序：

    `@Html.EditorFor(person => person.Age)`

    到:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 不再支持部分信任。 将链接到 MVC 或 WebAPI 二进制文件的项目应删除[SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx)属性和[AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx)属性。 移除这些特性可避免编译器错误，如下所示。

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > 请注意，为此，你的副作用不能在同一个应用程序中使用 4.0 和 5.0 程序集。 你需要它们全部更新为 5.0。

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>与 Facebook 授权 SPA 模板可能导致不稳定在 IE 中时在 intranet 区域中承载网站

SPA 模板提供了外部 Facebook 登录。 当本地运行时使用模板创建的项目时，则在登录可能会导致 IE 崩溃。

解决方案：

1. 托管该网站在 internet 区域;或

2. 在非 IE 浏览器中测试的方案。

### <a name="web-forms-scaffolding"></a>Web 窗体基架

Web 窗体基架已从 VS2013 中删除，并将在未来的更新到 Visual Studio 中可用。 但是，你仍可以通过添加 MVC 依赖项并为 MVC 生成基架使用在 Web 窗体项目中的基架。 你的项目将包含 Web 窗体和 MVC 的组合。

若要添加到你的 Web 窗体项目的 MVC，添加新的基架的项，并选择**MVC 5 依赖项**。 选择最少监视或完整具体取决于是否需要的所有内容文件，如脚本。 然后，添加基架的项 MVC，将在你的项目中创建视图和控制器的。

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC 和 Web API 基架-HTTP 404 找不到错误

如果在将基架的项添加到项目时遇到错误，，则可能你的项目将处于不一致状态。 某些将基架所做的更改将被回滚，但其他更改，例如已安装的 NuGet 程序包，将不会回滚。 如果回滚路由的配置更改，则用户将收到 HTTP 404 错误，当导航到基架项。

解决方法：

- 若要为 MVC 修复此错误，添加新的基架的项，然后选择 MVC 5 依赖项 （最小或完整）。 此过程将向你的项目添加所有所需的更改。
- 若要为 Web API 修复此错误：

    1. 将 WebApiConfig 类添加到你的项目。

        [!code-csharp[Main](release-notes/samples/sample25.cs)]

        [!code-vb[Main](release-notes/samples/sample26.vb)]
    2. 在应用程序中配置 WebApiConfig.Register\_，如下所示在 Global.asax 中启动方法：

        [!code-csharp[Main](release-notes/samples/sample27.cs)]

        [!code-vb[Main](release-notes/samples/sample28.vb)]
