---
title: "ASP.NET Core 中的应用程序启动"
author: ardalis
description: "了解 ASP.NET Core 中的 Startup 类如何配置服务和应用的请求管道。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: c324918b33af82b619bb2251f32308e4a57c27e5
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="application-startup-in-aspnet-core"></a>ASP.NET Core 中的应用程序启动

作者：[Steve Smith](https://ardalis.com)、[Tom Dykstra](https://github.com/tdykstra) 和 [Luke Latham](https://github.com/guardrex)

`Startup` 类配置服务和应用的请求管道。

## <a name="the-startup-class"></a>Startup 类

ASP.NET Core 应用使用 `Startup` 类，按照约定命名为 `Startup`。 `Startup` 类：

* 可选择性地包括 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 方法以配置应用的服务。
* 必须包括 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 方法以创建应用的请求处理管道。

当应用启动时，运行时调用 `ConfigureServices` 和 `Configure`：

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

通过 [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions)、[UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 方法指定 `Startup` 类：

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup` 类构造函数接受由主机定义的依赖关系。 在 `Startup` 类中[注入依赖关系](xref:fundamentals/dependency-injection)的常见用途为注入：

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) 以按环境配置服务。
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 以在启动过程中配置应用。

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

注入 `IHostingEnvironment` 的替代方法是使用基于约定的方法。 该应用可以为不同的环境（例如 `StartupDevelopment`）定义单独的 `Startup` 类，并在运行时选择适当的 startup 类。 优先考虑名称后缀与当前环境相匹配的类。 如果应用在开发环境中运行并包含 `Startup` 类和 `StartupDevelopment` 类，则使用 `StartupDevelopment` 类。 有关详细信息，请参阅[使用多个环境](xref:fundamentals/environments#startup-conventions)。

若要详细了解 `WebHostBuilder`，请参阅[承载](xref:fundamentals/hosting)主题。 有关在启动过程中处理错误的信息，请参阅[启动异常处理](xref:fundamentals/error-handling#startup-exception-handling)。

## <a name="the-configureservices-method"></a>ConfigureServices 方法

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 方法是：

* 可选。
* 在 `Configure` 方法配置应用服务之前，由 Web 主机调用。
* 其中按常规设置[配置选项](xref:fundamentals/configuration/index)。

将服务添加到服务容器，使其在应用和 `Configure` 方法中可用。 这些服务通过[依赖关系注入](xref:fundamentals/dependency-injection)或 [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) 解析。

Web 主机可能会在调用 `Startup` 方法之前配置某些服务。 有关详细信息，请参阅[承载](xref:fundamentals/hosting)主题。 

对于需要大量设置的功能，[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection) 上有 `Add[Service]` 扩展方法。 典型 Web 应用将为实体框架、标识和 MVC 注册服务：

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Startup 中可用的服务

Web 主机提供 `Startup` 类构造函数可用的某些服务。 应用通过 `ConfigureServices` 添加其他服务。 然后，主机和应用服务都可以在 `Configure` 和整个应用程序中使用。

## <a name="the-configure-method"></a>Configure 方法

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 方法用于指定应用响应 HTTP 请求的方式。 可通过将[中间件](xref:fundamentals/middleware/index)组件添加到 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 实例来配置请求管道。 `Configure` 方法可使用 `IApplicationBuilder`，但未在服务容器中注册。 承载创建 `IApplicationBuilder` 并将其直接传递给 `Configure`（[引用源](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)）。

[ASP.NET Core 模板](/dotnet/core/tools/dotnet-new)配置支持开发人员异常页、[BrowserLink](http://vswebessentials.com/features/browserlink)、错误页、静态文件和 ASP.NET MVC 的管道：

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

每个 `Use` 扩展方法将中间件组件添加到请求管道。 例如，`UseMvc` 扩展方法将[路由中间件](xref:fundamentals/routing)添加到请求管道，并将 [MVC](xref:mvc/overview) 配置为默认设置。

其他服务（如 `IHostingEnvironment` 和 `ILoggerFactory`），也可以在方法签名中指定。 如果指定，其他服务如果可用，将被注入。

有关如何使用 `IApplicationBuilder` 的详细信息，请参阅[中间件](xref:fundamentals/middleware/index)。

## <a name="convenience-methods"></a>便利方法

可使用 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) 和 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 便利方法，而不是指定 `Startup` 类。 多次调用 `ConfigureServices` 将追加到另一个。 多次调用 `Configure` 将使用上一个方法调用。

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Startup 筛选器

在应用的 [Configure](#the-configure-method) 中间件管道的开头或末尾使用 [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) 来配置中间件。 `IStartupFilter` 有助于确保中间件在应用请求处理管道的开始或结束时由库添加的中间件之前或之后运行。

`IStartupFilter` 实现单个方法（即 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure)），该方法接收并返回 `Action<IApplicationBuilder>`。 [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) 定义用于配置应用请求管道的类。 有关详细信息，请参阅[使用 IApplicationBuilder 创建中间件管道](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)。

在请求管道中，每个 `IStartupFilter` 实现一个或多个中间件。 筛选器按照添加到服务容器的顺序调用。 筛选器可在将控件传递给下一个筛选器之前或之后添加中间件，从而附加到应用管道的开头或末尾。

[示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）演示如何向 `IStartupFilter` 注册中间件。 示例应用包含一个中间件，该中间件从查询字符串参数中设置选项值：

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

在 `RequestSetOptionsStartupFilter` 类中配置 `RequestSetOptionsMiddleware`：

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

在 `ConfigureServices` 的服务容器中注册 `IStartupFilter`：

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

当提供 `option` 的查询字符串参数时，中间件在 MVC 中间件呈现响应之前处理分配值：

![浏览器窗口显示已呈现的索引页。 基于请求页面（其中查询字符串参数和选项的值设置为“From Middleware”），Option 的值呈现为“From Middleware”。](startup/_static/index.png)

中间件执行顺序由 `IStartupFilter` 注册顺序设置：

* 多个 `IStartupFilter` 实现可能与相同的对象进行交互。 如果顺序很重要，请将它们的 `IStartupFilter` 服务注册进行排序，以匹配其中间件应有的运行顺序。
* 库可能添加包含一个或多个 `IStartupFilter` 实现的中间件，这些实现在向 `IStartupFilter` 注册的其他应用中间件之前或之后运行。 若要在库的 `IStartupFilter` 添加中间件之前调用 `IStartupFilter` 中间件，请在将库添加到服务容器之前定位服务注册。 若要在此后调用，请在添加库之后定位服务注册。

## <a name="additional-resources"></a>其他资源

* [承载](xref:fundamentals/hosting)
* [使用多个环境](xref:fundamentals/environments)
* [中间件](xref:fundamentals/middleware/index)
* [日志记录](xref:fundamentals/logging/index)
* [配置](xref:fundamentals/configuration/index)
* [StartupLoader 类：FindStartupType 方法（引用源）](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
