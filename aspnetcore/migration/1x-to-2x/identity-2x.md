---
title: "迁移的身份验证和标识到 ASP.NET 核心 2.0"
author: scottaddie
description: "本文概述了迁移 ASP.NET Core 1.x 身份验证和标识为 ASP.NET 核心 2.0 的最常见步骤。"
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: f0c29e6b4faa5c9d574726fc960f0c7c60092757
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a>迁移的身份验证和标识到 ASP.NET 核心 2.0

通过[Scott Addie](https://github.com/scottaddie)和[Hao 永远](https://github.com/HaoK)

ASP.NET 核心 2.0 具有用于身份验证的新模型和[标识](xref:security/authentication/identity)这简化了使用服务的配置。 ASP.NET 核心 1.x 应用程序使用身份验证或标识可以更新以使用新的模型，如下所述。

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>身份验证中间件和服务
在 1.x 项目中，通过中间件配置身份验证。 中间件方法调用每个你想要支持的身份验证方案。

下面的 1.x 示例将 Facebook 身份验证配置中标识*Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions { 
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
} 
```

在 2.0 项目中，通过服务配置身份验证。 在中注册每个身份验证方案`ConfigureServices`方法*Startup.cs*。 `UseIdentity`方法将替换`UseAuthentication`。

下面的 2.0 示例将 Facebook 身份验证配置中标识*Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

`UseAuthentication`方法将添加一个单一的身份验证中间件组件，它负责自动身份验证和远程身份验证请求的处理。 它将替换所有单独的中间件组件与单个，常见的中间件组件。

以下是每个主要身份验证方案的 2.0 迁移说明。

### <a name="cookie-based-authentication"></a>基于 cookie 的身份验证
选择以下两个选项之一，然后进行必要的更改在*Startup.cs*:

1. 标识与使用 cookie
    - 替换`UseIdentity`与`UseAuthentication`中`Configure`方法：

        ```csharp
        app.UseAuthentication();
        ```

    - 调用`AddIdentity`中的方法`ConfigureServices`方法将添加 cookie 身份验证服务。
    - （可选） 调用`ConfigureApplicationCookie`或`ConfigureExternalCookie`中的方法`ConfigureServices`方法来调整标识 cookie 设置。

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. 使用 cookie，而无需标识
    - 替换`UseCookieAuthentication`方法调用`Configure`方法替换`UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - 调用`AddAuthentication`和`AddCookie`中的方法`ConfigureServices`方法：

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => 
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a>JWT 持有者身份验证
进行中的以下更改*Startup.cs*:
- 替换`UseJwtBearerAuthentication`方法调用`Configure`方法替换`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 调用`AddJwtBearer`中的方法`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    此代码段不使用标识，因此应通过将传递设置的默认方案`JwtBearerDefaults.AuthenticationScheme`到`AddAuthentication`方法。

### <a name="openid-connect-oidc-authentication"></a>OpenID 连接 (OIDC) 身份验证
进行中的以下更改*Startup.cs*:

- 替换`UseOpenIdConnectAuthentication`方法调用`Configure`方法替换`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 调用`AddOpenIdConnect`中的方法`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication(options => 
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => 
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a>facebook 身份验证
进行中的以下更改*Startup.cs*:
- 替换`UseFacebookAuthentication`方法调用`Configure`方法替换`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 调用`AddFacebook`中的方法`ConfigureServices`方法：
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Google 身份验证
进行中的以下更改*Startup.cs*:
- 替换`UseGoogleAuthentication`方法调用`Configure`方法替换`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 调用`AddGoogle`中的方法`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Microsoft 帐户身份验证
进行中的以下更改*Startup.cs*:
- 替换`UseMicrosoftAccountAuthentication`方法调用`Configure`方法替换`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 调用`AddMicrosoftAccount`中的方法`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Twitter 身份验证
进行中的以下更改*Startup.cs*:
- 替换`UseTwitterAuthentication`方法调用`Configure`方法替换`UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- 调用`AddTwitter`中的方法`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>设置默认身份验证方案
在 1.x`AutomaticAuthenticate`和`AutomaticChallenge`属性[AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基类用于设置在单一身份验证方案。 没有良好方法来强制执行此。

在 2.0 中，这两个属性已删除作为属性对各个`AuthenticationOptions`实例。 它们可以在中配置`AddAuthentication`内的方法调用`ConfigureServices`方法*Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

在前面的代码段中，默认方案设置为`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie")。

或者，使用一个重载的版本的`AddAuthentication`方法以设置多个属性。 在下面的重载的方法示例中，默认方案设置为`CookieAuthenticationDefaults.AuthenticationScheme`。 或者可能在个人中指定的身份验证方案`[Authorize]`属性或授权策略。

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

如果下列条件之一为 true，请在 2.0 中定义了默认的方案：
- 你希望用户可自动登录
- 你使用`[Authorize]`而无需指定方案的属性或授权策略

此规则的唯一例外是`AddIdentity`方法。 此方法将为您和设置默认值进行身份验证以及与应用程序 cookie 质询方案添加 cookie `IdentityConstants.ApplicationScheme`。 此外，它将默认登录方案设置为外部 cookie `IdentityConstants.ExternalScheme`。

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>使用 HttpContext 身份验证扩展插件
`IAuthenticationManager`接口是 1.x 身份验证系统的主入口点。 它已替换为一组新的`HttpContext`中的扩展方法`Microsoft.AspNetCore.Authentication`命名空间。

例如，1.x 项目引用`Authentication`属性：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

在 2.0 项目中，导入`Microsoft.AspNetCore.Authentication`命名空间，并删除`Authentication`属性引用：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows 身份验证 (HTTP.sys / IISIntegration)
有两种变体 Windows 身份验证：
1. 主机仅允许经过身份验证的用户
2. 主机允许同时匿名和身份验证的用户

上面所述的第一个变体不受 2.0 的更改。

2.0 更改的情况下，会影响上面所述的第二个变体。 例如，你可能将以允许匿名用户到你的应用程序在 IIS 或[HTTP.sys](xref:fundamentals/servers/weblistener)层在控制器级别但授权用户。 在此方案中，设置默认方案为`IISDefaults.AuthenticationScheme`中`ConfigureServices`方法*Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

相应地，如果设置的默认方案会使质询无法正常工作的授权请求。

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions 实例
2.0 更改的副作用是时切换到使用名为而不是 cookie 选项实例的选项。 删除自定义标识 cookie 方案名称的能力。

例如，1.x 项目使用[构造函数注入](xref:mvc/controllers/dependency-injection#constructor-injection)传递`IdentityCookieOptions`参数转换*AccountController.cs*。 从提供的实例访问外部 cookie 身份验证方案：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

前面提到的构造函数注入将成为在 2.0 项目中，不必要和`_externalCookieScheme`可以删除字段：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme`可以直接使用常量：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>添加 POCO IdentityUser 导航属性
基的 Entity Framework (EF) 核心导航属性`IdentityUser`POCO （普通旧 CLR 对象） 已被删除。 如果你 1.x 的项目使用这些属性，手动将它们添加回 2.0 项目：

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

若要防止重复的外键，运行 EF 核心迁移时，将以下代码添加到你`IdentityDbContext`类的`OnModelCreating`方法 (后`base.OnModelCreating();`调用):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a>替换 GetExternalAuthenticationSchemes
同步方法`GetExternalAuthenticationSchemes`已删除为支持的异步版本。 1.x 项目有以下代码*ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

此方法将出现在*Login.cshtml*太：

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

在 2.0 项目中，使用`GetExternalAuthenticationSchemesAsync`方法：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

在*Login.cshtml*、`AuthenticationScheme`中访问属性`foreach`循环更改为`Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel 属性更改
A`ManageLoginsViewModel`对象将用于`ManageLogins`操作*ManageController.cs*。 1.x 项目，该对象中`OtherLogins`属性的返回类型是`IList<AuthenticationDescription>`。 此返回类型需要导入`Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

在 2.0 项目中，返回类型更改为`IList<AuthenticationScheme>`。 此新的返回类型需要替换`Microsoft.AspNetCore.Http.Authentication`使用导入`Microsoft.AspNetCore.Authentication`导入。

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>其他资源
有关其他详细信息和讨论，请参阅[针对身份验证 2.0 讨论](https://github.com/aspnet/Security/issues/1338)GitHub 上的问题。
