---
title: "ASP.NET Core 中的会话和应用程序状态"
author: rick-anderson
description: "保留请求之间的应用程序和用户（会话）状态的方法。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: f4ed38f7395e3f4fe939584c1f3f5b0dba93724c
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>ASP.NET Core 中的会话和应用程序状态简介

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/) 和 [Diana LaRose](https://github.com/DianaLaRose)

HTTP 是无状态的协议。 Web 服务器将每个 HTTP 请求视为独立的请求，并不会保留以前请求中的用户值。 本文讨论保留请求之间的应用程序和会话状态的不同方式。 

## <a name="session-state"></a>会话状态

会话状态是 ASP.NET Core 中的一项功能，可用于在用户浏览 Web 应用时保存和存储用户数据。 会话状态由服务器上的字典或哈希表组成，在浏览器的请求中保持数据。 会话数据由缓存支持。

ASP.NET Core 通过向客户端提供包含会话 ID 的 Cookie 来维护会话状态，该会话 ID 与每个请求一起发送到服务器。 服务器使用会话 ID 来获取会话数据。 因为会话 Cookie 是特定于浏览器的，所以不能跨浏览器中共享会话。 仅当浏览器会话结束时才能删除会话 Cookie。 如果收到过期的会话 Cookie，则创建使用相同会话 Cookie 的新会话。 

服务器在上次请求后保留会话的时间有限。 设置会话超时，或者使用 20 分钟的默认值。 会话状态非常适合用于存储特定于特定会话但并不需要永久保留的用户数据。 在调用 `Session.Clear` 时或数据存储中会话到期时将删除后备存储中的数据。 服务器不知道关闭浏览器或删除会话 Cookie 的时间。

> [!WARNING]
> 请勿将敏感数据存储在会话中。 客户端可能不会关闭浏览器并清除会话 Cookie（某些浏览器在多个窗口中保持会话 Cookie）。 另外，会话可能不限于单个用户；下一个用户可能继续同一个会话。

内存中会话提供程序将会话数据存储在本地服务器上。 如果计划在服务器场上运行 Web 应用，则必须使用粘性会话将每个会话绑定到特定的服务器上。 Microsoft Azure 网站平台默认设置为粘性会话（应用程序请求路由或 ARR）。 然而，粘性会话可能会影响可伸缩性，并使 Web 应用更新变得复杂。 更好的选择是使用 Redis 或 SQL Server 分布式缓存，它们不需要粘性会话。 有关详细信息，请参阅[使用分布式缓存](xref:performance/caching/distributed)。 有关服务提供程序设置的详细信息，请参阅本文后续部分中的[配置会话](#configuring-session)。

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET Core MVC 在[控制器](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0)上公开了 [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 属性。 此属性存储未读取的数据。 `Keep` 和 `Peek` 方法可用于检查数据，而不执行删除。 多个请求需要数据时，`TempData` 非常有助于进行重定向。 通过 TempData 提供程序实现 `TempData`，例如，使用 Cookie 或会话状态。

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>TempData 提供程序

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 及更高版本中，基于 Cookie 的 TempData 提供程序在默认情况下使用，将 TempData 存储在 Cookie 中。

使用 [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0) 对 Cookie 数据进行编码。 因为 Cookie 被加密并分块，所以 ASP.NET Core 1.x 中的单个 Cookie 大小限制不适用。 未压缩 Cookie 数据，因为压缩加密的数据会导致安全问题，如 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 和 [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻击。 有关基于 Cookie 的 TempData 提供程序的详细信息，请参阅 [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在 ASP.NET Core 1.0 和 1.1 中，会话状态 TempData 提供程序是默认值。

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>选择 TempData 提供程序

选择 TempData 提供程序涉及几个注意事项，例如：

1. 应用程序是否已经将会话状态用于其他目的？ 如果是，使用会话状态 TempData 提供程序对应用程序没有额外的成本（除了数据的大小）。
2. 应用程序是否只对相对较小的数据量（最多 500 个字节）使用 TempData？ 如果是，Cookie TempData 提供程序将为每个携带 TempData 的请求增加较小的成本。 如果不是，会话状态 TempData 提供程序有助于在使用 TempData 前，避免在每个请求中来回切换大量数据。
3. 应用程序是否在 Web 场（多个服务器）中运行？ 如果是，则使用 Cookie TempData 提供程序无需额外配置。

> [!NOTE]
> 大多数 Web 客户端（如 Web 浏览器）针对每个 Cookie 的最大大小和/或 Cookie 总数强制实施限制。 因此，使用 Cookie TempData 提供程序时，请验证应用程序未超过这些限制。 请考虑数据的总大小，将加密和分块的开销考虑在内。

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>配置 TempData 提供程序

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

默认情况下启用基于 Cookie 的 TempData 提供程序。 以下 `Startup` 类代码配置基于会话的 TempData 提供程序：

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

以下 `Startup` 类代码配置基于会话的 TempData 提供程序：

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

排序对于中间件组件至关重要。 在前面的示例中，在 `UseMvcWithDefaultRoute` 之后调用 `UseSession` 时会发生 `InvalidOperationException` 类型的异常。 有关详细信息，请参阅[中间件排序](xref:fundamentals/middleware/index#ordering)。

> [!IMPORTANT]
> 如果面向 .NET Framework 和使用基于会话的提供程序，将 [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet 包添加到项目。

## <a name="query-strings"></a>查询字符串

可以将有限的数据从一个请求传递到另一个请求，方法是将其添加到新请求的查询字符串中。 这有利于以一种持久的方式捕获状态，这种方式允许通过电子邮件或社交网络共享嵌入式状态的链接。 但是，为此，不可将查询字符串用于敏感数据。 在查询字符串中包含数据除了易于共享，还为[跨站点请求伪造 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻击创造了机会，可以欺骗用户在通过身份验证时访问恶意网站。 然后，攻击者可以从应用程序中窃取用户数据，或者代表用户采取恶意操作。 任何保留的应用程序或会话状态必须防止 CSRF 攻击。 有关 CSRF 攻击的详细信息，请参阅[在 ASP.NET Core 中预防跨网站请求伪造 (XSRF/CSRF) 攻击](../security/anti-request-forgery.md)。

## <a name="post-data-and-hidden-fields"></a>后期数据和隐藏字段

数据可以保存在隐藏的表单域中，并在下一个请求上回发。 这在多页窗体中很常见。 但是，由于客户端可能会篡改数据，因此服务器必须始终重新验证数据。 

## <a name="cookies"></a>Cookie

Cookie 提供了一种在 Web 应用程序中存储用户特定数据的方法。 因为 Cookie 是随每个请求发送的，所以它们的大小应该保持在最低限度。 理想情况下，仅标识符应存储在 Cookie 中，而实际数据存储在服务器上。 大多数浏览器将 Cookie 限制为 4096 个字节。 此外，每个域仅有有限数量的 Cookie 可用。  

因为 Cookie 易被篡改，所以它们必须在服务器上进行验证。 尽管在客户端的 Cookie 的持续性是受用户干预并到期，但它们通常是客户端上最持久的数据持久形式。

Cookie 通常用于个性化设置，其中的内容是为已知用户定制的。 因为在大多数情况下，用户仅被标识且未经过身份验证，所以通常可以通过在 Cookie 中存储用户名、帐户名或唯一用户 ID（例如 GUID）来保护 Cookie。 然后可以使用 Cookie 来访问站点的用户个性化设置基础结构。

## <a name="httpcontextitems"></a>HttpContext.Items

`Items` 集合是存储仅在处理一个特定请求时才需要的数据的理想位置。 集合的内容在每次请求后被放弃。 `Items` 集合最好用作组件或中间件在请求期间在不同时间点操作且没有直接传递参数的方法时进行通信的方式。 有关详细信息，请参阅本文后面的[使用 HttpContext.Items](#working-with-httpcontextitems)。

## <a name="cache"></a>缓存

缓存是存储和检索数据的有效方法。 可以根据时间和其他注意事项控制缓存项的生存期。 了解有关 [缓存](../performance/caching/index.md)的详细信息。

<a name="session"></a>
## <a name="working-with-session-state"></a>使用会话状态

### <a name="configuring-session"></a>配置会话

`Microsoft.AspNetCore.Session` 包提供用于管理会话状态的中间件。 若要启用会话中间件，`Startup` 必须包含：

- 任一 [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) 内存缓存。 `IDistributedCache` 实现用作会话后备存储。
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 调用，要求 NuGet 包“Microsoft.AspNetCore.Session”。
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) 调用。

下面的代码演示如何设置内存中的会话提供程序。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

一旦 `HttpContext` 安装并配置，可以从它引用会话。

如果在 `UseSession` 被调用前尝试访问 `Session`，将引发异常 `InvalidOperationException: Session has not been configured for this application or request`。

如果在已经开始写入 `Response` 流后尝试创建一个新 `Session`（即，未创建会话 Cookie），将引发异常 `InvalidOperationException: The session cannot be established after the response has started`。 此异常可在 Web 服务器日志中找到；它将不会在浏览器中显示。

### <a name="loading-session-asynchronously"></a>以异步方式加载会话 

只有在 `TryGetValue`、`Set` 或 `Remove` 方法之前显式调用 [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) 方法，ASP.NET Core 中的默认会话提供程序才会从基础 [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) 存储以异步方式加载会话记录。 如果不首先调用 `LoadAsync`，基础会话记录以同步方式加载，这可能影响应用的扩展能力。

若要让应用程序强制实施此模式，如果未在`TryGetValue`、`Set` 或 `Remove`之前调用 `LoadAsync` 方法，将 [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) 和 [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession)实现包装在引起异常的版本。 在服务容器中注册的已包装的版本。

### <a name="implementation-details"></a>实现的详细信息

会话使用 Cookie 跟踪和标识来自单个浏览器的请求。 默认情况下，此 Cookie 名为“.AspNet.Session”，并使用路径“/”。 因为 Cookie 默认值不指定域，所以它不提供页上的客户端脚本（因为 `CookieHttpOnly` 默认为 `true`）。

若要重写会话默认值，使用 `SessionOptions`：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

服务器使用 `IdleTimeout` 属性来确定在放弃会话内容之前可以保持空闲的时间长短。 此属性独立于 Cookie 到期时间。 通过会话中间件（从会话中间件读取或写入）传递每个请求都会重置超时。

因为 `Session` 是非锁定的，如果两个请求都试图修改会话内容，最后一个内容会重写第一个内容。 `Session` 是作为一个连贯会话实现的，这意味着所有内容都存储在一起。 正在修改会话的不同部分（不同键）的两个请求仍可能会相互影响。

### <a name="setting-and-getting-session-values"></a>设置和获取会话值

通过 `HttpContext` 的 `Session` 属性访问会话。 此属性是 [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) 实现。

下面的示例演示了设置和获取 int 和字符串：

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

如果添加以下扩展方法，可以设置并获取可序列化的对象到会话：

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

下面的示例演示如何设置和获取可序列化的对象：

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>使用 HttpContext.Items

`HttpContext` 抽象为名为 `Items` 的 `IDictionary<object, object>` 类型字典集合提供支持。 此集合在 HttpRequest 开始时可用并在每个请求的末尾被放弃。 可以通过给键控的项分配值或为特定键请求值来访问它。

在下面示例中，[中间件](xref:fundamentals/middleware/index)将 `isVerified` 添加到 `Items` 集合。

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

在更高版本的管道中，另一个中间件无法访问它：

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

对于只供单个应用程序使用的中间件，`string` 键是可以接受的。 但是，应用程序之间共享的中间件应使用唯一的对象键以避免键冲突的可能性。 如果正在开发必须跨多个应用程序工作的中间件，使用中间件类中定义的唯一对象键，如下所示：

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

其他代码可以使用通过中间件类公开的键访问存储在 `HttpContext.Items` 中的值：

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

这种方法还具有消除代码中多个位置重复使用“神奇字符串”的优点。

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>应用程序状态数据

使用[依赖关系注入](xref:fundamentals/dependency-injection)可向所有用户提供数据：

1. 定义包含数据的服务（例如，一个名为 `MyAppData` 的类）。

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. 添加服务类到 `ConfigureServices`（例如 `services.AddSingleton<MyAppData>();`）。
3. 使用每个控制器中的数据服务类：

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>使用会话时的常见错误

* “在尝试激活‘Microsoft.AspNetCore.Session.DistributedSessionStore’时无法为类型‘Microsoft.Extensions.Caching.Distributed.IDistributedCache’解析服务。”

  这通常是由于不能配置至少一个 `IDistributedCache` 实现而造成的。 有关详细信息，请参阅[使用分布式缓存](xref:performance/caching/distributed)和[内存缓存中](xref:performance/caching/memory)。

* 如果会话中间件无法保留会话（例如：如果数据库不可用），它将记录并吞并异常。 然后，请求将继续正常运行，这会导致非常难以预料的行为。

典型示例：

有人将购物篮存储在会话中。 用户添加项但提交失败。 应用不知道失败，因此报告消息“已添加项”，然而并不是如此。

检查是否存在此类错误的建议方法是完成写入到该会话后从应用代码调用 `await feature.Session.CommitAsync();`。 然后就可以随意处理错误。 调用 `LoadAsync` 时同样适用。

### <a name="additional-resources"></a>其他资源

* [ASP.NET Core 1.x：本文档中使用的代码示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x：本文档中使用的代码示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
