---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: "ASP.NET 标识简介 |Microsoft 文档"
author: jongalloway
description: "ASP.NET 成员资格系统中引入了 ASP.NET 2.0 后在 2005 中，并且由于然后进行了很多更改方式的 web 应用程序 typicall..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 7c7dcb7903b0d0772acc560161ff39c6869c599a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-identity"></a>ASP.NET 标识简介
====================
通过[Jon Galloway](https://github.com/jongalloway)， [Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

> ASP.NET 成员资格系统中引入了 ASP.NET 2.0 后在 2005 中，并且由于然后进行了很多更改 web 应用程序通常处理身份验证和授权的方式。 ASP.NET 标识是全新查看成员资格系统在生成适用于 web、 电话或平板电脑的现代应用程序应该是什么。
> 
> 本文编写的 Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Jon Galloway ([@jongalloway](https://twitter.com/jongalloway))，Tom Dykstra 和 Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。


## <a name="background-membership-in-aspnet"></a>背景信息： 在 ASP.NET 中的成员身份

### <a name="aspnet-membership"></a>ASP.NET 成员资格

[ASP.NET 成员资格](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx)旨在解决常见 2005，均涉及到窗体身份验证和用户名、 密码和配置文件数据的 SQL Server 数据库中的站点成员身份要求。 目前没有 web 应用程序的数据存储选项的多更广泛数组，大多数开发人员想要使其站点能够使用社交标识提供者进行身份验证和授权功能。 ASP.NET 成员资格的设计限制难以此转换：

- 数据库架构旨在用于 SQL Server，您无法更改它。 您可以添加配置文件信息，但其他数据打包到另一个表，这会使难访问通过配置文件提供程序 API 通过任何方式除外。
- 提供程序系统使你能够更改后备数据存储，但系统围绕假设适用于关系数据库设计的。 你可以编写一个提供程序来存储非关系存储机制，如 Azure 存储表中的成员身份信息但则必须通过编写大量的代码和大量要解决的关系设计`System.NotImplementedException`不的方法的异常适用于 NoSQL 数据库。
- 由于日志/日志 out 功能基于窗体身份验证，不能使用成员资格系统[OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)。 OWIN 包括身份验证，包括登录名使用外部标识提供程序 （如 Microsoft 帐户、 Facebook、 Google、 Twitter)，支持的中间件组件和登录名使用组织帐户从本地 Active Directory 或Azure Active Directory。 OWIN 还包括支持 OAuth 2.0、 JWT 和 CORS。

### <a name="aspnet-simple-membership"></a>ASP.NET 简单成员身份

[ASP.NET 简单成员资格](../../../web-pages/overview/security/16-adding-security-and-membership.md)开发，作为成员资格系统的 ASP.NET Web 页。 它将已释放使用 WebMatrix 和 Visual Studio 2010 SP1。 简单的成员身份的目的是方便地将成员资格功能添加到网页应用程序。

简单的成员身份未更加轻松地自定义用户配置文件信息，而它仍共享 ASP.NET 成员资格的其他问题，它的一些限制：

- 很难将非关系存储区中的成员身份系统数据持久保存。
- 不能使用 OWIN 使用它。
- 它无法很好地使用现有的 ASP.NET 成员资格提供程序，并且不可扩展。

### <a name="aspnet-universal-providers"></a>ASP.NET 通用提供程序

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)开发以使其能够进行保持 Azure SQL 数据库，但它们也起作用与 SQL Server Compact 在 Microsoft 中的成员身份信息。 基于 Entity Framework Code First，这意味着通用提供程序可用于将数据保持受 EF 任何存储在生成了通用的提供程序。 使用通用提供程序，数据库架构已清理也相当多的很多。

通用提供程序基于 ASP.NET 成员资格的基础结构，以便它们仍执行相同的限制为 SqlMembership 提供程序。 也就是说，它们为关系数据库设计和很难自定义配置文件和用户信息。 这些提供程序还使用窗体身份验证登录和注销功能。

## <a name="aspnet-identity"></a>ASP.NET 标识

与成员资格中 ASP.NET 的情景已得到发展多年来，ASP.NET 团队已大量得出客户的反馈。

用户将登录通过输入用户名和密码在自己的应用程序中注册它们的假设将不再有效。 Web 已变得更社交。 用户通过社交通道，如 Facebook、 Twitter 和其他社交网站实时相互交互。 开发人员希望用户能够登录其社交标识，以便它们可以在自己网站上具有丰富的体验。 现代的成员身份系统必须启用到身份验证提供程序，如 Facebook、 Twitter 和其他基于重定向的日志项。

随着 web 开发发展，因此未 web 开发的模式。 单元测试的应用程序代码成为应用程序开发人员的问题。 在 2008 年 ASP.NET 添加一个新的框架，基于模型-视图-控制器 (MVC) 模式，部分来帮助开发人员构建可测试 ASP.NET 应用程序的单元。 开发人员希望对单元测试还希望能够执行该操作与成员资格系统对其应用程序逻辑。

考虑这些更改 web 应用程序开发中，ASP.NET 标识被开发与以下目标：

- **一个 ASP.NET 标识系统**

    - ASP.NET 标识可以用于所有的 ASP.NET 框架，例如 ASP.NET MVC、 Web 窗体、 网页、 Web API 和 SignalR。
    - 在生成 web、 电话、 存储或混合应用程序，可以使用 ASP.NET 标识。
- **有关用户的配置文件数据中插入的易用性**

    - 你可以控制用户及其配置文件信息的架构。 例如，你可以轻松地实现系统来存储在它们在你的应用程序中注册一个帐户时输入用户的出生日期。

- **持久性控件**

    - 默认情况下，ASP.NET 标识系统数据库中存储的所有用户信息。 ASP.NET 标识使用 Entity Framework Code First 来实现所有其持久性机制。
    - 由于您控制数据库架构、 常见任务，如更改表名称或更改主键的数据类型很简单。
    - 很容易地插入不同的存储机制，例如 SharePoint、 Azure 存储表服务、 NoSQL 数据库等，而不必引发`System.NotImplementedExceptions`异常。
- **单元可测试性**

    - ASP.NET 标识让 web 应用程序多个单元可测试。 你可以编写使用 ASP.NET 标识应用程序的部分的单元测试。
- **角色提供程序**

    - 没有角色提供程序，这样就可以限制到你的应用程序的部分的角色的访问权限。 你可以轻松创建角色，例如"Admin"和用户添加到角色。
- **基于声明**

    - ASP.NET 标识支持基于声明的身份验证，其中用户的标识表示为一组声明。 声明允许开发人员描述用户的标识，不是角色允许很多更有意义。 角色成员身份是只是一个布尔值 （成员或非成员），而声明可以包括有关用户的标识和成员身份的丰富信息。
- **社交登录提供程序**

    - 你可以轻松添加到你的应用程序，如 Microsoft 帐户、 Facebook、 Twitter、 Google、 及其他的社交日志接和存储你的应用程序中的特定于用户的数据。
- **Azure Active Directory**

    - 你可以添加日志功能使用 Azure Active Directory 和特定于用户的数据存储在你的应用程序。 有关详细信息，请参阅[组织帐户](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)在 Visual Studio 2013 中创建 ASP.NET Web 项目中
- **OWIN 集成**

    - ASP.NET 身份验证现在基于可在任何基于 OWIN 的主机的 OWIN 中间件。 ASP.NET 标识 System.Web 没有任何依赖项。 它是一个完全符合 OWIN 框架，并且可以用在任何托管的 OWIN 应用程序。
    - ASP.NET 标识为在/日志-注销的 web 站点中的用户使用 OWIN 身份验证。 这意味着，而不是使用 FormsAuthentication 生成 cookie，应用程序使用 OWIN CookieAuthentication 执行该操作。
- **NuGet 包**

    - ASP.NET 标识会为它们安装在随附在 Visual Studio 2013 的 ASP.NET MVC、 Web 窗体和 Web API 模板的 NuGet 包重新分发。 你可以从 NuGet 库下载此 NuGet 程序包。
    - 释放与 NuGet 的 ASP.NET 标识包使容易 ASP.NET 团队来循环访问新功能和 bug 修复和交付到开发人员敏捷的方式。

## <a name="getting-started-with-aspnet-identity"></a>开始使用 ASP.NET 标识

Visual Studio 2013 项目模板中使用 ASP.NET 标识为 ASP.NET MVC、 Web 窗体、 Web API 和 SPA。 在此演练中，我们将说明项目模板如何使用 ASP.NET 标识来添加功能，用于注册、 登录和注销用户。

使用以下过程来实现 ASP.NET 标识。 本文的目的是让你的 ASP.NET 标识; 高级别概述你可以按照分步来或只需读取的详细信息。 有关更多详细说明创建应用程序使用 ASP.NET 标识，包括使用新 API 添加用户、 角色和配置文件信息，请参阅本文末尾的后续步骤部分。

1. 使用个人帐户中创建 ASP.NET MVC 应用程序。 你可以使用 ASP.NET MVC、 Web 窗体、 Web API、 SignalR 等中的 ASP.NET 标识。这篇文章中我们将启动与 ASP.NET MVC 应用程序。  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. 创建的项目包含 ASP.NET 标识以下三个包。

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 此包已将保持 ASP.NET 标识数据和架构与 SQL Server ASP.NET 标识的实体框架实现。
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 此包为 ASP.NET 标识有核心接口。 此包可用于编写实现 ASP.NET 标识，目标不同的持久性存储 （如 Azure 表存储） NoSQL 数据库等。
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 此程序包包含用于在 ASP.NET 应用程序中加入与 ASP.NET Identity OWIN 身份验证的功能。 这用于日志的功能添加到你的应用程序和 OWIN Cookie 身份验证中间件调用生成 cookie 时。
3. 创建用户。  
 启动应用程序，然后单击**注册**链接来创建的用户。 下图显示了注册页它收集用户名和密码。  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 当用户单击**注册**按钮，`Register`在 Account 控制器的操作通过调用 ASP.NET 标识 API 中，为以下突出显示部分创建用户：

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. 登录。  
 如果已成功创建用户，她将通过记录中`SignInAsync`方法。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 在上面的突出显示的代码`SignInAsync`方法生成[ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)。 ASP.NET 标识和 OWIN Cookie 身份验证是基于声明的系统事件，因为框架需要生成用户 ClaimsIdentity 应用程序。 ClaimsIdentity 具有的用户，如用户所属的角色的所有声明的信息。 在此阶段，你还可以添加用户的多个的声明。  
  
 以下代码中的突出显示`SignInAsync`方法在用户使用签名从 OWIN 和调用 AuthenticationManager`SignIn`和在 ClaimsIdentity 中传递。  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. 注销。  
 单击**注销**链接帐户控制器中调用该注销操作。 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 突出显示的代码所示 OWIN`AuthenticationManager.SignOut`方法。 这是类似于[FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)方法，由[FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Web 窗体中的模块。

## <a name="components-of-aspnet-identity"></a>ASP.NET 标识的组件

下图显示了 ASP.NET 标识系统中的各组成部分 (单击[这](introduction-to-aspnet-identity/_static/image3.png)或放大关系图上)。 绿色中的包构成了 ASP.NET 标识系统。 所有其他包都需要 ASP.NET 应用程序中使用的 ASP.NET 标识系统的依赖项。

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

下面是不前面提到的 NuGet 包的简短说明：

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 可让应用程序使用 cookie 中间件基于身份验证，类似于 ASP。NET 的窗体身份验证。
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 实体框架是用于关系数据库的 Microsoft 的推荐使用此数据访问技术。

## <a name="migrating-from-membership-to-aspnet-identity"></a>从成员资格迁移到 ASP.NET 标识

我们希望很快迁移现有应用到新的 ASP.NET 标识系统中使用 ASP.NET 成员资格或简单的成员身份提供指导。

## <a name="next-steps"></a>后续步骤

- [创建 ASP.NET MVC 5 应用程序使用 Facebook 和 Google OAuth2 和 OpenID 登录](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 本教程使用 ASP.NET 标识 API 添加到用户数据库的配置文件信息以及如何使用 Google 和 Facebook 进行身份验证。
- [使用身份验证和 SQL 数据库中创建的 ASP.NET MVC 应用并部署到 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 本教程演示如何使用标识 API 来添加用户和角色。
- [单个用户帐户](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth)中创建 ASP.NET Web 项目在 Visual Studio 2013
- [组织帐户](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth)中创建 ASP.NET Web 项目在 Visual Studio 2013
- [在 ASP.NET Identity 中 VS 2013 模板中的自定义配置文件信息](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [从 VS 2013 项目模板中使用的社交提供商获取详细信息](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 演示如何添加基本的角色和用户支持以及如何执行角色和用户管理的示例应用程序。
