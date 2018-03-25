---
title: "在 ASP.NET Core 应用程序实施 HTTPS"
author: rick-anderson
description: "演示如何要求在 ASP.NET Core HTTPS/TLS web 应用。"
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>在 ASP.NET Core 应用程序实施 HTTPS

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文档说明如何：

- 所有请求需要 HTTPS。
- 所有 HTTP 请求重都定向到 HTTPS。

> [!WARNING]
> 执行**不**使用`RequireHttpsAttribute`接收敏感信息的 Web api。 `RequireHttpsAttribute` 使用 HTTP 状态代码将从 HTTP 到 HTTPS 的浏览器重定向。 API 客户端可能无法理解或遵循从 HTTP 到 HTTPS 的重定向。 此类客户端可能会通过 HTTP 发送信息。 Web Api 应具有下列任一：
>
>* 不在 HTTP 上侦听。
>* 关闭与状态代码 400 （错误请求） 的连接并不为请求提供服务。

## <a name="require-https"></a>需要 HTTPS

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute)用于需要 HTTPS。 `[RequireHttpsAttribute]` 可修饰控制器或方法，或可以全局应用。 若要全局应用该属性，以下代码添加到`ConfigureServices`中`Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

前面的突出显示的代码中需要的所有请求都使用`HTTPS`; 因此，HTTP 请求将被忽略。 以下突出显示的代码将所有 HTTP 请求重都定向到 HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

有关详细信息，请参阅[URL 重写中间件](xref:fundamentals/url-rewriting)。

全局需要 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是最佳安全方案。 应用`[RequireHttps]`特性应用到所有控制器/Razor 页面不会被视为尽可能安全全局需要 HTTPS。 你不能保证`[RequireHttps]`添加新控制器和 Razor 页时应用特性。