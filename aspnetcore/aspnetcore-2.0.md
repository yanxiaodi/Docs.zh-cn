---
title: "ASP.NET Core 2.0 中的新增功能"
author: rick-anderson
description: "ASP.NET Core 2.0 中的新增功能"
manager: wpickett
ms.author: riande
ms.date: 07/10/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: aspnetcore-2.0
ms.openlocfilehash: 35d57abd07e83cd5b190572962fbf43aef03a534
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="whats-new-in-aspnet-core-20"></a>ASP.NET Core 2.0 中的新增功能

本文重点介绍 ASP.NET Core 2.0 中最重要的更改，并提供相关文档的链接。

## <a name="razor-pages"></a>Razor 页面

Razor 页面是 ASP.NET Core MVC 的一个新功能，它可以使基于页面的编码方式更简单高效。

有关详细信息，请参阅相关介绍和教程：

* [Razor 页面介绍](xref:mvc/razor-pages/index)
* [Razor 页面入门](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>ASP.NET Core 元包

新的 ASP.NET Core 元包包含 ASP.NET Core 和 Entity Framework 团队生成和提供支持的所有包及其内部和第三方依赖项。 无需再通过包选择单个 ASP.NET Core 功能。 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 包中包含所有的功能。 默认模板使用此包。

有关详细信息，请参阅 [ASP.NET Core 2.0 的 Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)。

## <a name="runtime-store"></a>运行时存储

使用 `Microsoft.AspNetCore.All` 元包的应用程序会自动使用新的 .NET Core 运行时存储。 此存储包含运行 ASP.NET Core 2.0 应用程序所需的所有运行时资产。 使用 `Microsoft.AspNetCore.All` 元包时，应用程序不会部署引用的 ASP.NET Core NuGet 包中的任何资产，因为目标系统中已存在这些资产。 运行时存储中的资产也已经过预编译，以便缩短应用程序启动时间。

有关详细信息，请参阅[运行时存储](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)

## <a name="net-standard-20"></a>.NET Standard 2.0

ASP.NET Core 2.0 包面向 NET Standard 2.0。 这些包可以由其他 .NET Standard 2.0 库引用，也可以在兼容 .NET Standard 2.0 的 .NET 实现上运行，其中包括 .NET Core 2.0 和 .NET Framework 4.6.1。 

`Microsoft.AspNetCore.All` 元包仅面向 .NET Core 2.0，因为它旨在与 .NET Core 2.0 运行时存储一起使用。

## <a name="configuration-update"></a>配置更新

在 ASP.NET Core 2.0 中，已默认将 `IConfiguration` 实例添加到服务容器。 服务容器中的 `IConfiguration` 可以使应用程序更轻松地从容器中检索配置值。

有关已规划文档的状态的信息，请参阅 [GitHub 问题](https://github.com/aspnet/Docs/issues/3387)。

## <a name="logging-update"></a>日志记录更新

在 ASP.NET Core 2.0 中，已默认将日志记录并入依存关系注入 (DI) 系统。 在 Program.cs 文件（而非 Startup.cs 文件）中添加提供程序并配置筛选。 此外，默认的 `ILoggerFactory` 支持进行筛选，并且你可以使用灵活的方式来进行跨提供程序筛选和特定于提供程序的筛选。

有关详细信息，请参阅[日志记录介绍](xref:fundamentals/logging/index)。

## <a name="authentication-update"></a>身份验证更新

新的身份验证模型简化了使用 DI 为应用程序配置身份验证的过程。

借助新模板，可以使用 [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/) 为 Web 应用和 Web API 配置身份验证。

有关已规划文档的状态的信息，请参阅 [GitHub 问题](https://github.com/aspnet/Docs/issues/3054)。

## <a name="identity-update"></a>标识更新

在 ASP.NET Core 2.0 中，我们简化了使用标识生成安全的 Web API 的过程。 可以使用 [Microsoft 身份验证库 (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client)获取用于访问 Web API 的访问令牌。

有关 2.0 中的身份验证更改的详细信息，请参阅以下资源：

* [ASP.NET Core 中的帐户确认和密码恢复](xref:security/authentication/accconfirm)
* [为 ASP.NET Core 中的验证器应用启用 QR 码生成](xref:security/authentication/identity-enable-qrcodes)
* [将身份验证和标识迁移到 ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>SPA 模板

已提供适用于 Angular、Aurelia、Knockout.js、React.js 及 React.js 和 Redux 的单页应用程序 (SPA) 项目模板。 Angular 模板已更新至 Angular 4。 默认情况下，Angular 和 React 模板已可用；有关如何获取其他模板的信息，请参阅[新建 SPA 项目](xref:client-side/spa-services#creating-a-new-project)。 有关如何在 ASP.NET Core 中生成 SPA 的信息，请参阅[使用 JavaScriptServices 创建单页应用程序](xref:client-side/spa-services)。

## <a name="kestrel-improvements"></a>Kestrel 改进

Kestrel Web 服务器包含一项新功能，使其更适合作为面向 Internet 的服务器。 在 `KestrelServerOptions` 类的新 `Limits` 属性中添加大量服务器约束配置选项。 为以下内容添加限制：

- 客户端最大连接数
- 请求正文最大大小
- 请求正文最小数据速率

有关详细信息，请参阅 [ASP.NET Core 中的 Kestrel Web 服务器实现](xref:fundamentals/servers/kestrel)。

## <a name="weblistener-renamed-to-httpsys"></a>WebListener 已重命名为 HTTP.sys

`Microsoft.AspNetCore.Server.WebListener` 和 `Microsoft.Net.Http.Server` 包已合并为一个新包 `Microsoft.AspNetCore.Server.HttpSys`。 命名空间已进行更新以保持一致。

有关详细信息，请参阅 [ASP.NET Core 中的 HTTP.sys Web 服务器实现](xref:fundamentals/servers/httpsys)。

## <a name="enhanced-http-header-support"></a>增强了 HTTP 标头支持

使用 MVC 传输 `FileStreamResult` 或 `FileContentResult` 时，现在可以选择对传输的内容设置 `ETag` 或 `LastModified` 日期。 可以使用如下所示的代码在返回的内容上设置这些值：

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

返回给访问者的文件将附带 `ETag` 和 `LastModified` 值的适当 HTTP 标头。

如果应用程序访问者使用范围请求标头请求内容，ASP.NET 将识别出这一点，并会处理该标头。 如果可以对请求的内容执行部分传输操作，ASP.NET 将适当地跳过一些内容，只返回请求的字节集。  不必为了采用或处理此功能而将任何特殊的处理程序写入方法；系统会自动处理。

## <a name="hosting-startup-and-application-insights"></a>托管启动和 Application Insights

托管环境现在可以在应用程序启动时插入额外的包依赖项并执行代码，而应用程序无需显式使用依赖项或调用任何方法。 可以使用此功能来允许某些环境“启用”该环境特有的功能，而应用程序无需提前获知。 

在 ASP.NET Core 2.0 中，如果在 Visual Studio 中调试并且（选择加入后）在 Azure App Services 中运行，将使用此功能自动启用 Application Insights 诊断。 因此，默认情况下，项目模板不再添加 Application Insights 包和代码。

有关已规划文档的状态的信息，请参阅 [GitHub 问题](https://github.com/aspnet/Docs/issues/3389)。

## <a name="automatic-use-of-anti-forgery-tokens"></a>自动使用防伪标记

默认情况下，ASP.NET Core 始终在帮助对内容进行 HTML 编码，但是在新版本中，还采用了额外的措施来帮助预防跨网站请求伪造 (XSRF) 攻击。 现在在默认情况下，ASP.NET Core 会发出防伪标记，并在窗体 POST 操作和页面上验证它们，且无需其他配置。

有关详细信息，请参阅[在 ASP.NET Core 中预防跨网站请求伪造 (XSRF/CSRF) 攻击](xref:security/anti-request-forgery)。

## <a name="automatic-precompilation"></a>自动预编译

默认情况下，会在发布时启用 Razor 视图预编译，以缩减发布输出大小和应用程序启动时间。

## <a name="razor-support-for-c-71"></a>Razor 支持 C# 7.1

Razor 视图引擎已更新为可使用新的 Roslyn 编译器。 其中包含对 C# 7.1 功能的支持，例如默认表达式、推断元组名称和泛型模式匹配。 若要在项目中使用 C# 7.1，请在项目文件中添加以下属性，然后重新加载解决方案：

```xml
<LangVersion>latest</LangVersion>
```

有关 C# 7.1 功能的状态的信息，请参阅 [Roslyn GitHub 存储库](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md)。

## <a name="other-documentation-updates-for-20"></a>2.0 的其他文档更新

* [用于 ASP.NET Core 应用部署的 Visual Studio 发布配置文件](xref:host-and-deploy/visual-studio-publish-profiles)
* [密钥管理](xref:security/data-protection/implementation/key-management)
* [为 Facebook 配置身份验证](xref:security/authentication/facebook-logins)
* [为 Twitter 配置身份验证](xref:security/authentication/twitter-logins)
* [为 Google 配置身份验证](xref:security/authentication/google-logins)
* [为 Microsoft 帐户配置身份验证](xref:security/authentication/microsoft-logins)

## <a name="migration-guidance"></a>迁移指南

有关如何将 ASP.NET Core 1.x 应用程序迁移到 ASP.NET Core 2.0 的指南，请参阅以下资源：

* [从 ASP.NET Core 1.x 迁移到 ASP.NET Core 2.0](xref:migration/1x-to-2x/index)
* [将身份验证和标识迁移到 ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>其他信息

有关更改的完整列表，请参阅 [ASP.NET Core 2.0 发行说明](https://github.com/aspnet/Home/releases/tag/2.0.0)。

若要实时了解 ASP.NET Core 开发团队的进度和计划，请收看 [ASP.NET Community Standup](https://live.asp.net/)。
