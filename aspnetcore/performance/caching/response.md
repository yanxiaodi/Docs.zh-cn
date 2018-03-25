---
title: "响应缓存在 ASP.NET 核心"
author: rick-anderson
description: "了解如何使用缓存到较低带宽要求的响应，并增加的 ASP.NET Core 应用的性能。"
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/response
ms.openlocfilehash: c654cfd7c2d291849067bfd3297f940018ccb3d8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="response-caching-in-aspnet-core"></a>响应缓存在 ASP.NET 核心

通过[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)

> [!NOTE]
> 响应缓存[Razor 页与 ASP.NET 核心 2.0 中不支持](https://github.com/aspnet/Mvc/issues/6437)。 此功能将支持[ASP.NET 核心 2.1 版本](https://github.com/aspnet/Home/wiki/Roadmap)。
  
[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

响应缓存可减少客户端或代理对 web 服务器的请求数。 响应缓存还可减少量工作的 web 服务器执行程序生成响应。 响应缓存由标头，指定你希望客户端、 代理和缓存响应的中间件如何控制。

添加时，web 服务器可以缓存响应[响应缓存中间件](xref:performance/caching/middleware)。

## <a name="http-based-response-caching"></a>基于 HTTP 的响应缓存

[HTTP 1.1 缓存规范](https://tools.ietf.org/html/rfc7234)介绍 Internet 缓存的行为方式。 用于缓存的主 HTTP 标头是[缓存控制](https://tools.ietf.org/html/rfc7234#section-5.2)，用于指定缓存*指令*。 指令控制缓存行为，请求来自客户端对服务器进行其方法和响应进行地从服务器返回到客户端。 请求和响应将通过代理服务器和代理服务器还必须遵循 HTTP 1.1 缓存功能规范。

常见`Cache-Control`指令下表中所示。

| 指令                                                       | 操作 |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | 缓存可能会存储响应。 |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | 不能通过共享缓存中存储响应。 专用缓存可以存储并重复使用响应。 |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | 客户端将不会接受其保留时间大于指定的秒数的响应。 示例： `max-age=60` （60 秒）， `max-age=2592000` （1 个月） |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **在请求上**： 缓存不得使用存储的响应来满足该请求。 注意： 对于客户端，为源服务器将重新生成响应，并且该中间件更新其缓存中存储的响应。<br><br>**响应**： 响应必须不能用于在不验证源服务器上的后续请求。 |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **在请求上**： 缓存必须不会将请求的存储。<br><br>**响应**： 缓存不得存储任何响应的一部分。 |

起缓存作用其他缓存标头下表所示。

| Header                                                     | 函数 |
| ---------------------------------------------------------- | -------- |
| [保留时间](https://tools.ietf.org/html/rfc7234#section-5.1)     | 以秒为单位由于生成或者在源服务器已成功验证响应的时间量的估计值。 |
| [Expires](https://tools.ietf.org/html/rfc7234#section-5.3) | 日期/时间后响应被视为是陈旧。 |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | 存在向后兼容性与 HTTP/1.0 缓存设置`no-cache`行为。 如果`Cache-Control`标头是否存在、`Pragma`标头将被忽略。 |
| [改变](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | 指定缓存的响应必须不发送除非所有的`Vary`中缓存的响应的原始请求和新的请求标头字段所匹配。 |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>基于 HTTP 的缓存方面请求的缓存控制指令

[HTTP 1.1 缓存的缓存控制标头的规范](https://tools.ietf.org/html/rfc7234#section-5.2)需要遵守是有效的缓存`Cache-Control`客户端发送的标头。 客户端可以发出请求与`no-cache`标头值和强制服务器生成的每个请求的新响应。

始终考虑客户端`Cache-Control`请求标头是有意义，如果你考虑 HTTP 缓存功能的目标。 官方规范中，在缓存旨在减少在网络中客户端、 代理和服务器满足请求的延迟和网络的开销。 它不一定是一种方法来控制的源服务器上的负载。

没有对此缓存行为的当前开发人员控件时使用[响应缓存中间件](xref:performance/caching/middleware)因为缓存规范正式遵循该中间件。 [将来对该中间件增强](https://github.com/aspnet/ResponseCaching/issues/96)配置中间件，若要忽略的请求将允许`Cache-Control`时决定提供缓存的响应的标头。 当你使用该中间件，这将为您提供更好地控制负载在你的服务器上的机会。

## <a name="other-caching-technology-in-aspnet-core"></a>在 ASP.NET 核心中其他缓存技术

### <a name="in-memory-caching"></a>内存中缓存

内存中缓存使用的服务器内存来存储缓存的数据。 此类型的缓存适合于单个服务器或多个服务器使用*粘性会话*。 粘性会话意味着，客户端的请求始终路由至同一服务器进行处理。

有关详细信息，请参阅[简介内存中缓存中 ASP.NET Core](xref:performance/caching/memory)。

### <a name="distributed-cache"></a>分布式的缓存

使用分布式的缓存时云或服务器场中承载应用程序数据存储在内存中。 处理请求的服务器之间共享缓存。 客户端可以提交用于客户端的缓存的数据是否可用的组中的任何服务器处理的请求。 ASP.NET Core 提供 SQL Server 和分布式的 Redis 缓存。

有关详细信息，请参阅[使用分布式缓存](xref:performance/caching/distributed)。

### <a name="cache-tag-helper"></a>缓存标记帮助器

你可以使用缓存标记帮助器缓存中的 MVC 视图或 Razor 页的内容。 缓存标记帮助器使用的内存中缓存来存储数据。

有关详细信息，请参阅[ASP.NET 核心 mvc 缓存标记帮助器](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。

### <a name="distributed-cache-tag-helper"></a>分布式缓存标记帮助程序

你可以使用分布式缓存标记帮助程序缓存的 MVC 视图或分布式的云或 web 场方案中的 Razor 页面中的内容。 分布式缓存标记帮助器使用 SQL Server 或 Redis 来存储数据。

有关详细信息，请参阅[分布式缓存标记帮助器](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)。

## <a name="responsecache-attribute"></a>ResponseCache 属性

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)指定在缓存中，响应设置适当的标头的所需的参数。

> [!WARNING]
> 禁用缓存的内容，其中包含已经过身份验证的客户端的信息。 仅应为不会更改基于用户的标识或用户已登录的内容启用缓存。

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)存储的响应因给定的查询密钥列表的值。 时的单个值`*`是响应的所有请求查询字符串参数提供该中间件而异。 `VaryByQueryKeys` 需要 ASP.NET Core 1.1 或更高版本。

必须启用响应缓存中间件设置`VaryByQueryKeys`属性; 否则，会引发一个运行时异常。 没有为相应的 HTTP 标头`VaryByQueryKeys`属性。 该属性是一项 HTTP 功能由响应缓存中间件。 若要提供缓存的响应的中间件，对于查询字符串和查询字符串值必须匹配上一个请求。 例如，考虑的请求和下表中显示结果的序列。

| 请求                          | 结果                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | 从服务器返回     |
| `http://example.com?key1=value1` | 返回从中间件 |
| `http://example.com?key1=value2` | 从服务器返回     |

第一个请求是由服务器返回，并在中间件中缓冲。 由于查询字符串与上一个请求，中间件会返回第二个请求。 因为查询字符串值不匹配的上一个请求，第三个请求未处于中间件缓存。 

`ResponseCacheAttribute`用于配置和创建 (通过`IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)。 `ResponseCacheFilter`执行的工作的更新的相应 HTTP 标头和响应功能。 筛选器：

* 删除任何现有标头`Vary`， `Cache-Control`，和`Pragma`。 
* 写出相应的标头设置的属性上基于`ResponseCacheAttribute`。 
* 更新缓存中项 HTTP 功能，如果响应`VaryByQueryKeys`设置。

### <a name="vary"></a>改变

此标头只会写入时`VaryByHeader`属性设置。 设置为`Vary`属性的值。 下面的示例使用`VaryByHeader`属性：

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

你可以查看你的浏览器的网络工具的响应标头。 下图显示了输出上边缘 F12**网络**选项卡上时`About2`刷新操作方法：

![在网络选项卡上边缘 F12 输出，当调用 About2 操作方法](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore 和 Location.None

`NoStore` 重写的大多数其他属性。 当此属性设置为`true`、`Cache-Control`标头设置为`no-store`。 如果`Location`设置为`None`:

* 将 `Cache-Control` 设置为 `no-store,no-cache`。
* 将 `Pragma` 设置为 `no-cache`。

如果`NoStore`是`false`和`Location`是`None`，`Cache-Control`和`Pragma`设置为`no-cache`。

通常情况下设置`NoStore`到`true`错误页上。 例如:

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

这将导致以下标头：

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>位置和持续时间

若要启用缓存，`Duration`必须设置为正值和`Location`必须是`Any`（默认值） 或`Client`。 在这种情况下，`Cache-Control`标头设置为位置值后跟`max-age`的响应。

> [!NOTE]
> `Location`选项的`Any`和`Client`将转换`Cache-Control`的标头值`public`和`private`分别。 按前面所述，设置`Location`到`None`设置同时`Cache-Control`和`Pragma`标头到`no-cache`。

下面生成通过设置一个示例，演示标头`Duration`并保留默认值`Location`值：

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

这将产生以下标头：

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>缓存配置文件

而不是复制`ResponseCache`设置 MVC 中时，可以将许多控制器操作属性而言，缓存配置文件上的设置配置为选项`ConfigureServices`中的方法`Startup`。 在引用的缓存配置文件中找到值用作通过默认`ResponseCache`属性和重写了指定的属性上的任何属性。

设置缓存配置文件：

[!code-csharp[](response/sample/Startup.cs?name=snippet1)] 

引用缓存配置文件：

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

`ResponseCache`特性可应用于操作 （方法） 和控制器 （类）。 方法级别的属性重写在类级别特性中指定的设置。

在上面的示例中，类级别属性指定持续时间为 30 秒，而方法级别属性引用与设置为 60 秒的持续时间的缓存配置文件。

生成的标头：

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>其他资源

* [在缓存中 HTTP 规范中](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [内存中缓存](xref:performance/caching/memory)
* [使用分布式缓存](xref:performance/caching/distributed)
* [使用更改令牌检测更改](xref:fundamentals/primitives/change-tokens)
* [响应缓存中间件](xref:performance/caching/middleware)
* [缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分布式缓存标记帮助程序](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
