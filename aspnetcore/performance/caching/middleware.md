---
title: "响应缓存在 ASP.NET 核心中的中间件"
author: guardrex
description: "了解如何配置和 ASP.NET Core 中使用缓存响应的中间件。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/26/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/middleware
ms.openlocfilehash: e9a74d8f6c3945b1bc8c62d0ab21145a7c5717fb
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>响应缓存在 ASP.NET 核心中的中间件

通过[Luke Latham](https://github.com/guardrex)和[John Luo](https://github.com/JunTaoLuo)

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

此文章介绍了如何在 ASP.NET Core 应用程序中配置缓存响应的中间件。 该中间件确定响应何时可缓存、 存储响应和从缓存充当响应。 有关 HTTP 缓存功能的简介和`ResponseCache`属性，请参阅[响应缓存](xref:performance/caching/response)。

## <a name="package"></a>Package

若要在项目中包含该中间件，添加到引用[ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)包或使用[ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)包 (ASP.NET Core 2.0 或更高版本时目标.NET 核心)。

## <a name="configuration"></a>配置

在`ConfigureServices`，将该中间件添加到服务集合。

[!code-csharp[](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

配置应用程序以使用与中间件`UseResponseCaching`扩展方法，将该中间件添加到请求处理管道。 示例应用添加[ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2)最多 10 秒钟来缓存可缓存响应的响应的标头。 该示例发送[ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4)标头来配置用于缓存的响应仅当该中间件[ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4)的后续请求的标头与原始请求相匹配。

[!code-csharp[](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

响应缓存中间件仅缓存导致状态代码为 200 （正常） 的服务器响应。 任何其他响应，包括[错误页](xref:fundamentals/error-handling)，将忽略的中间件。

> [!WARNING]
> 响应包含内容的经过身份验证的客户端必须标记为不可缓存，以防止从存储并提供这些响应中间件。 请参阅[缓存的条件](#conditions-for-caching)有关该中间件如何确定响应是否是可缓存的详细信息。

## <a name="options"></a>选项

该中间件提供三个选项用于控制响应缓存。

| 选项                | 默认值 |
| --------------------- | ------------- |
| UseCaseSensitivePaths | 确定响应将在区分大小写的路径上缓存。</p><p>默认值为 `false`。 |
| MaximumBodySize       | 以字节为单位的响应正文的最大缓存大小。</p>默认值是`64 * 1024 * 1024`(64 MB)。 |
| 大小限制             | 以字节为单位的响应缓存中间件大小限制。 默认值是`100 * 1024 * 1024`(100 MB)。 |

下面的示例将配置到中间件：

* 小于或等于 1024 字节的缓存响应。
* 存储响应的区分大小写的路径 (例如，`/page1`和`/Page1`分开存储)。

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

当使用 MVC/Web API 控制器或 Razor 页页模型，`ResponseCache`属性指定所需的设置适当的标头为响应缓存参数。 唯一参数`ResponseCache`严格需要中间件的属性是`VaryByQueryKeys`，这不对应于实际 HTTP 标头。 有关详细信息，请参阅[ResponseCache 属性](xref:performance/caching/response#responsecache-attribute)。

当未使用`ResponseCache`属性中，响应缓存可能会不同`VaryByQueryKeys`功能。 使用`ResponseCachingFeature`直接从`IFeatureCollection`的`HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

使用单个值等于`*`中`VaryByQueryKeys`随缓存所有请求查询参数而都变化。

## <a name="http-headers-used-by-response-caching-middleware"></a>响应缓存中间件所使用的 HTTP 标头

使用 HTTP 标头配置响应的缓存该中间件。

| Header | 详细信息 |
| ------ | ------- |
| 授权 | 如果标头存在，则不缓存响应。 |
| Cache-Control | 该中间件只考虑缓存标记为响应`public`缓存指令。 控制缓存使用以下参数：<ul><li>最长时间</li><li>max-stale&#8224;</li><li>最小值全新</li><li>must-revalidate</li><li>无缓存</li><li>no-store</li><li>仅当-缓存</li><li>private</li><li>public</li><li>s maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224; 如果没有限制指定到`max-stale`，中间件不执行任何操作。<br>&#8225;`proxy-revalidate`具有相同的效果`must-revalidate`。<br><br>有关详细信息，请参阅[RFC 7231： 请求的缓存控制指令](https://tools.ietf.org/html/rfc7234#section-5.2.1)。 |
| 杂注 | A`Pragma: no-cache`请求标头中的生成相同的效果`Cache-Control: no-cache`。 此标头中的相关指令来重写`Cache-Control`标头，如果存在。 考虑对与 HTTP/1.0 的向后兼容性。 |
| Set-Cookie | 如果标头存在，则不缓存响应。 |
| 改变 | `Vary`标头用于改变缓存的响应的另一个标头。 例如，通过包括通过编码来缓存响应`Vary: Accept-Encoding`标头，来缓存响应的请求标头`Accept-Encoding: gzip`和`Accept-Encoding: text/plain`单独。 标头值为响应`*`永远不会存储。 |
| 过期 | 通过此标头视为过时的响应不存储或检索除非重写由其他`Cache-Control`标头。 |
| If-None-Match | 如果该值不完整的响应从缓存提供`*`和`ETag`的响应中不匹配任何提供的值。 否则，提供 304 （未修改） 响应。 |
| If-Modified-Since | 如果`If-None-Match`标头不存在，如果缓存的响应日期晚于提供的值，完整的响应从缓存中提供。 否则，提供 304 （未修改） 响应。 |
| 日期 | 从缓存提供服务时`Date`由该中间件设置标头，如果它未在原始响应上提供。 |
| Content-Length | 从缓存提供服务时`Content-Length`由该中间件设置标头，如果它未在原始响应上提供。 |
| 保留时间 | `Age`在原始响应中发送的标头将被忽略。 提供缓存的响应时，该中间件将计算新值。 |

## <a name="caching-respects-request-cache-control-directives"></a>缓存遵循请求缓存控制指令

该中间件会遵守的规则[HTTP 1.1 缓存规范](https://tools.ietf.org/html/rfc7234#section-5.2)。 在规则需要遵守是有效的缓存`Cache-Control`客户端发送的标头。 在规范，客户端可以发出请求与`no-cache`标头值和强制服务器生成的每个请求的新响应。 目前，没有任何开发人员控制此缓存行为时使用中间件，因为该中间件符合官方缓存规范。

为了更好地控制缓存行为，将介绍其他缓存功能的 ASP.NET Core。 请参见下面的主题：

* [内存中缓存](xref:performance/caching/memory)
* [使用分布式缓存](xref:performance/caching/distributed)
* [缓存 ASP.NET Core MVC 中的标记帮助器](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分布式缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>疑难解答

如果缓存行为未按预期方式，，确认响应是否可缓存并且能够从缓存中提供。 检查请求的传入标头和响应的传出标头。 启用[日志记录](xref:fundamentals/logging/index)以帮助进行调试。

当测试和故障排除缓存行为，浏览器可能设置影响不可取的方法中的缓存的请求标头。 例如，浏览器可能设置`Cache-Control`标头到`no-cache`或`max-age=0`刷新页面时。 以下工具可以显式设置请求标头和测试缓存的首选：

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>用于缓存的条件

* 请求必须导致服务器响应 200 （正常） 状态代码。
* 请求方法必须是 GET 或 HEAD。
* 终端中间件，如[静态文件中间件](xref:fundamentals/static-files)，必须处理响应缓存中间件之前的响应。
* `Authorization`标头不能存在。
* `Cache-Control` 标头参数必须是有效，并且必须标记为响应`public`和未标记`private`。
* `Pragma: no-cache`标头不能存在如果`Cache-Control`标头不存在，作为`Cache-Control`标头重写`Pragma`标头时存在。
* `Set-Cookie`标头不能存在。
* `Vary` 标头参数必须是有效且不等于`*`。
* `Content-Length`标头值 (如果设置) 必须与匹配的响应正文的大小。
* [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature)未使用。
* 响应不能为指定的陈旧`Expires`标头和`max-age`和`s-maxage`缓存指令。
* 响应缓冲必须成功，并且响应的大小必须小于配置或默认`SizeLimit`。
* 响应必须是可根据缓存[RFC 7234](https://tools.ietf.org/html/rfc7234)规范。 例如，`no-store`指令必须在请求或响应标头字段中存在。 请参阅*第 3 部分： 在缓存中存储响应*的[RFC 7234](https://tools.ietf.org/html/rfc7234)有关详细信息。

> [!NOTE]
> Antiforgery 系统用于生成安全令牌，以防止跨站点请求伪造 (CSRF) 攻击集`Cache-Control`和`Pragma`标头到`no-cache`以便不缓存响应。

## <a name="additional-resources"></a>其他资源

* [应用程序启动](xref:fundamentals/startup)
* [中间件](xref:fundamentals/middleware/index)
* [内存中缓存](xref:performance/caching/memory)
* [使用分布式缓存](xref:performance/caching/distributed)
* [使用更改令牌检测更改](xref:fundamentals/primitives/change-tokens)
* [响应缓存](xref:performance/caching/response)
* [缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分布式缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
