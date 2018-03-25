---
title: "ASP.NET Core 模块"
author: tdykstra
description: "介绍 ASP.NET Core 模块 (ANCM)，该模块是一种可使 Kestrel Web 服务器将 IIS 或 IIS Express 用作反向代理服务器的 IIS 模块。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>ASP.NET Core 模块简介

作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl) 和 [Chris Ross](https://github.com/Tratcher) 

ASP.NET Core 模块 (ANCM) 允许在 IIS 背后运行 ASP.NET Core 应用程序，将 IIS 用于 IIS 所擅长的领域（安全性、可管理性等），将 [Kestrel](kestrel.md) 用于 Kestrel 所擅长的领域（速度快），让你同时从这两种技术中获益。 ANCM 仅适用于 Kestrel；它不与 WebListener（ASP.NET Core 1.x 版本）或 HTTP.sys（2.x 版本）兼容。 

受支持的 Windows 版本：

* Windows 7 和 Windows Server 2008 R2 及更高版本

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="what-aspnet-core-module-does"></a>ASP.NET Core 模块的作用

ANCM 是一种挂钩到 IIS 管道并将流量重定向到后端 ASP.NET Core 应用程序的本机 IIS 模块。 大多数其他模块（如 Windows 身份验证）仍有机会运行。 仅当为请求选择了处理程序，且处理程序映射在应用程序 web.config 文件中定义时，ANCM 才进行掌控。

由于 ASP.NET Core 应用程序在独立于 IIS 工作进程的进程中运行，因此，ANCM 还执行进程管理。 当第一个请求传入时，ANCM 启动 ASP.NET Core 应用程序进程，并在进程崩溃时重新启动进程。 这本质上是与在 IIS 中进行进程内运行并由 WAS（Windows 激活服务）托管的经典 ASP.NET 应用程序相同的行为。

下图阐释了 IIS、ANCM 和 ASP.NET Core 应用程序之间的关系。

![ASP.NET Core 模块](aspnet-core-module/_static/ancm.png)

请求来自 Web 并命中内核模式 Http.Sys 驱动程序，该驱动程序在主要端口 (80) 或 SSL 端口 (443) 上将请求路由到 IIS。 ANCM 在针对应用程序配置的 HTTP 端口（不是端口 80/443）上将请求转发到 ASP.NET Core 应用程序。

Kestrel 侦听来自 ANCM 的流量。  ANCM 在启动时通过环境变量指定端口，且 [UseIISIntegration](#call-useiisintegration) 方法将服务器配置来侦听 `http://localhost:{port}`。 存在其他一些检查，拒绝不是来自 ANCM 的请求。 （ANCM 不支持 HTTPS 转发，因此即使请求由 IIS 通过 HTTPS 接收，它们还是通过 HTTP 转发。）

Kestrel 从 ANCM 获取请求，并将请求推送到 ASP.NET Core 中间件管道，然后该管道将对请求进行处理，并将其作为 `HttpContext` 实例传递到应用程序逻辑。 接着，应用程序的响应被传递回 IIS，IIS 将响应推送回启动请求的 HTTP 客户端。

ANCM 还具有几项其他功能：

* 设置环境变量。
* 将 `stdout` 输出记录到文件存储。
* 转发 Windows 身份验证令牌。

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>如何在 ASP.NET Core 应用中使用 ANCM

本部分概述设置 IIS 服务器和 ASP.NET Core 应用程序的过程。 有关详细说明，请参阅[使用 IIS 在 Windows 上进行托管](xref:host-and-deploy/iis/index)。

### <a name="install-ancm"></a>安装 ANCM


必须在服务器上的 IIS 中和开发计算机上 IIS Express 中安装 ASP.NET Core 模块。 对于服务器，ANCM 包括在 [.NET Core Windows 服务器主机捆绑包](https://aka.ms/dotnetcore-2-windowshosting)中。 对于开发计算机，Visual Studio 在 IIS Express 和 IIS（如果计算机上未安装 IIS Express）中自动安装 ANCM。

### <a name="install-the-iisintegration-nuget-package"></a>安装 IISIntegration NuGet 包

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 包随附于 ASP.NET Core 元包（[Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) 和 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)）中。 如果不使用元包，请单独安装 `Microsoft.AspNetCore.Server.IISIntegration`。 `IISIntegration` 包是互操作性包，它读取 ANCM 广播的环境变量来设置应用。 环境变量提供配置信息，如要侦听的端口。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在应用程序中安装 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)。 `IISIntegration` 包是互操作性包，它读取 ANCM 广播的环境变量来设置应用。 环境变量提供配置信息，如要侦听的端口。 

---

### <a name="call-useiisintegration"></a>调用 UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

通过 IIS 运行 [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) 上的 `UseIISIntegration` 扩展方法时，将自动调用此方法。

如果未使用 ASP.NET Core 元包，且未安装 `Microsoft.AspNetCore.Server.IISIntegration` 包，则将收到运行时错误。 如果显式调用 `UseIISIntegration`，则在未安装此包的情况下将收到编译时错误。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在应用程序的 `Main` 方法中，调用 [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) 上的 `UseIISIntegration` 扩展方法。 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration` 方法将查找 ANCM 设置的环境变量，如果未找到环境变量，则不进行任何操作。 此行为有助于在 macOS 或 Linux 上进行开发和测试，以及部署到运行 IIS 的服务器等方案。 当在 macOS 或 Linux 上运行时，Kestrel 充当 Web 服务器；但当应用部署到 IIS 环境中时，它将自动使用 ANCM 和 IIS。

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>ANCM 端口绑定替代其他端口绑定

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM 生成动态端口，以分配给后端进程。 `UseIISIntegration` 方法获取此动态端口，并将 Kestrel 配置为侦听 `http://locahost:{dynamicPort}/`。 这将替代其他 URL 配置，如对 `UseUrls` 或 [Kestrel 的侦听 API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) 的调用。 因此，在使用 ANCM 时，无需调用 `UseUrls` 或 Kestrel 的 `Listen` API。 如果调用 `UseUrls` 或 `Listen`，则在不使用 IIS 情况下运行应用时，Kestrel 将侦听指定的端口。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM 生成动态端口，以分配给后端进程。 `UseIISIntegration` 方法获取此动态端口，并将 Kestrel 配置为侦听 `http://locahost:{dynamicPort}/`。 这将替代其他 URL 配置，如对 `UseUrls` 的调用。 因此，在使用 ANCM 时，无需调用 `UseUrls`。 如果调用 `UseUrls`，则在不使用 IIS 情况下运行应用时，Kestrel 将侦听指定的端口。

在 ASP.NET Core 1.0 中，如果调用 `UseUrls`，请在调用 `UseIISIntegration` 之前对其进行调用，以便不覆盖 ANCM 配置的端口。 ASP.NET Core 1.1 中不需要此调用顺序，因为 ANCM 设置将重写 `UseUrls`。

---

### <a name="configure-ancm-options-in-webconfig"></a>在 Web.config 中配置 ANCM 选项

针对 ASP.NET Core 模块的配置存储在位于应用程序的根文件夹中的 web.config 文件中。 此文件中的设置指向启动命令和启动 ASP.NET Core 应用的参数。 有关 web.config 代码的示例和配置选项指南，请参阅 [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)。

### <a name="run-with-iis-express-in-development"></a>在开发过程中通过 IIS Express 运行

Visual Studio 可使用 ASP.NET Core 模板定义的默认配置文件启动 IIS Express。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>代理配置使用 HTTP 协议和配对令牌

在 ANCM 和 Kestrel 之间创建的代理使用 HTTP 协议。 使用 HTTP 是一种性能优化，其中 ANCM 和 Kestrel 之间的流量发生于脱离网络接口的环回地址。 因此，不存在从脱离服务器的位置窃取 ANCM 和 Kestrel 之间的流量的风险。

配对令牌用于保证 Kestrel 收到的请求已由 IIS 代理且不来自某些其他源。 ANCM 已创建配对令牌并将其设置到环境变量 (`ASPNETCORE_TOKEN`)。 此外，配对令牌还设置到每个代理请求的标头 (`MSAspNetCoreToken`)。 IIS 中间件检查它所接收的每个请求，以确认配对令牌标头值与环境变量值相匹配。 如果令牌值不匹配，则将记录请求并拒绝该请求。 无法从脱离服务器的位置访问配对令牌环境变量及 ANCM 和 Kestrel 之间的流量。 如果不知道配对令牌值，攻击者就无法提交绕过 IIS 中间件中的检查的请求。

## <a name="next-steps"></a>后续步骤

有关更多信息，请参见以下资源：

* [本文的示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET Core 模块源代码](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core 模块配置参考](xref:host-and-deploy/aspnet-core-module)
* [使用 IIS 在 Windows 上进行托管](xref:host-and-deploy/iis/index)
