---
title: "在 ASP.NET 核心中配置 Windows 身份验证"
author: ardalis
description: "本文介绍如何在 ASP.NET Core 上使用 IIS Express、 IIS、 HTTP.sys，和 WebListener 中配置 Windows 身份验证。"
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: f6efd838d7b6c837c75f36591a49eab812f9d54c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a>在 ASP.NET Core 应用程序配置 Windows 身份验证

作者：[Steve Smith](https://ardalis.com) 和 [Scott Addie](https://twitter.com/Scott_Addie)

Windows 身份验证可以配置用于与 IIS 托管的 ASP.NET Core 应用[HTTP.sys](xref:fundamentals/servers/httpsys)，或[WebListener](xref:fundamentals/servers/weblistener)。

## <a name="what-is-windows-authentication"></a>什么是 Windows 身份验证？

Windows 身份验证依赖于操作系统的 ASP.NET Core 应用的用户进行身份验证。 在使用 Active Directory 域标识或其他 Windows 帐户来标识用户的企业网络上运行你的服务器时，你可以使用 Windows 身份验证。 Windows 身份验证是最适合 intranet 环境中的用户、 客户端应用程序和 web 服务器属于同一 Windows 域。

[了解有关 Windows 身份验证和为 IIS 安装](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)。

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>启用 ASP.NET Core 应用程序中的 Windows 身份验证

Visual Studio Web 应用程序模板可以配置为支持 Windows 身份验证。

### <a name="use-the-windows-authentication-app-template"></a>使用 Windows 身份验证应用程序模板

在 Visual Studio 中：
1. 创建新的 ASP.NET Core Web 应用程序。 
1. 从模板列表中选择 Web 应用程序。
1. 选择**更改身份验证**按钮，然后选择**Windows 身份验证**。 

运行应用。 用户名已显示在顶部的应用程序的权限。

![Windows 身份验证浏览器屏幕快照](windowsauth/_static/browser-screenshot.png)

对于使用 IIS Express 的开发工作，该模板提供所有使用 Windows 身份验证所需的配置。 以下部分说明如何手动配置 Windows 身份验证的 ASP.NET Core 应用。

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>为 Windows 以及匿名身份验证的 visual Studio 设置

Visual Studio 项目**属性**页面的**调试**选项卡为 Windows 身份验证和匿名身份验证提供的复选框。

![Windows 身份验证浏览器屏幕快照](windowsauth/_static/vs-auth-property-menu.png)

或者，在中配置这两个属性*launchSettings.json*文件：

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>启用与 IIS 的 Windows 身份验证

IIS 使用[ASP.NET 核心模块](xref:fundamentals/servers/aspnet-core-module)到承载 ASP.NET Core 应用程序。 模块流 Windows 身份验证到 IIS 默认情况下。 在 IIS 中，不应用配置了 Windows 身份验证。 以下部分说明如何使用 IIS 管理器来配置 ASP.NET Core 应用以使用 Windows 身份验证。

### <a name="create-a-new-iis-site"></a>创建一个新的 IIS 站点

指定名称和文件夹，并允许它创建新的应用程序池。

### <a name="customize-authentication"></a>自定义身份验证

打开身份验证菜单上的站点。

![IIS 身份验证菜单](windowsauth/_static/iis-authentication-menu.png)

禁用匿名身份验证并启用 Windows 身份验证。

![IIS 身份验证设置](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>将你的项目发布到 IIS 站点文件夹

使用 Visual Studio 或.NET 核心 CLI，将应用发布到目标文件夹。

![Visual Studio 发布对话框](windowsauth/_static/vs-publish-app.png)

详细了解[发布到 IIS](xref:host-and-deploy/iis/index)。

启动应用程序来验证使用 Windows 身份验证。

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>启用 Windows 身份验证与 HTTP.sys 或 WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

尽管 Kestrel 不支持 Windows 身份验证，你可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)以在 Windows 上支持自承载的方案。 下面的示例将配置应用程序的 web 主机 HTTP.sys 用于 Windows 身份验证：

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

尽管 Kestrel 不支持 Windows 身份验证，你可以使用[WebListener](xref:fundamentals/servers/weblistener)以在 Windows 上支持自承载的方案。 下面的示例将配置应用程序的 web 主机 WebListener 用于 Windows 身份验证：

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>使用 Windows 身份验证

匿名访问的配置状态确定的方式`[Authorize]`和`[AllowAnonymous]`特性用于应用程序中。 以下两节介绍如何处理的匿名访问权限不允许的和允许配置状态。

### <a name="disallow-anonymous-access"></a>不允许匿名访问

当启用了 Windows 身份验证并且禁用了匿名访问，`[Authorize]`和`[AllowAnonymous]`属性产生任何影响。 如果 IIS 站点 （或 HTTP.sys 或 WebListener 服务器） 配置为不允许匿名访问，请求将永远不会到达你的应用程序。 为此，`[AllowAnonymous]`属性不适用。

### <a name="allow-anonymous-access"></a>允许匿名访问

当启用了 Windows 身份验证和匿名访问时，使用`[Authorize]`和`[AllowAnonymous]`属性。 `[Authorize]`属性允许你保护的应用程序的部分，真正需要 Windows 身份验证。 `[AllowAnonymous]`特性重写`[Authorize]`属性中允许匿名访问的应用程序的使用情况。 请参阅[简单授权](xref:security/authorization/simple)对于特性用法的详细信息。

在 ASP.NET Core 2.x，`[Authorize]`属性要求中的其他配置*Startup.cs*质询对于 Windows 身份验证的匿名请求。 建议的配置略有根据正在使用的 web 服务器而有所不同。

> [!NOTE]
> 默认情况下，缺少授权可访问的页面的用户出现了空的 HTTP 403 响应。 [StatusCodePages 中间件](xref:fundamentals/error-handling#configuring-status-code-pages)可以配置为用户提供更好的"拒绝访问"体验。

#### <a name="iis"></a>IIS

如果使用 IIS，添加到以下`ConfigureServices`方法： 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

如果使用 HTTP.sys，添加到以下`ConfigureServices`方法：

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Impersonation

ASP.NET 核心未实现模拟。 使用应用程序池或进程标识的所有请求的应用程序标识使用运行应用。 如果你需要显式执行代表用户操作，使用`WindowsIdentity.RunImpersonated`。 在此上下文中运行的单个操作，然后关闭上下文。

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

请注意，`RunImpersonated`不支持异步操作，并且不应可用于复杂的方案。 例如，包装整个请求或中间件链不受支持或建议。

---
