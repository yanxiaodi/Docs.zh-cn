---
uid: mvc/mvc5
title: "ASP.NET MVC 5 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET MVC 5 ASP.NET MVC 5 是一个框架，用于构建使用成熟设计模式以及 AS.的能力的可缩放的、 基于标准的 web 应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: e57163469ae4606df0fc17e3e054b7696782a084
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>什么是 ASP.NET MVC 5 中的新增功能

### <a name="one-aspnet"></a>一个 ASP.NET

Web MVC 项目模板与新的一个 ASP.NET 体验无缝集成。 可以自定义 MVC 项目，并使用一个 ASP.NET 项目创建向导配置身份验证。 处找不到 ASP.NET MVC 5 的介绍性教程[Getting Started with ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md)。

有关升级到 MVC 5 的 MVC 4 项目的信息，请参阅[如何将 ASP.NET MVC 4 和 Web API 项目升级到 ASP.NET MVC 5 和 Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)。

### <a name="aspnet-identity"></a>ASP.NET 标识

MVC 项目模板已更新为使用 ASP.NET 标识进行身份验证和标识管理。 处找不到具有 Facebook 和 Google 身份验证和新的成员资格 API 教程，说明如何[创建 ASP.NET MVC 5 应用程序使用 Facebook 和 Google OAuth2 和 OpenID 登录](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)和[部署包含的安全 ASP.NET MVC 应用程序成员资格、 OAuth 和 SQL 数据库部署到 Windows Azure 网站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。

### <a name="bootstrap"></a>bootstrap

MVC 项目模板已更新为使用[Bootstrap](http://getbootstrap.com/)提供作为时尚且高度可响应外观和感觉，你可以轻松自定义。 有关详细信息，请参阅[Visual Studio 2013 web 项目模板中的 Bootstrap](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)。

### <a name="authentication-filters"></a>身份验证筛选器

[身份验证筛选器](http://www.dotnetcurry.com/showarticle.aspx?ID=957)是一种新的筛选器在 ASP.NET MVC，在 ASP.NET MVC 管道中的授权筛选器之前运行并且使您能够指定身份验证逻辑每个操作，每个控制器或全局范围内的所有控制器。 身份验证筛选器处理请求中的凭据，并提供相应的用户权限。 身份验证筛选器还可以添加身份验证质询响应未经授权的请求。 请参阅[ASP.NET MVC 5 身份验证筛选器](http://www.dotnetcurry.com/showarticle.aspx?ID=957)， [ASP.NET MVC 5 中的身份验证筛选器](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/)和[最后新 ASP.NET MVC 5 身份验证筛选器 ！](http://hackwebwith.net/finally-the-new-asp-net-mvc-5-authentication-filters/)。

### <a name="filter-overrides"></a>筛选器将重写

现在可以重写筛选器应用于给定的操作方法或控制器通过指定[重写筛选器](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)。 重写筛选器指定一的组不应为给定作用域 （操作或控制器） 运行的筛选器类型。 这允许您配置执行全局应用，但然后从将应用于特定操作或控制器中排除某些全局筛选器的筛选器。 请参阅[ASP.NET MVC 5 和 ASP.NET Web API 2 中的新筛选器将重写功能](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx)，[如何使用 ASP.NET MVC 5 筛选器将重写功能](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/)，和[筛选器在 ASP.NET MVC 5 中重写](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>属性路由

ASP.NET MVC 现在支持[的属性路由](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)，感谢通过 Tim McCall 的作者贡献[http://attributerouting.net](http://attributerouting.net)。 使用的属性路由可以对你的操作和控制器进行批注来指定路由。

## <a name="new-web-project-experience"></a>新的 Web 项目体验

我们已增强，在 Visual Studio 2013 中创建新的 web 项目的体验。 在**新的 ASP.NET Web 项目**对话框，你可以选择想、 配置技术 (Web 窗体，MVC、 Web API) 的任意组合，配置身份验证选项和添加单元测试项目的项目类型。

![新建 ASP.NET 项目](mvc5/_static/image1.png)

新建对话框可以更改模板中的许多的默认身份验证选项。 例如，创建 ASP.NET Web 窗体项目时可以选择任意下列选项：

- 无身份验证
- 单个用户帐户 （ASP.NET 成员资格或社交提供程序登录）
- 组织帐户 (internet 应用程序中的 Active Directory)
- Windows 身份验证 (intranet 应用程序中的 Active Directory)

![身份验证选项](mvc5/_static/image2.png)

有关用于创建 web 项目的新过程的详细信息，请参阅[在 Visual Studio 2013 中创建 ASP.NET Web 项目](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md)。 有关新的身份验证选项的详细信息，请参阅[ASP.NET 标识](../identity/overview/index.md)。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET 基架

ASP.NET 基架是用于 ASP.NET Web 应用程序的代码生成框架。 它可以轻松将样板文件代码添加到您的项目，与数据模型进行交互。

在以前版本的 Visual Studio 中，基架只限于 ASP.NET MVC 项目。 使用 Visual Studio 2013，现在可以用于任何 ASP.NET 项目，包括 Web 窗体使用基架。 Visual Studio 2013 当前不支持生成页对于 Web 窗体项目，但你仍可以使用基架，Web 窗体通过向项目添加 MVC 依赖项。 将在将来更新中添加对生成的 Web 窗体页支持。

在使用基架，我们确保所有必需的项目中安装依赖关系。 例如，如果您首先 ASP.NET Web 窗体项目，然后使用基架添加一个 Web API 控制器，所需的 NuGet 程序包和引用将自动添加到你的项目。

若要将 MVC 基架添加到 Web 窗体项目，添加**新建基架项**和选择**MVC 5 依赖项**对话框窗口中。 有两个选项的基架 MVC;最小和完整。 如果你选择最小，只有的 NuGet 包和 ASP.NET MVC 的引用添加到项目中。 如果选择完整选项，添加最小依赖关系，以及对于 MVC 项目所需的内容文件。

对基架异步控制器支持使用 Entity Framework 6 中的新异步功能。

有关详细信息和教程，请参阅[ASP.NET 基架概述](../visual-studio/overview/2013/aspnet-scaffolding-overview.md)。

### <a name="getting-help-and-reporting-issues"></a>获取帮助和报表问题

- [已知的问题和重大更改列表](../visual-studio/overview/2013/release-notes.md#knownissues)
- 获取帮助和讨论中的 ASP.NET MVC 5[论坛](https://forums.asp.net/1146.aspx)
- [报告在 ASP.NET MVC 5 bug](https://github.com/aspnet/AspNetWebStack/issues)
- [发出功能请求](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>从 ASP.NET MVC 4 升级

请参阅[如何升级 ASP.NET MVC 4 和 Web API 项目到 ASP.NET MVC 5 和 Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
