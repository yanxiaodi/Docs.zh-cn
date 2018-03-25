---
title: "ASP.NET Core 基础知识"
author: rick-anderson
description: "了解生成 ASP.NET Core 应用程序的基础概念。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: be37df7789354ac4ce8e373a1560366be157ffa5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core 基础知识

ASP.NET Core 应用程序是在其 `Main` 方法中创建 Web 服务器的控制台应用：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` 方法调用 `WebHost.CreateDefaultBuilder`，后者按照生成器模式来创建 Web 应用程序主机。 生成器提供定义 Web 服务器（例如，`UseKestrel`）和启动类 (`UseStartup`) 的方法。 在前面的例子中，自动分配了 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器。 ASP.NET Core 的 Web 主机尝试在 IIS 上运行（如果可用）。 对于其他 Web 服务器（如 [HTTP.sys](xref:fundamentals/servers/httpsys)），可通过调用相应的扩展方法来使用。 在下一节对 `UseStartup` 进行了更深入的介绍。

`IWebHostBuilder` 是 `WebHost.CreateDefaultBuilder` 调用的返回类型，它提供了许多可选方法。 其中的一些方法包括用于在 HTTP.sys 中托管应用的 `UseHttpSys`，以及用于指定根内容目录的 `UseContentRoot`。 `Build` 和 `Run` 方法生成 `IWebHost` 对象，该对象托管应用并开始侦听 HTTP 请求。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` 方法使用 `WebHostBuilder`，后者按照生成器模式来创建 Web 应用程序主机。 生成器提供定义 Web 服务器（例如，`UseKestrel`）和启动类 (`UseStartup`) 的方法。 在前面的示例中，使用了 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器。 对于其他 Web 服务器（如 [WebListener](xref:fundamentals/servers/weblistener)），可通过调用相应的扩展方法来使用。 在下一节对 `UseStartup` 进行了更深入的介绍。

`WebHostBuilder` 提供了许多可选方法，其中包括用于在 IIS 和 IIS Express 中进行托管的 `UseIISIntegration`，以及用于指定根内容目录的 `UseContentRoot`。 `Build` 和 `Run` 方法生成 `IWebHost` 对象，该对象托管应用并开始侦听 HTTP 请求。

---

## <a name="startup"></a>启动

`WebHostBuilder` 上的 `UseStartup` 方法为你的应用指定 `Startup` 类：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` 类用于定义请求处理管道和配置应用所需的任何服务。 `Startup` 必须是公共类，并包含以下方法：

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` 定义应用所使用的[服务](#dependency-injection-services)（如 ASP.NET Core MVC、Entity Framework Core 和标识）。 `Configure` 定义请求管道的[中间件](xref:fundamentals/middleware/index)。

有关详细信息，请参阅[应用程序启动](xref:fundamentals/startup)。

## <a name="content-root"></a>内容根

内容根是应用所使用的任何内容的基路径，如视图、[Razor 页面](xref:mvc/razor-pages/index) 和静态资产。 默认情况下，内容根与用于托管应用的可执行文件的应用程序基路径相同。

## <a name="web-root"></a>Web 根

应用的 Web 根是项目中的目录，其中包含公共资源、CSS 等静态资源、JavaScript 和图形文件。

## <a name="dependency-injection-services"></a>依赖关系注入（服务）

服务是应用中常用的组件。 可以通过[依存关系注入 (DI)](xref:fundamentals/dependency-injection) 来获取服务。 ASP.NET Core 包括默认支持[构造函数注入](xref:mvc/controllers/dependency-injection#constructor-injection)的本机控制反转 (IoC) 容器。 可根据需要替换默认本机容器。 DI 除了具备松散耦合优势以外，还可以使服务（例如，[日志记录](xref:fundamentals/logging/index)）在整个应用中可用。

有关详细信息，请参阅[依存关系注入](xref:fundamentals/dependency-injection)。

## <a name="middleware"></a>中间件

在 ASP.NET Core 中，使用[中间件](xref:fundamentals/middleware/index)来撰写请求管道。 ASP.NET Core 中间件在 `HttpContext` 上执行异步逻辑，然后调用序列中的下一个中间件或直接终止请求。 通过在 `Configure` 方法中调用 `UseXYZ` 扩展方法来添加名为“XYZ”的中间件组件。

ASP.NET Core 包含一组丰富的内置中间件：

* [静态文件](xref:fundamentals/static-files)
* [路由](xref:fundamentals/routing)
* [身份验证](xref:security/authentication/index)
* [响应压缩中间件](xref:performance/response-compression)
* [URL 重写中间件](xref:fundamentals/url-rewriting)

可以将任何基于 [OWIN](http://owin.org) 的中间件与 ASP.NET Core 应用结合使用，也可以编写自己的自定义中间件。

有关详细信息，请参阅[中间件](xref:fundamentals/middleware/index)和 [.NET 的开放 Web 接口 (OWIN)](xref:fundamentals/owin)。

## <a name="environments"></a>环境

环境（如“开发”环境和“生产”环境）是 ASP.NET Core 的高级概念，可使用环境变量进行设置。

有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments)。

## <a name="configuration"></a>配置

ASP.NET Core 基于名称/值对使用配置模型。 配置模型不基于 `System.Configuration` 或 *web.config*。配置从一组有序的配置提供程序获取设置。 内置配置提供程序支持各种文件格式（XML、 JSON、INI）和环境变量，从而实现基于环境的配置。 也可以编写你自己的自定义配置提供程序。

有关详细信息，请参阅[配置](xref:fundamentals/configuration/index)。

## <a name="logging"></a>日志记录

ASP.NET Core 支持适用于各种日志记录提供程序的日志记录 API。 内置提供程序支持向一个或多个目标发送日志。 可使用第三方记录框架。

[日志记录](xref:fundamentals/logging/index)

## <a name="error-handling"></a>错误处理

ASP.NET Core 的内置功能可处理应用中的错误，包括开发人员异常页、自定义错误页、静态状态代码页和启动异常处理。

有关详细信息，请参阅[错误处理](xref:fundamentals/error-handling)。

## <a name="routing"></a>路由

ASP.NET Core 提供将应用请求路由到路由处理程序的功能。

有关详细信息，请参阅[路由](xref:fundamentals/routing)。

## <a name="file-providers"></a>文件提供程序

ASP.NET Core 通过使用文件提供程序抽象化文件系统访问，文件提供程序可提供一个跨平台处理文件的通用界面。

有关详细信息，请参阅[文件提供程序](xref:fundamentals/file-providers)。

## <a name="static-files"></a>静态文件

静态文件中间件为静态文件（如 HTML、CSS、映像和 JavaScript）提供服务。

有关详细信息，请参阅[使用静态文件](xref:fundamentals/static-files)。

## <a name="hosting"></a>宿主

ASP.NET Core 应用可配置和启动一个*主机*，负责应用启动和生存期管理。

有关详细信息，请参阅[托管](xref:fundamentals/hosting)。

## <a name="session-and-application-state"></a>会话和应用程序状态

会话状态是 ASP.NET Core 中的一项功能，可用于在用户浏览 Web 应用时保存和存储用户数据。

有关详细信息，请参阅[会话和应用程序状态](xref:fundamentals/app-state)。

## <a name="servers"></a>服务器

ASP.NET Core 托管模型不直接侦听请求。 托管模型依赖 HTTP 服务器实现将请求转发到应用。 转发的请求被打包为一组可通过接口进行访问的功能对象。 ASP.NET Core 包含托管的跨平台 Web 服务器，名为 [Kestrel](xref:fundamentals/servers/kestrel)。 Kestrel 通常在生产 Web 服务器（如 [IIS](https://www.iis.net/) 或 [Nginx](http://nginx.org)）后台运行。 Kestrel 可作为边缘服务器运行。

有关详细信息，请参阅[服务器](xref:fundamentals/servers/index)和下列主题：

* [Kestrel](xref:fundamentals/servers/kestrel)
* [ASP.NET Core 模块](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys)（以前称为 [WebListener](xref:fundamentals/servers/weblistener)）

## <a name="globalization-and-localization"></a>全球化和本地化

使用 ASP.NET Core 创建多语言网站，可让网站拥有更多受众。 ASP.NET Core 提供的服务和中间件可将网站本地化为不同的语言和文化。

有关详细信息，请参阅[全球化和本地化](xref:fundamentals/localization)。

## <a name="request-features"></a>请求功能

与 HTTP 请求和响应相关的 Web 服务器实现详细信息在接口中定义。 服务器实现和中间件使用这些接口来创建和修改应用的托管管道。

有关详细信息，请参阅[请求功能](xref:fundamentals/request-features)。

## <a name="background-tasks"></a>后台任务

后台任务作为*托管服务*实现。 托管服务是一个类，具有实现 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 接口的后台任务逻辑。

有关详细信息，请参阅[使用托管服务的后台任务](xref:fundamentals/hosted-services)。

## <a name="open-web-interface-for-net-owin"></a>.NET 的开放 Web 接口 (OWIN)

ASP.NET Core 支持 .NET 的开放 Web 接口 (OWIN)。 OWIN 允许 Web 应用从 Web 服务器分离。

有关详细信息，请参阅 [.NET 的开放 Web 接口 (OWIN)](xref:fundamentals/owin)。

## <a name="websockets"></a>WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) 是一个协议，支持通过 TCP 连接建立持久的双向信道。 它可用于聊天、股票报价和游戏等应用，以及 Web 应用中需要实时功能的任何位置。 ASP.NET Core 支持 Web 套接字功能。

有关详细信息，请参阅 [WebSockets](xref:fundamentals/websockets)。

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All 元包

ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 元包包括：

* ASP.NET Core 团队支持的所有包。
* Entity Framework Core 支持的所有包。 
* ASP.NET Core 和 Entity Framework Core 使用的内部和第三方依赖关系。

有关详细信息，请参阅 [Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)。

## <a name="net-core-vs-net-framework-runtime"></a>.NET Core 与 .NET Framework 运行时

ASP.NET Core 应用可以面向 .NET Core 或 .NET Framework 运行时。

有关详细信息，请参阅[在 .NET Core 和 .NET Framework 之间进行选择](/dotnet/articles/standard/choosing-core-framework-server)。

## <a name="choose-between-aspnet-core-and-aspnet"></a>在 ASP.NET Core 和 ASP.NET 之间进行选择

有关在 ASP.NET Core 和 ASP.NET 之间进行选择的详细信息，请参阅 [在 ASP.NET Core 和 ASP.NET 之间进行选择](xref:fundamentals/choose-between-aspnet-and-aspnetcore)。
