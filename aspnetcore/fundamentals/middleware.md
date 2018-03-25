---
title: "ASP.NET Core 中间件"
author: rick-anderson
description: "了解 ASP.NET Core 中间件和请求管道。"
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware
ms.openlocfilehash: 697a3a8e475dda0d48a11dbfad8fbb0603aa1bf8
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a>ASP.NET Core 中间件基础知识

<a name="fundamentals-middleware"></a>

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Steve Smith](https://ardalis.com/)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="what-is-middleware"></a>什么是中间件？

中间件是一种装配到应用程序管道以处理请求和响应的软件。 每个组件：

* 选择是否将请求传递到管道中的下一个组件。
* 可在调用管道中的下一个组件前后执行工作。 

请求委托用于生成请求管道。 请求委托处理每个 HTTP 请求。

使用 [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) 和 [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) 扩展方法来配置请求委托。 可将一个单独的请求委托并行指定为匿名方法（称为并行中间件），或在可重用的类中对其进行定义。 这些可重用的类和并行匿名方法即为中间件或中间件组件。 请求管道中的每个中间件组件负责调用管道中的下一个组件，或在适当情况下使链发生短路。

[将 HTTP 模块迁移到中间件](../migration/http-modules.md)介绍了 ASP.NET Core 中的请求管道和早期版本之间的差异，并提供了更多的中间件示例。

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>使用 IApplicationBuilder 创建中间件管道

ASP.NET Core 请求管道包含一系列相继调用的请求委托，如下图所示（执行过程遵循黑色箭头）：

![请求处理模式显示请求到达、通过三个中间件进行处理以及响应离开应用程序。 每个中间件运行其逻辑，并在 next() 语句处将请求传递到下一个中间件。 在第三个中间件处理请求之后，在离开应用程序前，它被传回通过前两个中间件于 next() 语句后进行其他处理，作为对客户端的响应。](middleware/_static/request-delegate-pipeline.png)

每个委托均可在下一个委托前后执行操作。 此外，委托还可以决定不将请求传递给下一个委托，这就是对请求管道进行短路。 通常需要短路，因为这样可以避免不必要的工作。 例如，静态文件中间件可以返回静态文件请求并使管道的其余部分短路。 需要尽早在管道中调用异常处理委托，以便它们可以捕获在管道的后期阶段所发生的异常。

尽可能简单的 ASP.NET Core 应用设置了处理所有请求的单个请求委托。 这种情况不包括实际请求管道。 相反，调用单个匿名函数以响应每个 HTTP 请求。

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

第一个 [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) 委托终止了管道。

可使用 [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) 将多个请求委托链接在一起。 `next` 参数表示管道中的下一个委托。 （请记住，可通过不调用 next 参数使管道短路。）通常可在下一个委托前后执行操作，如以下示例所示：

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> 在向客户端发送响应后，请勿调用 `next.Invoke`。 响应启动后，针对 `HttpResponse` 的更改将引发异常。 例如，设置标头、状态代码等更改将引发异常。 调用 `next` 后写入响应正文：
> - 可能导致违反协议。 例如，写入的长度超过规定的 `content-length`。
> - 可能损坏正文格式。 例如，向 CSS 文件中写入 HTML 页脚。
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) 是一个有用的提示，指示是否已发送标头和/或已写入正文。

## <a name="ordering"></a>订购

向 `Configure` 方法添加中间件组件的顺序定义了针对请求调用这些组件的顺序，以及响应的相反顺序。 此排序对于安全性、性能和功能至关重要。

Configure 方法（如下所示）添加以下中间件组件：

1. 异常/错误处理
2. 静态文件服务器
3. 身份验证
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

在以上代码中，`UseExceptionHandler` 是添加到管道的第一个中间件组件，因此，该组件可捕获在后面的调用中发生的任何异常。

因为尽早在管道中调用静态文件中间件，因此该组件可处理请求并使引致短路，而无需通过剩余组件。 静态文件中间件不提供授权检查。 可公开访问由静态文件中间件服务的任何文件，包括 wwwroot 下的文件。 请参阅[使用静态文件](xref:fundamentals/static-files)获取有关保护静态文件的方法。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


如果静态文件中间件未处理请求，则请求将被传递给执行身份验证的标识中间件 (`app.UseAuthentication`)。 标识不使未经身份验证的请求短路。 虽然标识对请求进行身份验证，但仅在 MVC 选择特定 Razor 页或控制器和操作后，才发生授权（和拒绝）。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果静态文件中间件未处理请求，则请求将被传递给执行身份验证的标识中间件 (`app.UseIdentity`)。 标识不使未经身份验证的请求短路。 虽然标识对请求进行身份验证，但仅在 MVC 选择特定控制器和操作后，才发生授权（和拒绝）。

-----------

以下示例演示中间件排序，其中静态文件的请求在响应压缩中间件前由静态文件中间件进行处理。 静态文件未通过此中间件排序进行压缩。 可压缩来自 [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 的 MVC 响应。

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Use、Run 和 Map

使用 `Use`、`Run` 和 `Map` 配置 HTTP 管道。 `Use` 方法可使管道短路（即不调用 `next` 请求委托）。 `Run` 是一种约定，并且某些中间件组件可公开在管道末尾运行的 `Run[Middleware]` 方法。

`Map*` 扩展用作约定来创建管道分支。 [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) 基于给定请求路径的匹配项来创建请求管道分支。 如果请求路径以给定路径开头，则执行分支。

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

下表使用前面的代码显示来自 `http://localhost:1234` 的请求和响应：

| 请求 | 响应 |
| --- | --- |
| localhost:1234 | 来自非 Map 委托的 Hello。  |
| localhost:1234/map1 | Map 测试 1 |
| localhost:1234/map2 | Map 测试 2 |
| localhost:1234/map3 | 来自非 Map 委托的 Hello。  |

使用 `Map` 时，将从 `HttpRequest.Path` 中删除匹配的线段，并针对每个请求将该线段追加到 `HttpRequest.PathBase`。

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) 基于给定谓词的结果创建请求管道分支。 `Func<HttpContext, bool>` 类型的任何谓词均可用于将请求映射到管道的新分支。 在以下示例中，谓词用于检测查询字符串变量 `branch` 是否存在：

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

下表使用前面的代码显示来自 `http://localhost:1234` 的请求和响应：

| 请求 | 响应 |
| --- | --- |
| localhost:1234 | 来自非 Map 委托的 Hello。  |
| localhost:1234/?branch=master | 使用的分支 = 主要分支|

`Map` 支持嵌套，例如：

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

此外，`Map` 还可同时匹配多个段，例如：

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>内置中间件

ASP.NET Core 附带以下中间件组件，以及用于添加这些组件的顺序的说明：

| 中间件 | 描述 | 订单 |
| ---------- | ----------- | ----- |
| [身份验证](xref:security/authentication/identity) | 提供身份验证支持。 | 在需要 `HttpContext.User` 之前。 OAuth 回叫的终端。 |
| [CORS](xref:security/cors) | 配置跨域资源共享。 | 在使用 CORS 的组件之前。 |
| [诊断](xref:fundamentals/error-handling) | 配置诊断。 | 在生成错误的组件之前。 |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | 将代理标头转发到当前请求。 | 在使用更新的字段（示例：Scheme、Host、ClientIP、Method）的组件之前。 |
| [响应缓存](xref:performance/caching/middleware) | 提供对缓存响应的支持。 | 在需要缓存的组件之前。 |
| [响应压缩](xref:performance/response-compression) | 提供对压缩响应的支持。 | 在需要压缩的组件之前。 |
| [RequestLocalization](xref:fundamentals/localization) | 提供本地化支持。 | 在对本地化敏感的组件之前。 |
| [路由](xref:fundamentals/routing) | 定义和约束请求路由。 | 用于匹配路由的终端。 |
| [会话](xref:fundamentals/app-state) | 提供对管理用户会话的支持。 | 在需要会话的组件之前。 |
| [静态文件](xref:fundamentals/static-files) | 为提供静态文件和目录浏览提供支持。 | 如果请求与文件匹配，则为终端。 |
| [URL 重写](xref:fundamentals/url-rewriting) | 提供对重写 URL 和重定向请求的支持。 | 在使用 URL 的组件之前。 |
| [WebSockets](xref:fundamentals/websockets) | 启用 WebSockets 协议。 | 在接受 WebSocket 请求所需的组件之前。 |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>写入中间件

通常，中间件封装在类中，并且通过扩展方法公开。 请考虑以下中间件，该中间件通过查询字符串设置当前请求的区域性：

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

注意：以上示例代码用于演示创建中间件组件。 有关 ASP.NET Core 的内置本地化支持，请参阅[全球化和本地化](xref:fundamentals/localization)。

可通过传入区域性（如 `http://localhost:7997/?culture=no`）测试中间件。

以下代码将中间件委托移动到类：

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

以下扩展方法通过 [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) 公开中间件：

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

以下代码通过 `Configure` 调用中间件：

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

中间件应通过在其构造函数中公开其依赖项来遵循[显式依赖项原则](http://deviq.com/explicit-dependencies-principle/)。 在每个应用程序生存期构造一次中间件。 如果需要与请求中的中间件共享服务，请参阅下面讲述的按请求依赖项。

中间件组件可通过构造函数参数从依赖关系注入解析其依赖项。 此外，[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) 还可直接接受其他参数。

### <a name="per-request-dependencies"></a>按请求依赖项

由于中间件是在应用启动时构造的，而不是按请求构造的，因此在每个请求过程中，中间件构造函数使用的范围内生存期服务不与其他依赖关系注入类型共享。 如果必须在中间件和其他类型之间共享范围内服务，请将这些服务添加到 `Invoke` 方法的签名。 `Invoke` 方法可接受由依赖关系注入填充的其他参数。 例如:

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>资源

* [本文档中使用的示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [将 HTTP 模块迁移到中间件](../migration/http-modules.md)
* [应用程序启动](startup.md)
* [请求功能](request-features.md)
