---
title: "具有特定的方案-授权 ASP.NET 核心"
author: rick-anderson
description: "此文章介绍了如何使用多个身份验证方法时限制为特定方案的标识。"
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: dd044a0829382f9f7f0c3256c6e669340f2d5240
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="authorize-with-a-specific-scheme"></a>具有特定方案授权

在某些情况下，如单页面应用程序 (Spa) 很常见的是使用多个身份验证方法。 例如，应用可能会使用基于 cookie 的身份验证登录和 JWT 持有者身份验证的 JavaScript 请求。 在某些情况下，应用程序可能具有身份验证处理程序的多个的实例。 例如，其中一个包含基本标识的两个 cookie 处理程序，另一个时创建已触发多因素身份验证 (MFA)。 用户请求要求额外的安全的操作，因此，可能会触发 MFA。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

当在身份验证过程中配置身份验证服务，名为身份验证方案。 例如:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

已在前面的代码中，添加两个身份验证处理程序： 一个用于 cookie，一个用于持有者。

>[!NOTE]
>指定的默认方案会导致`HttpContext.User`属性设置为该标识。 如果不需要该行为，禁用调用的无参数形式`AddAuthentication`。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

身份验证方案进行命名时在身份验证过程配置身份验证中间件。 例如:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

已在前面的代码中，添加两个身份验证中间件： 一个用于 cookie，一个用于持有者。

>[!NOTE]
>指定的默认方案会导致`HttpContext.User`属性设置为该标识。 如果不需要该行为，禁用设置`AuthenticationOptions.AutomaticAuthenticate`属性`false`。

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>选择带有 Authorize 属性的方案

在授权，终端应用指示要使用的处理程序。 选择与应用程序将授权通过传递到身份验证方案的以逗号分隔列表的处理程序`[Authorize]`。 `[Authorize]`属性指定的身份验证方案或方案，可使用无论配置默认值。 例如:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

在前面的示例中，cookie 和持有者处理程序运行，并有机会在创建并追加当前用户的标识。 通过指定一种方案，相应的处理程序运行。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

在前面的代码中，仅具有"Bearer"方案处理程序运行。 任何基于 cookie 的标识将被忽略。

## <a name="selecting-the-scheme-with-policies"></a>选择使用策略的方案

如果想要指定在所需的架构[策略](xref:security/authorization/policies)，你可以设置`AuthenticationSchemes`集合添加你的策略时：

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

在前面的示例中，"Over18"策略仅运行针对"Bearer"处理程序创建的标识。 通过设置来使用策略`[Authorize]`特性的`Policy`属性：

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
