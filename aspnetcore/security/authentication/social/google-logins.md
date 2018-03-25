---
title: "在 ASP.NET Core Google 外部登录安装程序"
author: rick-anderson
description: "本教程演示的集成到现有的 ASP.NET Core 应用程序的 Google 帐户用户身份验证。"
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: 1ca63593a7cf2b0eff1e52c0beda7ef2b826d474
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="configuring-google-authentication-in-aspnet-core"></a>在 ASP.NET 核心中配置 Google 身份验证

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教程演示如何使用户可以使用示例 ASP.NET 核心 2.0 项目上创建其 Google + 帐户登录[上一页](index.md)。 我们先执行[官方步骤](https://developers.google.com/identity/sign-in/web/devconsole-project)在 Google API 控制台中创建新的应用程序。

## <a name="create-the-app-in-google-api-console"></a>在 Google API 控制台中创建应用程序

* 导航到[https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library)并登录。 如果你还没有 Google 帐户，使用**更多选项** > **[创建帐户](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**链接创建一个：

![Google API 控制台](index/_static/GoogleConsoleLogin.png)

* 重定向到**API Manager 库**页：

![API 管理器库页](index/_static/GoogleConsoleSwitchboard.png)

* 点击**创建**并输入你**项目名称**:

![“新建项目”对话框](index/_static/GoogleConsoleNewProj.png)

* 接受对话框中之后, 你将重定向回库页允许你选择为新应用程序的功能。 查找**Google + API**在列表中，单击其链接来添加的 API 功能：

![API 管理器库页](index/_static/GoogleConsoleChooseApi.png)

* 显示新添加的 API 的页。 点击**启用**将 Google + 登录功能中添加到你的应用程序：

![API 管理器 Google + API 页](index/_static/GoogleConsoleEnableApi.png)

* 启用 API 后, 点击**创建凭据**配置机密：

![API 管理器 Google + API 页](index/_static/GoogleConsoleGoCredentials.png)

* 选择：
   * **Google+ API**
   * **Web 服务器 (例如 node.js，Tomcat)**，和
   * **用户数据**:

![API 管理器凭据页： 找到什么类型的凭据需要面板](index/_static/GoogleConsoleChooseCred.png)

* 点击**我需要什么凭据？**随即将跳转到应用配置的第二个步骤**创建 OAuth 2.0 客户端 ID**:

![API 管理器凭据页： 创建 OAuth 2.0 客户端 ID](index/_static/GoogleConsoleCreateClient.png)

* 因为我们将使用只对一个功能 （登录），我们可以输入相同创建 Google + 项目**名称**与我们的项目使用的 OAuth 2.0 客户端 id。

* 输入你的开发 URI 与*/signin-google*追加到**已授权重定向 Uri**字段 (例如： `https://localhost:44320/signin-google`)。 本教程中稍后配置 Google 身份验证将自动处理请求在*/signin-google*要实现的 OAuth 流路由。

* 按 tab 键以添加**已授权重定向 Uri**条目。

* 点击**创建客户端 ID**，随即将跳转到第三个步骤中，**设置 OAuth 2.0 许可屏幕**:

![API 管理器凭据页： 设置 OAuth 2.0 许可屏幕](index/_static/GoogleConsoleAddCred.png)

* 输入你的面向公众**电子邮件地址**和**产品名称**Google + 提示用户登录时显示为你的应用。 其他选项下有**更多的自定义选项**。

* 点击**继续**继续到最后一个步骤中，**下载凭据**:

![API 管理器凭据页： 下载凭据](index/_static/GoogleConsoleFinish.png)

* 点击**下载**若要使用应用程序机密，保存 JSON 文件和**完成**以完成创建新的应用程序。

* 部署站点时你将需要重新访问**Google 控制台**和注册一个新的公共 url。

## <a name="store-google-clientid-and-clientsecret"></a>应用商店 Google ClientID 和 ClientSecret

链接敏感设置，例如 Google`Client ID`和`Client Secret`到你应用程序配置中使用[机密 Manager](../../app-secrets.md)。 对于此教程的目的，命名为令牌`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`。

这些标记的值可以在下一步中下载的 JSON 文件中找到`web.client_id`和`web.client_secret`。

## <a name="configure-google-authentication"></a>配置 Google 身份验证

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

添加中的 Google 服务`ConfigureServices`中的方法*Startup.cs*文件：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在本教程使用的项目模板可确保[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)安装包。

 * 若要使用 Visual Studio 2017 安装此包，请右键单击项目并选择**管理 NuGet 包**。
 * 若要使用.NET 核心 CLI 安装，请在项目目录中执行以下命令：

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

添加中的 Google 中间件`Configure`中的方法*Startup.cs*文件：

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

请参阅[GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) Google 身份验证支持的配置选项的详细信息的 API 参考。 这可以用于请求有关用户的不同信息。

## <a name="sign-in-with-google"></a>使用 Google 登录

运行你的应用程序，然后单击**登录**。 将显示一个选项以使用 Google 登录：

![Microsoft Edge 中运行 web 应用程序： 用户未经过身份验证](index/_static/DoneGoogle.png)

当你单击 Google 时，你将重定向到 Google 身份验证：

![Google 身份验证对话框](index/_static/GoogleLogin.png)

在输入你的 Google 凭据，然后你将重定向回 web 站点，你可以设置你的电子邮件。

现在已在使用你的 Google 凭据进行登录：

![Microsoft Edge 中运行 web 应用程序： 身份验证的用户](index/_static/Done.png)

## <a name="troubleshooting"></a>疑难解答

* 如果你收到`403 (Forbidden)`从你自己的应用，请确保在开发模式 （或中断到调试器中相同的错误），运行时的错误页**Google + API**已在中启用**API Manager 库**按照列出的步骤[更早版本在此页](#create-the-app-in-google-api-console)。 如果在登录不起作用，并且不能接收任何错误，切换到开发模式以使问题更易于调试。
* **ASP.NET 核心 2.x 仅：**如果标识未通过调用配置`services.AddIdentity`中`ConfigureServices`，尝试进行身份验证将导致*ArgumentException： 必须提供 SignInScheme 选项*。 在本教程使用的项目模板可确保，这完成的。
* 如果尚未通过应用初始迁移创建站点数据库，则会出现*处理请求时，数据库操作失败*错误。 点击**应用迁移**创建数据库和刷新可跳过错误。

## <a name="next-steps"></a>后续步骤

* 本文介绍了你使用 Google 可以进行的验证。 你可以遵循类似的方法进行身份验证使用其他提供商上列出[上一页](index.md)。

* 一旦您的网站发布到 Azure web 应用时，您应重置`ClientSecret`在 Google API 控制台中。

* 设置`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`作为在 Azure 门户中的应用程序设置。 配置系统设置以从环境变量中读取项。
