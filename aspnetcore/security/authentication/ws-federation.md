---
title: "使用 WS 联合身份验证在 ASP.NET 核心中的用户进行身份验证"
author: chlowell
description: "本教程演示如何在 ASP.NET Core 应用程序使用 WS 联合身份验证。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>使用 WS 联合身份验证在 ASP.NET 核心中的用户进行身份验证

本教程演示如何使用户能够使用 WS 联合身份验证提供程序 (如 Active Directory 联合身份验证服务 (ADFS) 登录或[Azure Active Directory](/azure/active-directory/) (AAD)。 它使用 ASP.NET 核心 2.0 示例应用程序中所述[Facebook、 Google、 和外部提供程序身份验证](xref:security/authentication/social/index)。

对于 ASP.NET 核心 2.0 应用程序中，WS 联合身份验证的支持由提供[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)。 此组件移植从[Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)和共享许多该组件的机制。 但是，组件的几个重要方面不同。

默认情况下，新的中间件：

* 不允许未经请求的登录名。 这一功能的 WS 联合身份验证协议容易受到 XSRF 攻击。 但是，可以使用启用此`AllowUnsolicitedLogins`选项。
* 不会检查消息登录的每个窗体发布请求。 只请求在到`CallbackPath`登录程序。 检查`CallbackPath`默认为`/signin-wsfed`但可以更改。 此路径可以与其他身份验证提供程序共享通过启用`SkipUnrecognizedRequests`选项。

## <a name="register-the-app-with-active-directory"></a>向 Active Directory 注册应用程序

### <a name="active-directory-federation-services"></a>Active Directory 联合身份验证服务

* 打开服务器的**添加信赖方信任向导**从 ADFS 管理控制台：

![添加信赖方信任向导： 欢迎](ws-federation/_static/AdfsAddTrust.png)

* 选择了手动输入数据：

![添加信赖方信任向导： 选择数据源](ws-federation/_static/AdfsSelectDataSource.png)

* 输入信赖方的显示名称。 名称并不重要到 ASP.NET 核心应用程序。

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)缺少用于令牌加密，因此未配置令牌加密证书的支持：

![添加信赖方信任向导： 配置证书](ws-federation/_static/AdfsConfigureCert.png)

* 启用支持 WS 联合身份验证被动协议，使用应用的 URL。 验证端口正确应用：

![添加信赖方信任向导： 配置 URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> 这必须是 HTTPS URL。 在开发期间托管应用程序时，IIS Express 可以提供自签名的证书。 Kestrel 需要手动证书配置。 请参阅[Kestrel 文档](xref:fundamentals/servers/kestrel)有关详细信息。

* 单击**下一步**完成该向导的其余部分和**关闭**末尾。

* ASP.NET 核心标识需要**名称 ID**声明。 添加一个从**编辑声明规则**对话框：

![编辑声明规则](ws-federation/_static/EditClaimRules.png)

* 在**添加转换声明规则向导**，保留默认**以声明方式发送 LDAP 属性**选择的模板，然后单击**下一步**。 添加规则映射**SAM 帐户名**LDAP 属性到**名称 ID**传出声明：

![添加转换声明规则向导： 配置声明规则](ws-federation/_static/AddTransformClaimRule.png)

* 单击**完成** > **确定**中**编辑声明规则**窗口。

### <a name="azure-active-directory"></a>Azure Active Directory

* 导航到 AAD 租户的应用注册边栏选项卡。 单击**新应用程序注册**:

![Azure Active Directory： 应用程序注册](ws-federation/_static/AadNewAppRegistration.png)

* 输入应用程序注册的名称。 这并不重要到 ASP.NET 核心应用程序。
* 输入作为侦听的应用程序的 URL**登录 URL**:

![Azure Active Directory： 创建应用程序注册](ws-federation/_static/AadCreateAppRegistration.png)

* 单击**终结点**并记下**联合元数据文档**URL。 这是 WS 联合身份验证中间件的`MetadataAddress`:

![Azure Active Directory： 终结点](ws-federation/_static/AadFederationMetadataDocument.png)

* 导航到新的应用程序注册。 单击**设置** > **属性**并记下**应用程序 ID URI**。 这是 WS 联合身份验证中间件的`Wtrealm`:

![Azure Active Directory： 应用程序注册属性](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>为 ASP.NET 核心标识的外部登录提供程序中添加 WS 联合身份验证

* 添加的依赖项[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)到项目。
* 添加 WS 联合身份验证到`Configure`中的方法*Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>使用 WS 联合身份验证登录

浏览到应用程序并单击**登录**nav 标头中的链接。 没有用于登录 WsFederation 选项：![登录页](ws-federation/_static/WsFederationButton.png)

使用作为提供程序的 ADFS，按钮将重定向到 ADFS 登录页： ![ADFS 登录页](ws-federation/_static/AdfsLoginPage.png)

与 Azure Active Directory 作为提供程序，该按钮将重定向到 AAD 登录页： ![AAD 登录页](ws-federation/_static/AadSignIn.png)

成功登录的新用户将重定向到应用程序的用户注册页：![注册页](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>使用 WS 联合身份验证而无需 ASP.NET 核心标识

没有标识，可以使用 WS 联合身份验证中间件。 例如:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
