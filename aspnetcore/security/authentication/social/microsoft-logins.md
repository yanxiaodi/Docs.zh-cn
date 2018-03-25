---
title: "Microsoft 帐户外部登录设置"
author: rick-anderson
description: "本教程演示的集成到现有的 ASP.NET Core 应用程序的 Microsoft 帐户用户身份验证。"
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: d57647da978f7edaaddedba7c9f4c1de8dc07405
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="configuring-microsoft-account-authentication"></a>配置 Microsoft 帐户身份验证

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教程演示如何使用户可以使用示例 ASP.NET 核心 2.0 项目上创建其 Microsoft 帐户登录[上一页](index.md)。

## <a name="create-the-app-in-microsoft-developer-portal"></a>在 Microsoft 开发人员门户中创建应用程序

* 导航到[https://apps.dev.microsoft.com](https://apps.dev.microsoft.com)和创建或登录到 Microsoft 帐户：

![登录对话框](index/_static/MicrosoftDevLogin.png)

如果你尚没有 Microsoft 帐户，请点击**[创建一个 ！](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** 在登录后你将重定向到**我的应用程序**页：

![Microsoft 开发人员门户中，打开 Microsoft Edge 中](index/_static/MicrosoftDev.png)

* 点击**添加应用程序**在右上角，然后输入你**应用程序名称**和**联系人电子邮件**:

![新的应用程序注册对话框](index/_static/MicrosoftDevAppCreate.png)

* 对于此教程的目的，清除**指导安装程序**复选框。

* 点击**创建**继续**注册**页。 提供**名称**并记下的值**应用程序 Id**，其中使用即`ClientId`教程后面：

![注册页](index/_static/MicrosoftDevAppReg.png)

* 点击**添加平台**中**平台**部分，并选择**Web**平台：

![添加平台对话框](index/_static/MicrosoftDevAppPlatform.png)

* 在新**Web**平台部分中，输入与你开发的 URL */signin-microsoft*追加到**重定向 Url**字段 (例如： `https://localhost:44320/signin-microsoft`)。 本教程中稍后配置的 Microsoft 身份验证方案将自动处理请求在*/signin-microsoft*要实现的 OAuth 流路由：

![Web 平台部分](index/_static/MicrosoftRedirectUri.png)

* 点击**添加 URL**以确保该 URL 已添加。

* 填写应用程序的任何其他设置，如有必要，并点击**保存**底部的页后，可以将更改保存到应用配置。

* 部署站点时你将需要重新访问**注册**页，并设置新的公共 URL。

## <a name="store-microsoft-application-id-and-password"></a>存储 Microsoft 应用程序 Id 和密码

* 请注意`Application Id`上显示**注册**页。

* 点击**生成新密码**中**应用程序机密**部分。 此时将显示一个框，其中你可以将复制的应用程序密码：

![新建生成的密码对话框](index/_static/MicrosoftDevPassword.png)

链接敏感设置，例如 Microsoft`Application ID`和`Password`到你应用程序配置中使用[机密 Manager](../../app-secrets.md)。 对于此教程的目的，命名为令牌`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`。

## <a name="configure-microsoft-account-authentication"></a>配置 Microsoft 帐户身份验证

在本教程使用的项目模板可确保[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)已安装包。

* 若要使用 Visual Studio 2017 安装此包，请右键单击项目并选择**管理 NuGet 包**。
* 若要使用.NET 核心 CLI 安装，请在项目目录中执行以下命令：

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

添加 Microsoft 帐户服务`ConfigureServices`中的方法*Startup.cs*文件：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

添加在 Microsoft 帐户中间件`Configure`中的方法*Startup.cs*文件：

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

尽管在 Microsoft 开发人员门户上使用的术语名称这些令牌`ApplicationId`和`Password`，它们作为公开`ClientId`和`ClientSecret`到配置 API。

请参阅[MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions)支持 Microsoft 帐户身份验证的配置选项的详细信息的 API 参考。 这可以用于请求有关用户的不同信息。

## <a name="sign-in-with-microsoft-account"></a>使用 Microsoft 帐户登录

运行你的应用程序，然后单击**登录**。 将显示一个选项以使用 Microsoft 登录：

![Web 应用程序日志中页： 未经过身份验证的用户](index/_static/DoneMicrosoft.png)

当你单击 on Microsoft 时，你将重定向到 Microsoft 进行身份验证。 （如果尚未登录），使用你的 Microsoft 帐户登录后你将会提示您允许此应用访问你的信息：

![Microsoft 身份验证对话框](index/_static/MicrosoftLogin.png)

点击**是**，将重定向回 web 站点，你可以设置你的电子邮件。

现在已在使用您的 Microsoft 凭据进行登录：

![Web 应用程序： 身份验证的用户](index/_static/Done.png)

## <a name="troubleshooting"></a>疑难解答

* 如果 Microsoft 帐户提供程序将你重定向到登录错误页，请注意错误标题和说明查询字符串参数紧接`#`（哈希标记） 的 Uri 中。

  尽管错误消息看起来，以指示 Microsoft 身份验证问题，但最常见的原因是你的应用程序与任何不匹配的 Uri**重定向 Uri**指定的**Web**平台.
* **ASP.NET 核心 2.x 仅：**如果标识未通过调用配置`services.AddIdentity`中`ConfigureServices`，尝试进行身份验证将导致*ArgumentException： 必须提供 SignInScheme 选项*。 在本教程使用的项目模板可确保，这完成的。
* 如果尚未通过应用初始迁移创建站点数据库，则会出现*处理请求时，数据库操作失败*错误。 点击**应用迁移**创建数据库和刷新可跳过错误。

## <a name="next-steps"></a>后续步骤

* 本文介绍了你与 Microsoft 可以进行的验证。 你可以遵循类似的方法进行身份验证使用其他提供商上列出[上一页](index.md)。

* 一旦您的网站发布到 Azure web 应用时，你应该创建一个新`Password`Microsoft 开发人员门户中。

* 设置`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`作为在 Azure 门户中的应用程序设置。 配置系统设置以从环境变量中读取项。
