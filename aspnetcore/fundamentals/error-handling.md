---
title: "ASP.NET Core 中的错误处理"
author: ardalis
description: "了解如何处理 ASP.NET Core 应用程序中的错误。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>ASP.NET Core 中的错误处理简介

作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra/)

本文介绍了处理 ASP.NET Core 应用中常见错误的一些方法。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

## <a name="the-developer-exception-page"></a>开发人员异常页

如需在应用中配置可以显示异常详细信息的页面，请先安装 `Microsoft.AspNetCore.Diagnostics` NuGet 包并将以下代码行添加到 Startup 类 [Startup 类配置方法](startup.md)：

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

`UseDeveloperExceptionPage` 需要放在所有你想要捕获异常的中间件声明之前，如`app.UseMvc`。

>[!WARNING]
> **仅当应用程序在开发环境中运行时**才启用开发人员异常页。 否则当应用程序在生产环境中运行时，详细的异常信息会向公众泄露 了解[环境配置](environments.md)。

若要查看开发人员异常页，可使用`Development`环境来运行应用程序，并在基础 URL 后添加`?throw=true`。 该页面包括几个选项卡，这些选项卡中包含关于异常和请求的信息。 第一个选项卡包括堆栈跟踪。 

![堆栈跟踪](error-handling/_static/developer-exception-page.png)

下一个`Query`选项卡将显示查询字符串参数信息（如有）。

![查询字符串参数](error-handling/_static/developer-exception-page-query.png)

示例请求没有任何 cookie，但如果有的话，它们将出现在 **Cookies**选项卡。最后一个选项卡显示请求的标头。

![标头](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>配置自定义异常处理页

最好配置一个当应用在非 `Development` 环境中运行时可以使用的异常处理页。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

在 MVC 应用程序中，不要使用 HTTP 谓词 Attribute（如 `HttpGet`）来显式修饰错误处理程序操作方法。 使用显式谓词会阻止某些请求访问方法。

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>配置状态代码页

默认情况下，应用程序不会为 500（内部服务器错误） 或 404（未找到）等 HTTP 状态代码提供丰富状态代码页。 可以通过在 `Configure` 方法中添加行来配置 `StatusCodePagesMiddleware`：

```csharp
app.UseStatusCodePages();
```

默认情况下，此中间件为常见状态代码（如 404）添加简单的纯文本处理程序：

![404 页面](error-handling/_static/default-404-status-code.png)

该中间件支持几种不同的扩展方法。 一种采用 lambda 表达式，另一种采用内容类型和格式字符串。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

也可以使用重定向扩展方法。 一个将 302 状态代码发送到客户端，另一个将原始的状态代码返回到客户端，但同时还会执行重定向 URL 的处理程序。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

如需禁用某些请求的状态代码页，可以通过以下代码实现：

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>异常处理代码

异常处理页中的代码可能会引发异常。 建议在生产错误页面中包含纯静态内容。

此外还要注意，一旦发送响应的标头，则无法更改响应的状态代码，也不能运行任何异常页或处理程序。 必须完成响应或中止连接。

## <a name="server-exception-handling"></a>服务器异常处理

除应用程序中的异常处理逻辑外，托管应用程序的服务器[服务器](servers/index.md)也会执行一些异常处理。 如果服务器在发送标头之前捕获异常，服务器将发送没有正文的 500 内部服务器错误响应。 如果服务器在已发送标头后捕获异常，服务器会关闭连接。 应用程序无法处理的请求将由服务器进行处理。 发生的任何异常都将由服务器进行处理。 任何配置的自定义错误页面或异常处理中间件或筛选器都不会影响此行为。

## <a name="startup-exception-handling"></a>启动异常处理

应用程序启动期间发生的异常仅可在承载层进行处理。 可以使用 `captureStartupErrors` 和 `detailedErrors` 键[配置主机在响应启动期间的错误时的行为方式](hosting.md#detailed-errors)。

仅当捕获的启动错误发生在主机地址/端口绑定之后，承载层才会为该错误显示错误页。 如果绑定因任何原因而失败，则承载层会记录关键异常，dotnet 进程崩溃，且不显示任何错误页。

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC 错误处理

[MVC](xref:mvc/overview) 应用还有一些其他的错误处理选项，例如配置异常筛选器和执行模型验证。

### <a name="exception-filters"></a>异常筛选器

在 MVC 应用中，异常筛选器可以进行全局配置，也可以为每个控制器或每个操作单独配置。 这些筛选器处理在执行控制器操作或其他筛选器时出现的任何未处理的异常，且不会以其他方式调用。 通过[筛选器](../mvc/controllers/filters.md)详细了解异常筛选器。

>[!TIP]
> 异常筛选器非常适用于捕获 MVC 操作内出现的异常，但灵活性不如错误处理中间件。 一般情况下建议使用中间件；仅在需要根据所选 MVC 操作以不同方式执行错误处理时，才使用筛选器。

### <a name="handling-model-state-errors"></a>处理模型状态错误

[模型验证](../mvc/models/validation.md)在每个控制器操作被调用之前发生，操作方法负责检查 `ModelState.IsValid` 并相应地作出反应。

某些应用会使用标准约定来处理模型验证错误，在这种情况下，使用[筛选器](../mvc/controllers/filters.md)可以更好地实施这种策略。 你需要使用无效模型状态测试操作的行为。 查看[测试控制器逻辑](../mvc/controllers/testing.md)了解详情。



