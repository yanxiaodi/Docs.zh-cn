---
title: "配置 ASP.NET 核心标识"
author: AdrienTorris
description: "了解 ASP.NET 核心标识默认值，并了解如何配置要使用自定义值的标识属性。"
manager: wpickett
ms.author: scaddie
ms.date: 03/06/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: f8be8a555454a99a3e75b5cd3d42c11e1d7b2b7e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="configure-identity"></a>配置标识

ASP.NET 核心标识使用默认配置设置，例如密码策略、 锁定时间和 cookie 设置。 应用程序的可重写这些设置`Startup`类。

## <a name="identity-options"></a>标识选项

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)类表示可以用于配置的标识系统的选项。

### <a name="claims-identity"></a>声明的标识

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)指定[ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)与表中显示的属性。

| 属性 | 描述 | 默认 |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | 获取或设置用于角色声明的声明类型。 | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | 获取或设置用于安全戳声明的声明类型。 | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | 获取或设置用于用户标识符声明的声明类型。 | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | 获取或设置用于用户名称声明的声明类型。 | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>锁定

为一段时间内阻止用户，在给定的失败的访问尝试次数后 (默认： 5 分钟锁定 5 失败的访问尝试次数后)。 成功的身份验证将失败的访问尝试计数重置并重置时钟。

下面的示例显示默认值：

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

确认[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)设置`lockoutOnFailure`到`true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)指定[LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)与表中显示的属性。

| 属性 | 描述 | 默认 |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | 确定是否新用户会被锁定。 | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | 时间量锁定用户锁定发生时。 | 5 分钟 |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | 失败的访问尝试，直到用户已被锁定，如果启用了锁定次数。 | 5 |

### <a name="password"></a>Password

默认情况下，标识要求密码包含大写字符、 小写字符、 数字和非字母数字字符。 密码必须至少为六个字符。 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)可以更改在`Startup.ConfigureServices`。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET 核心 2.0 增加[RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars)属性。 否则，选项是 ASP.NET Core 相同 1.x。

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)指定[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)与表中显示的属性。

| 属性 | 描述 | 默认 |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | 需要介于 0-9 中的密码。 | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | 密码最小长度。 | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | 仅适用于 ASP.NET 核心 2.0 或更高版本。<br><br> 需要密码中的非重复字符数。 | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | 需要密码中的小写字符。 | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | 需要密码中的非字母数字字符。 | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | 需要密码中的大写字符。 | `true` |

### <a name="sign-in"></a>登录

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)指定[SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions)与表中显示的属性。

| 属性 | 描述 | 默认 |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | 需要登录的确认电子邮件。 | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | 需要一个确认的电话号码来登录。 | `false` |

### <a name="tokens"></a>标记

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)指定[TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions)与表中显示的属性。

| 属性 | 描述 |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | 获取或设置`AuthenticatorTokenProvider`用于验证双因素登录使用验证器。 |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | 获取或设置`ChangeEmailTokenProvider`用于生成电子邮件更改确认电子邮件中使用的令牌。 |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | 获取或设置`ChangePhoneNumberTokenProvider`用于生成令牌更改电话号码时使用。 |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | 获取或设置用于生成在帐户确认电子邮件中使用的令牌的令牌提供程序。 |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | 获取或设置[IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)用于生成在密码重置电子邮件中使用的令牌。 |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | 用于构造[用户令牌提供程序](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)的密钥与用作为提供程序的名称。 |

### <a name="user"></a>“用户”

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)指定[UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions)与表中显示的属性。

| 属性 | 描述 | 默认 |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | 允许使用的用户名中的字符。 | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | 要求每个用户具有唯一的电子邮件。 | `false` |

## <a name="cookie-settings"></a>Cookie 设置

配置中的应用程序的 cookie `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)具有以下属性：

| 属性 | 描述 |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | 通知处理程序，它应更改传出*403 禁止访问*状态代码转换为*302 重定向*到给定的路径。<br><br>默认值为 `/Account/AccessDenied`。 |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | 仅适用于 ASP.NET Core 1.x。<br><br> 特定的身份验证方案逻辑名称。 |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | 仅适用于 ASP.NET Core 1.x。<br><br> 为 true 时，cookie 身份验证应在每个请求上运行并尝试验证并重新构造它创建的任何序列化的主体。 |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | 仅适用于 ASP.NET Core 1.x。<br><br> 如果为 true，则身份验证中间件处理自动挑战。 如果为 false，身份验证中间件仅更改响应时显式由`AuthenticationScheme`。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | 获取或设置应将用于创建的任何声明的颁发者 (继承自[AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions))。 |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | 要将与 cookie 相关联的域。 |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | 获取或设置 HTTP cookie (不身份验证 cookie) 的使用期限。 通过重写此属性[ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan)。 它不应使用在 CookieAuthentication 的上下文。 |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | 指示是否可访问客户端脚本的 cookie。<br><br>默认值为 `true`。 |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Cookie 的名称。<br><br>默认值为 `.AspNetCore.Cookies`。 |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Cookie 路径中。 |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | `SameSite`的 cookie 的属性。<br><br>默认值是[SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode)。 |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy)配置。<br><br>默认值是[CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy)。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | 仅适用于 ASP.NET Core 1.x。<br><br> Cookie 提供服务位置的域名。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | 仅适用于 ASP.NET Core 1.x。<br><br> 一个标志，指示该 cookie 应仅服务器访问。<br><br>默认值为 `true`。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | 仅适用于 ASP.NET Core 1.x。<br><br> 用于隔离在相同的主机名上运行的应用程序。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | 仅适用于 ASP.NET Core 1.x。<br><br> 一个标志，指示是否创建的 cookie 应被限制为 HTTPS (`CookieSecurePolicy.Always`)，HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或请求的同一个协议 (`CookieSecurePolicy.SameAsRequest`)。<br><br>默认值为 `CookieSecurePolicy.SameAsRequest`。 |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | 用于从请求中获取 cookie 或将它们设置在响应上的组件。 | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | 如果设置，使用的提供程序通过[CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler)的数据保护。 |
| [说明](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | 仅适用于 ASP.NET Core 1.x。<br><br> 有关提供给应用程序的身份验证类型的其他信息。 |
| [事件](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | 该处理程序的提供程序以便为应用程序控件提供在处理发生的位置中的某些点上调用方法。 |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | 如果设置，该服务获取类型`Events`实例而不是属性 (继承自[AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions))。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | 控件用多少时间有效从创建它时保留了 cookie 中存储身份验证票证。<br><br>默认值为 14 天。 |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | 未授权用户时，则在重定向到登录到此路径。<br><br>默认值为 `/Account/Login`。 |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | 当用户已注销时，则在重定向到此路径。<br><br>默认值为 `/Account/Logout`。 |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | 确定附加的中间件的查询字符串参数的名称时*401 未授权*状态代码更改为*302 重定向*到登录名路径。<br><br>默认值为 `ReturnUrl`。 |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | 要在其中存储跨请求的标识可选容器。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | 为 true 时，使用当前 cookie 的多个中途过期窗口新过期时间被颁发一个新的 cookie。<br><br>默认值为 `true`。 |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | `TicketDataFormat`用于保护和取消保护标识和其他属性存储在 cookie 值。 |
