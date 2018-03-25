---
title: "ASP.NET Core 中基于工厂的中间件激活"
author: guardrex
description: "了解如何在 ASP.NET Core 中通过基于工厂的激活实现使用强类型中间件。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 57ff9db2edbf307f2442443dc14e69b0498f7475
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET Core 中基于工厂的中间件激活

作者：[Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 是[中间件](xref:fundamentals/middleware/index)激活的扩展点。

`UseMiddleware` 扩展方法检查中间件的已注册类型是否实现 `IMiddleware`。 如果是，则使用在容器中注册的 `IMiddlewareFactory` 实例来解析 `IMiddleware` 实现，而不使用基于约定的中间件激活逻辑。 中间件在应用的服务容器中注册为作用域或瞬态服务。

优点：

* 按请求（作用域服务的注入）激活
* 让中间件强类型化

`IMiddleware` 按请求激活，因此作用域服务可以注入到中间件的构造函数中。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

示例应用演示了使用以下两种方式激活的中间件：

* 约定（`ConventionalMiddleware`）。 有关使用约定激活中间件的详细信息，请参阅[中间件](xref:fundamentals/middleware/index)主题。
* [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 实现（`IMiddlewareMiddleware`）。 默认的 [MiddlewareFactory 类](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)可激活中间件。

这两种中间件实现的功能相同，并能记录由查询字符串参数 (`key`) 提供的值。 中间件使用插入的数据库上下文（作用域服务）将查询字符串值记录在内存中数据库。

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) 定义应用的请求管道的中间件。 [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) 方法处理请求，并返回代表中间件执行的 `Task`。

使用约定激活的中间件：

[!code-csharp[Main](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

使用 `MiddlewareFactory` 激活的中间件：

[!code-csharp[Main](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

程序会为中间件创建扩展：

[!code-csharp[Main](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

无法通过 `UseMiddleware` 将对象传递给工厂激活的中间件：

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

将工厂激活的中间件添加到 *Startup.cs* 的内置容器中：

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

两个中间件均在 `Configure` 的请求处理管道中注册：

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 提供中间件的创建方法。 中间件工厂实现在容器中注册为作用域服务。

可在 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 包（[引用源](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)）中找到默认的 `IMiddlewareFactory` 实现（即[MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory)）。

## <a name="additional-resources"></a>其他资源

* [中间件](xref:fundamentals/middleware/index)
