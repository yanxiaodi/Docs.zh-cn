---
title: "使用 Cookie 而无需 ASP.NET 核心标识的身份验证"
author: rick-anderson
description: "使用 cookie 而无需 ASP.NET 核心标识的身份验证的说明"
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: bbc49a0d3ede66ad07ec3f1dea055cae5fec39ff
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>使用 Cookie 而无需 ASP.NET 核心标识的身份验证

通过[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

如你所见在早期的身份验证主题中， [ASP.NET 核心标识](xref:security/authentication/identity)完成、 功能完备的身份验证提供程序是用于创建和维护登录名。 但是，你可能想要使用基于 cookie 的身份验证有时使用您自己的自定义身份验证逻辑。 你可以使用基于 cookie 的身份验证作为独立身份验证提供程序，没有 ASP.NET 核心标识。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample)（[如何下载](xref:tutorials/index#how-to-download-a-sample)）

有关从 ASP.NET Core 迁移基于 cookie 的身份验证信息 1.x 到 2.0，请参阅[迁移身份验证和标识到 ASP.NET 核心 2.0 主题 （基于 Cookie 的身份验证）](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)。

## <a name="configuration"></a>配置

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果你未使用[Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)，安装 2.0 以上版本的[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet 包。

在`ConfigureServices`方法，创建具有的身份验证中间件服务`AddAuthentication`和`AddCookie`方法：

[!code-csharp[](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme` 传递给`AddAuthentication`设置应用程序的默认身份验证方案。 `AuthenticationScheme` 当有多个实例的 cookie 身份验证，并且希望时非常有用[授权与特定方案](xref:security/authorization/limitingidentitybyscheme)。 设置`AuthenticationScheme`到`CookieAuthenticationDefaults.AuthenticationScheme`方案提供的"Cookie"的值。 你可以提供任何字符串值，用于区分方案。

在`Configure`方法，请使用`UseAuthentication`方法来调用设置身份验证中间件`HttpContext.User`属性。 调用`UseAuthentication`方法之前调用`UseMvcWithDefaultRoute`或`UseMvc`:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet2)]

**AddCookie 选项**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)类用于配置身份验证提供程序选项。

| 选项 | 描述 |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | 提供的路径以提供 302 找到 （URL 重定向） 时触发的`HttpContext.ForbidAsync`。 默认值为 `/Account/AccessDenied`。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | 要用于颁发者[颁发者](/dotnet/api/system.security.claims.claim.issuer)cookie 身份验证服务创建的任何声明上的属性。 |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Cookie 提供服务位置的域名。 默认情况下，这是请求的主机名。 浏览器仅将 cookie 在请求中发送到匹配的主机名。 你可能希望调整这在你的域中具有可用于任何主机的 cookie。 例如，将 cookie 域设置为`.contoso.com`使其可供`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。 |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | 获取或设置一个 cookie 的生存期。 目前，此选项没有 ops，并且将会过时中 ASP.NET Core 2.1 +。 使用`ExpireTimeSpan`选项设置 cookie 到期时间。 有关详细信息，请参阅[阐明 CookieAuthenticationOptions.Cookie.Expiration 行为](https://github.com/aspnet/Security/issues/1293)。 |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | 一个标志，指示该 cookie 应仅服务器访问。 更改此值与`false`允许客户端脚本来访问 cookie 和可能打开你的应用 cookie 被盗应你的应用具备[跨站点脚本 (XSS)](xref:security/cross-site-scripting)漏洞。 默认值为 `true`。 |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | 设置的 cookie 的名称。 |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | 用于隔离在相同的主机名上运行的应用程序。 如果必须在运行的应用程序`/app1`并且想要限制对该应用的 cookie，请将设置`CookiePath`属性`/app1`。 通过这样做，cookie 是仅可在上找到对请求`/app1`和其下的任何应用程序。 |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | 该值指示浏览器应允许 cookie 要附加到同一站点的请求 (`SameSiteMode.Strict`) 或使用安全的 HTTP 方法和同一站点请求的跨站点请求 (`SameSiteMode.Lax`)。 当设置为`SameSiteMode.None`，未设置的 cookie 标头值。 请注意， [Cookie 策略中间件](#cookie-policy-middleware)可能会覆盖你提供的值。 若要支持 OAuth 身份验证，默认值是`SameSiteMode.Lax`。 有关详细信息，请参阅[OAuth 身份验证由于 SameSite cookie 策略中断](https://github.com/aspnet/Security/issues/1231)。 |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | 一个标志，指示是否创建的 cookie 应被限制为 HTTPS (`CookieSecurePolicy.Always`)，HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或请求的同一个协议 (`CookieSecurePolicy.SameAsRequest`)。 默认值为 `CookieSecurePolicy.SameAsRequest`。 |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | 集`DataProtectionProvider`用于创建默认值`TicketDataFormat`。 如果`TicketDataFormat`设置属性，`DataProtectionProvider`选项未使用。 如果未提供，则使用应用程序的默认数据保护提供程序。 |
| [事件](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | 该处理程序在处理中的某些点提供的应用程序控制的提供程序上调用方法。 如果`Events`不提供的默认实例提供的方法在调用时不执行任何操作。 |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | 用作服务类型，以获取`Events`实例而不是属性。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan`后将存储在 cookie 的身份验证票证到期。 `ExpireTimeSpan` 将添加到当前时间来创建票证的到期时间。 `ExpiredTimeSpan`值始终将验证由服务器加密 AuthTicket 进入。 它可能还进入[Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)标头，但仅当`IsPersistent`设置。 若要设置`IsPersistent`到`true`，配置[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)传递给`SignInAsync`。 默认值`ExpireTimeSpan`为 14 天。 |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | 提供的路径以提供 302 找到 （URL 重定向） 时触发的`HttpContext.ChallengeAsync`。 当前生成 401 的 URL 添加到`LoginPath`作为查询字符串参数由名为`ReturnUrlParameter`。 一次请求`LoginPath`授予新登录标识，`ReturnUrlParameter`值用于将浏览器重定向回导致原始的未授权的状态代码的 URL。 默认值为 `/Account/Login`。 |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | 如果`LogoutPath`然后对该路径的请求重定向到处理程序，提供的值基于`ReturnUrlParameter`。 默认值为 `/Account/Logout`。 |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | 确定追加通过处理程序 302 找到 （URL 重定向） 响应的查询字符串参数的名称。 `ReturnUrlParameter` 请求到达时，将使用`LoginPath`或`LogoutPath`以返回到原始的 URL 的浏览器之后执行的登录或注销操作。 默认值为 `ReturnUrl`。 |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | 用来存储标识跨请求可选容器。 使用时，仅一个会话标识符是发送到客户端。 `SessionStore` 可以用于缓解的大型标识潜在问题。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | 应动态发出一个标志，指示如果具有更新的到期时间的新 cookie。 这可能发生在当前的 cookie 过期时间已超过 50%过期任何请求。 向前移动新的到期日期为当前日期加上`ExpireTimespan`。 [绝对 cookie 到期时间](xref:security/authentication/cookie#absolute-cookie-expiration)可以通过使用设置`AuthenticationProperties`类调用时`SignInAsync`。 绝对到期时间可通过限制的身份验证 cookie 是有效的时间量来提高您的应用程序的安全性。 默认值为 `true`。 |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat`用于保护和取消保护的标识以及存储在 cookie 值中的其他属性。 如果未提供，`TicketDataFormat`使用创建[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)。 |
| [验证](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | 检查的选项为有效的方法。 |

设置`CookieAuthenticationOptions`中的身份验证的服务配置中`ConfigureServices`方法：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET 核心 1.x 使用 cookie[中间件](xref:fundamentals/middleware/index)，序列化为一个用户主体的加密 cookie。 在后续请求中，验证 cookie，并重新创建主体并将其分配到`HttpContext.User`属性。

安装[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)项目中的 NuGet 包。 此程序包包含 cookie 中间件。

使用`UseCookieAuthentication`中的方法`Configure`方法在你*Startup.cs*文件，然后`UseMvc`或`UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**CookieAuthenticationOptions 选项**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)类用于配置身份验证提供程序选项。

| 选项 | 描述 |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | 设置身份验证方案。 `AuthenticationScheme` 当有多个实例的身份验证，并且希望时非常有用[授权与特定方案](xref:security/authorization/limitingidentitybyscheme)。 设置`AuthenticationScheme`到`CookieAuthenticationDefaults.AuthenticationScheme`方案提供的"Cookie"的值。 你可以提供任何字符串值，用于区分方案。 |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | 设置一个值以指示应在每个请求上运行并且尝试验证并重新构造它创建的任何序列化的主体 cookie 身份验证。 |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | 如果为 true，则身份验证中间件处理自动挑战。 如果为 false，身份验证中间件仅更改响应时显式由`AuthenticationScheme`。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | 要用于颁发者[颁发者](/dotnet/api/system.security.claims.claim.issuer)创建 cookie 身份验证中间件的任何声明上的属性。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Cookie 提供服务位置的域名。 默认情况下，这是请求的主机名。 浏览器只为服务为匹配的主机名称的 cookie。 你可能希望调整这在你的域中具有可用于任何主机的 cookie。 例如，将 cookie 域设置为`.contoso.com`使其可供`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | 一个标志，指示该 cookie 应仅服务器访问。 更改此值与`false`允许客户端脚本来访问 cookie 和可能打开你的应用 cookie 被盗应你的应用具备[跨站点脚本 (XSS)](xref:security/cross-site-scripting)漏洞。 默认值为 `true`。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | 用于隔离在相同的主机名上运行的应用程序。 如果必须在运行的应用程序`/app1`并且想要限制对该应用的 cookie，请将设置`CookiePath`属性`/app1`。 通过这样做，cookie 是仅可在上找到对请求`/app1`和其下的任何应用程序。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | 一个标志，指示是否创建的 cookie 应被限制为 HTTPS (`CookieSecurePolicy.Always`)，HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或请求的同一个协议 (`CookieSecurePolicy.SameAsRequest`)。 默认值为 `CookieSecurePolicy.SameAsRequest`。 |
| [说明](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | 有关提供给应用程序的身份验证类型的其他信息。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan`直到身份验证票证到期。 它添加到当前时间来创建票证的到期时间。 若要使用`ExpireTimeSpan`，必须设置`IsPersistent`到`true`中`AuthenticationProperties`传递给`SignInAsync`。 默认值为 14 天。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | 一个标志，指示是否重置时超过一半的 cookie 的到期日期`ExpireTimeSpan`时间间隔。 新的 exipiration 时间向前移动，为当前日期加上`ExpireTimespan`。 [绝对 cookie 到期时间](xref:security/authentication/cookie#absolute-cookie-expiration)可以通过使用设置`AuthenticationProperties`类调用时`SignInAsync`。 绝对到期时间可通过限制的身份验证 cookie 是有效的时间量来提高您的应用程序的安全性。 默认值为 `true`。 |

设置`CookieAuthenticationOptions`为在 Cookie 身份验证中间件`Configure`方法：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Cookie 策略中间件

[Cookie 策略中间件](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)启用应用程序中的 cookie 策略功能。 将该中间件添加到应用程序处理管道是顺序敏感;它只影响在管道中后它注册的组件。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)到 Cookie 策略中间件提供可用于控制到 cookie 处理处理程序的 cookie 处理和挂钩全局特性，在追加或删除 cookie 的时间。

| 属性 | 描述 |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | 会影响是否必须为 cookie HttpOnly，该值一个标志，指示是否 cookie 应只允许到服务器可访问。 默认值为 `HttpOnlyPolicy.None`。 |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | 影响 cookie 的同一站点属性 （见下文）。 默认值为 `SameSiteMode.Lax`。 此选项仅供 ASP.NET Core 2.0 +。 |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | 当追加 cookie 时调用。 |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | 当删除 cookie 时调用。 |
| [安全](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | 会影响是否必须安全 cookie。 默认值为 `CookieSecurePolicy.None`。 |

**MinimumSameSitePolicy** （ASP.NET 2.0 + 仅内核）

默认值`MinimumSameSitePolicy`值是`SameSiteMode.Lax`允许 OAuth2 身份验证。 若要严格强制执行的同一站点策略`SameSiteMode.Strict`，将其设置`MinimumSameSitePolicy`。 虽然此设置将中断 OAuth2 和其他跨域身份验证方案，它将提升其他类型的应用程序不依赖于跨域请求处理的 cookie 安全的级别。

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Cookie 策略中间件设置`MinimumSameSitePolicy`可能会影响你的设置的`Cookie.SameSite`中`CookieAuthenticationOptions`根据下面的矩阵的设置。

| MinimumSameSitePolicy | Cookie.SameSite | 最终的 Cookie.SameSite 设置 |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>创建身份验证 cookie

若要创建一个保存用户信息的 cookie，您必须先构造[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)。 序列化用户信息并将其存储在 cookie 中。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

创建[ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)与任何所需[声明](/dotnet/api/system.security.claims.claim)s 并调用[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)若要在用户登录：

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

调用[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)若要在用户登录：

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync` 创建一个加密的 cookie，并将其添加到当前响应。 如果没有指定`AuthenticationScheme`，则使用默认方案。

事实上，使用的加密是 ASP.NET Core[数据保护](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)系统。 如果您正在承载应用程序在多台计算机、 负载平衡应用程序，或使用 web 场，则必须[配置数据保护](xref:security/data-protection/configuration/overview)使用同一密钥链和应用程序标识符。

## <a name="signing-out"></a>注销

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要注销当前用户，并删除其 cookie，调用[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要注销当前用户，并删除其 cookie，调用[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

如果你未使用`CookieAuthenticationDefaults.AuthenticationScheme`（或"Cookie"） 的方案 (例如，"ContosoCookie")，提供你在配置身份验证提供程序时使用的方案。 否则，使用的默认方案。

## <a name="reacting-to-back-end-changes"></a>对后端更改的作出反应

一旦创建 cookie，它将成为标识的单个来源。 即使在后端系统中禁用用户，cookie 身份验证系统不了解，并且用户一直保持登录，只要其 cookie 的有效期。

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal)中 ASP.NET 核心事件 2.x 或[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1)中 ASP.NET Core 1.x 可用来截获和重写的 cookie 身份验证方法。 这种方法可以降低吊销用户访问应用程序的风险。

Cookie 验证的一种方法取决于跟踪的用户数据库已更改时。 如果颁发用户的 cookie 以来，数据库没有被更改，，则无需重新进行用户身份验证其 cookie 是否仍有效。 若要实现此方案中，数据库中实现`IUserRepository`对于此示例中，存储`LastChanged`值。 在数据库中，更新的任何用户时`LastChanged`值设置为当前时间。

为了使 cookie 无效时的数据库更改基于`LastChanged`值时，请创建具有 cookie`LastChanged`声明包含当前`LastChanged`从数据库的值：

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要实现的替代`ValidatePrincipal`事件，带有以下签名的类中派生自方法写入[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

示例如下所示：

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

在中的 cookie 服务注册期间注册的事件实例`ConfigureServices`方法。 提供有关指定了作用域的服务注册你`CustomCookieAuthenticationEvents`类：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要实现的替代`ValidateAsync`事件，写入具有以下签名的方法：

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET 核心标识作为的一部分实现这一检查其[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)。 示例如下所示：

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

在中的 cookie 的身份验证配置期间注册事件`Configure`方法：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

请考虑在其中更新用户的名称的情况下&mdash;决策不会影响任何方式中的安全性。 如果你想要非破坏性地更新的用户主体，调用`context.ReplacePrincipal`并设置`context.ShouldRenew`属性`true`。

> [!WARNING]
> 此处介绍的方法上的每个请求触发。 这可能导致应用程序较大的性能损失。

## <a name="persistent-cookies"></a>永久 cookie

你可能想要在浏览器会话之间保持的 cookie。 仅应使用显式用户有一个"记住我"复选框上登录名或类似机制同意的情况下启用此持久性。 

下面的代码段将创建的标识和能够通过浏览器闭包后保留下来的相应 cookie。 遵循任何以前配置的滑动过期设置。 如果 cookie 过期浏览器处于关闭状态时，浏览器将它重新启动后清除 cookie。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)类驻留在`Microsoft.AspNetCore.Authentication`命名空间。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)类驻留在`Microsoft.AspNetCore.Http.Authentication`命名空间。

---

## <a name="absolute-cookie-expiration"></a>绝对 cookie 到期时间

你可以设置与绝对过期时间`ExpiresUtc`。 你还必须设置`IsPersistent`; 否则为`ExpiresUtc`将被忽略，并创建一个单会话 cookie。 当`ExpiresUtc`上设置`SignInAsync`，它将重写的值`ExpireTimeSpan`选项`CookieAuthenticationOptions`，如果设置。

下面的代码段将创建的标识和持续时间为 20 分钟的相应 cookie。 这将忽略任何以前配置的滑动过期设置。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a>请参阅

* [身份验证 2.0 更改 / 迁移公告](https://github.com/aspnet/Announcements/issues/262)
* [使用方案限制标识](xref:security/authorization/limitingidentitybyscheme)
* [基于声明的授权](xref:security/authorization/claims)
* [基于策略的角色检查](xref:security/authorization/roles#policy-based-role-checks)
