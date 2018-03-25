---
title: "在 ASP.NET Core Facebook 外部登录安装程序"
author: rick-anderson
description: "本教程演示的集成到现有的 ASP.NET Core 应用程序的 Facebook 帐户用户身份验证。"
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: 283de1df68c45fa1b41ed75710fe9eb7925f815f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="configuring-facebook-authentication"></a>配置 Facebook 身份验证

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教程演示如何使用户可以使用示例 ASP.NET 核心 2.0 项目上创建其 Facebook 帐户登录[上一页](index.md)。 我们首先按照创建 Facebook 应用程序 ID[官方步骤](https://developers.facebook.com)。

## <a name="create-the-app-in-facebook"></a>在 Facebook 中创建应用程序

*  导航到[Facebook 开发人员应用](https://developers.facebook.com/apps/)页上，并登录。 如果你还没有的 Facebook 帐户，使用**注册 Facebook**创建一个的登录页上的链接。

* 点击**添加新的应用程序**创建一个新的应用程序 id。 右上角的按钮

   ![Microsoft Edge 中打开 Facebook 开发人员门户](index/_static/FBMyApps.png)

* 填写表单，然后点击**创建应用程序 ID**按钮。

   ![创建新的应用程序 ID 窗体](index/_static/FBNewAppId.png)

* 上**选择一个产品**页上，单击**Set Up**上**Facebook 登录名**卡。

   ![产品安装程序页](index/_static/FBProductSetup.png)
  
* **快速入门**向导将启动与**选择一个平台**作为第一页。 现在跳过该向导，通过单击**设置**左侧菜单中的链接：

   ![跳过快速启动](index/_static/FBSkipQuickStart.png)

* 将向你提供**客户端的 OAuth 设置**页：

![客户端的 OAuth 设置页](index/_static/FBOAuthSetup.png)

* 输入你的开发 URI 与*/signin-facebook*追加到**有效的 OAuth 重定向 Uri**字段 (例如： `https://localhost:44320/signin-facebook`)。 本教程中稍后配置的 Facebook 身份验证将自动处理请求在*/signin-facebook*要实现的 OAuth 流路由。

* 单击**保存更改**。

* 单击**仪表板**左侧导航区域中的链接。 

    在此页上，记下你`App ID`和你`App Secret`。 你将添加到 ASP.NET 核心应用程序下一节中：

   ![Facebook 开发人员仪表板](index/_static/FBDashboard.png)

* 部署站点时需要重新访问**Facebook 登录名**安装页并注册一个新的公共 URI。

## <a name="store-facebook-app-id-and-app-secret"></a>存储 Facebook 应用程序 ID 和应用程序密码

链接敏感设置，例如 Facebook`App ID`和`App Secret`到你应用程序配置中使用[机密 Manager](xref:security/app-secrets)。 对于此教程的目的，命名为令牌`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`。

执行以下命令以安全地存储`App ID`和`App Secret`使用密钥管理器：

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>配置 Facebook 身份验证

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

添加中的 Facebook 服务`ConfigureServices`中的方法*Startup.cs*文件：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

安装[Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook)包。

* 若要使用 Visual Studio 2017 安装此包，请右键单击项目并选择**管理 NuGet 包**。
* 若要使用.NET 核心 CLI 安装，请在项目目录中执行以下命令：

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

添加在 Facebook 中间件`Configure`中的方法*Startup.cs*文件：

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

请参阅[FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) Facebook 身份验证支持的配置选项的详细信息的 API 参考。 配置选项可用于：

* 请求有关用户的不同信息。
* 添加查询字符串参数，以自定义登录体验。

## <a name="sign-in-with-facebook"></a>使用 Facebook 登录

运行你的应用程序，然后单击**登录**。 你看到一个选项以使用 Facebook 登录。

![Web 应用程序： 用户未经过身份验证](index/_static/DoneFacebook.png)

当你单击**Facebook**，你将重定向到 Facebook 进行身份验证：

![Facebook 身份验证页面](index/_static/FBLogin.png)

默认情况下，Facebook 身份验证请求公共配置文件和电子邮件地址：

![Facebook 身份验证页面](index/_static/FBLoginDone.png)

输入你的 Facebook 凭据后你重定向回你的站点，你可以设置你的电子邮件。

现在已在使用你的 Facebook 凭据进行登录：

![Web 应用程序： 身份验证的用户](index/_static/Done.png)

## <a name="troubleshooting"></a>疑难解答

* **ASP.NET 核心 2.x 仅：**如果标识未通过调用配置`services.AddIdentity`中`ConfigureServices`，尝试进行身份验证将导致*ArgumentException： 必须提供 SignInScheme 选项*。 在本教程使用的项目模板可确保，这完成的。
* 如果尚未通过应用初始迁移创建站点数据库，则获取*处理请求时，数据库操作失败*错误。 点击**应用迁移**创建数据库和刷新可跳过错误。

## <a name="next-steps"></a>后续步骤

* 本文介绍了你使用 Facebook 可以进行的验证。 你可以遵循类似的方法进行身份验证使用其他提供商上列出[上一页](index.md)。

* 一旦您的网站发布到 Azure web 应用时，您应重置`AppSecret`Facebook 开发人员门户中。

* 设置`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`作为在 Azure 门户中的应用程序设置。 配置系统设置以从环境变量中读取项。
