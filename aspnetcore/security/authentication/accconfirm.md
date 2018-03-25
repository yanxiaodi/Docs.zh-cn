---
title: "帐户确认和 ASP.NET Core 中的密码恢复"
author: rick-anderson
description: "了解如何生成使用电子邮件确认及密码重置功能的 ASP.NET Core 应用程序。"
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b236b4e5d3a4fa7212453f2aec209d145f5f5e32
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>帐户确认和 ASP.NET Core 中的密码恢复

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Joe Audette](https://twitter.com/joeaudette)

本教程演示了如何生成具有电子邮件确认及密码重置的 ASP.NET Core 应用。 本教程是**不**开头主题。 你应熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [身份验证](xref:security/authentication/index)
* [帐户确认和密码恢复](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

请参阅[此 PDF 文件](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)的 ASP.NET 核心 MVC 1.1 和 2.x 版本。

## <a name="prerequisites"></a>系统必备

[.NET 核心 2.1.4 SDK](https://www.microsoft.com/net/core)或更高版本。

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>使用.NET 核心 CLI 创建新的 ASP.NET Core 项目

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* `--auth Individual` 指定的单个用户帐户项目模板。
* 在 Windows 上，添加`-uld`选项。 它指定应而不是 SQLite 使用 LocalDB。
* 运行`new mvc --help`以获取有关此命令帮助。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果你使用 CLI 或 SQLite，运行以下命令在命令窗口中：

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` 指定的单个用户帐户项目模板。
* 在 Windows 上，添加`-uld`选项。 它指定应而不是 SQLite 使用 LocalDB。
* 运行`new mvc --help`以获取有关此命令帮助。

---

或者，你可以使用 Visual Studio 创建新的 ASP.NET Core 项目：

* 在 Visual Studio 中，创建一个新**Web 应用程序**项目。
* 选择**ASP.NET Core 2.0**。 **.NET 核心**选择在下图中，但你可以选择**.NET Framework**。
* 选择**更改身份验证**并将设置为**单个用户帐户**。
* 保留默认值**存储用户帐户在应用程序**。

![显示"单个用户帐户单选"所选的新建项目对话框](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>测试新的用户注册

运行应用程序中，选择**注册**链接，并注册用户。 按照运行实体框架核心迁移的说明。 此时，电子邮件的唯一验证是使用[[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute)属性。 提交注册之后, 你登录到应用程序。 更高版本在教程中，这样，直到其电子邮件已验证新的用户无法登录时将更新代码。

## <a name="view-the-identity-database"></a>查看标识数据库

请参阅[ASP.NET 核心 MVC 项目中使用 SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql)有关说明如何查看 SQLite 数据库。

Visual studio:

* 从**视图**菜单上，选择**SQL Server 对象资源管理器**(SSOX)。
* 导航到**(localdb) MSSQLLocalDB (SQL Server 13)**。 右键单击**dbo。AspNetUsers** > **查看数据**:

![在 AspNetUsers 表中 SQL Server 对象资源管理器的上下文菜单](accconfirm/_static/ssox.png)

请注意表的`EmailConfirmed`字段是`False`。

你可能想要再次在下一步中使用此电子邮件，当应用程序发送确认电子邮件。 右键单击行并选择**删除**。 删除的电子邮件别名便于您在以下步骤。

---

## <a name="require-https"></a>需要 HTTPS

请参阅[需要 HTTPS](xref:security/enforcing-ssl)。

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>需要电子邮件确认

它是一种最佳做法以确认新的用户注册的电子邮件。 电子邮件确认有助于验证它们不模拟其他人 （即，它们尚未注册与其他人的电子邮件）。 假设有论坛，并且你想阻止"yli@example.com"发件人将注册为"nolivetto@contoso.com。" 而无需电子邮件确认"nolivetto@contoso.com"无法接收来自您的应用程序不需要的电子邮件。 假设用户意外注册为"ylo@example.com"和未注意到"yli"的拼写错误。 它们将无法使用恢复密码，因为此应用程序没有正确的电子邮件。 电子邮件确认从机器人提供有限的保护。 电子邮件确认不免受恶意用户的多个电子邮件帐户与提供保护。

你通常想要使不能在有确认电子邮件之前发布到网站的任何数据的新用户。

更新`ConfigureServices`需要确认电子邮件：

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` 防止已注册的用户登录，直到其电子邮件进行确认。

### <a name="configure-email-provider"></a>配置电子邮件提供商

在本教程中，使用 SendGrid 发送电子邮件。 你需要一个 SendGrid 帐户和密钥用于发送电子邮件。 你可以使用其他电子邮件提供商。 ASP.NET 核心 2.x 包括`System.Net.Mail`，这允许你从你的应用程序发送电子邮件。 我们建议你使用 SendGrid 或另一个电子邮件服务发送电子邮件。 SMTP 很难保护并正确设置。

[选项模式](xref:fundamentals/configuration/options)用于访问的用户帐户和密钥设置。 有关详细信息，请参阅[配置](xref:fundamentals/configuration/index)。

创建一个类以提取安全的电子邮件密钥。 对于此示例，`AuthMessageSenderOptions`中创建类*Services/AuthMessageSenderOptions.cs*文件：

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

设置`SendGridUser`和`SendGridKey`与[机密管理器工具](xref:security/app-secrets)。 例如:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

在 Windows 上，密钥管理器存储中的键/值对*secrets.json*文件中`%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`目录。

内容*secrets.json*文件未加密。 *Secrets.json*文件如下所示 (`SendGridKey`值已删除。)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>配置启动要使用 AuthMessageSenderOptions

添加`AuthMessageSenderOptions`到末尾的服务容器`ConfigureServices`中的方法*Startup.cs*文件：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>配置 AuthMessageSender 类

本教程演示如何添加通过电子邮件通知[SendGrid](https://sendgrid.com/)，但是你可以发送电子邮件使用 SMTP 和其他机制。

安装`SendGrid`NuGet 包：

* 从命令行：

    `dotnet add package SendGrid`

* 从程序包管理器控制台中，输入以下命令：

 `Install-Package SendGrid`

请参阅[免费开始使用 SendGrid](https://sendgrid.com/free/)注册一个免费的 SendGrid 帐户。

#### <a name="configure-sendgrid"></a>配置了 SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要配置 SendGrid，添加类似于以下的代码*Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* 中添加代码*Services/MessageServices.cs*类似于以下配置 SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>启用帐户确认和密码恢复

已为帐户确认和密码恢复代码的模板。 查找`OnPostAsync`中的方法*Pages/Account/Register.cshtml.cs*。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

可防止新注册的用户自动登录通过注释掉以下行：

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

突出显示的已更改行还会显示完整的方法：

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要启用帐户确认后，取消注释以下代码：

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**注意：**代码将阻止新注册的用户自动登录通过注释掉以下行：

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

通过对取消注释中的代码中启用密码恢复`ForgotPassword`操作*controllers/Accountcontroller.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

取消注释中的窗体元素*Views/Account/ForgotPassword.cshtml*。 你可能想要删除`<p> For more information on how to enable reset password ... </p>`元素，它包含此文章的链接。

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>注册、 确认电子邮件，以及重置密码

运行该 web 应用，并测试帐户确认和密码恢复流。

* 运行应用并注册新的用户

 ![Web 应用程序帐户注册视图](accconfirm/_static/loginaccconfirm1.png)

* 检查你的帐户确认链接的电子邮件。 请参阅[调试电子邮件](#debug)如果你不会获得电子邮件。
* 单击链接以确认你的电子邮件。
* 登录你的电子邮件和密码。
* 注销。

### <a name="view-the-manage-page"></a>查看管理页

在浏览器中选择你的用户名：![用户同名的浏览器窗口](accconfirm/_static/un.png)

你可能需要展开导航栏，以查看用户名称。

![导航栏](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

管理页显示与**配置文件**选定选项卡。 **电子邮件**显示一个复选框，表明电子邮件已确认。

![管理页](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

这在教程后面部分提到。
![管理页](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>测试密码重置

* 如果你要登录，选择**注销**。
* 选择**登录**链接并选择**忘记了密码？**链接。
* 输入用于注册帐户的电子邮件。
* 发送一封电子邮件包含要重置密码的链接。 请检查你的电子邮件，单击链接以重置密码。 你的密码已成功重置后，你可以登录你的电子邮件和新密码。

<a name="debug"></a>

### <a name="debug-email"></a>调试电子邮件

如果无法获取电子邮件工作：

* 创建[控制台应用程序发送电子邮件](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)。
* 查看[电子邮件活动](https://sendgrid.com/docs/User_Guide/email_activity.html)页。
* 请检查垃圾邮件文件夹。
* 请尝试其他电子邮件提供程序 （Microsoft、 Yahoo、 Gmail 等） 上的另一个电子邮件别名
* 尝试发送到不同的电子邮件帐户。

**最佳安全方案**是**不**使用生产中测试和开发的机密。 如果将应用发布到 Azure，你可以设置 SendGrid 机密与 Azure Web 应用门户中的应用程序设置。 配置系统设置以从环境变量中读取项。

## <a name="combine-social-and-local-login-accounts"></a>合并社交和本地登录帐户

若要完成此部分，必须先启用外部身份验证提供程序。 请参阅[Facebook、 Google、 和外部提供程序身份验证](xref:security/authentication/social/index)。

你可以通过单击电子邮件链接组合本地和社交帐户。 按以下顺序"RickAndMSFT@gmail.com"首先创建为本地登录名; 但是，你可以创建帐户作为的社交登录名，然后再添加本地登录名。

![Web 应用程序：RickAndMSFT@gmail.com身份验证的用户](accconfirm/_static/rick.png)

单击**管理**链接。 请注意与此帐户关联 0 外部 （社交登录名）。

![管理视图](accconfirm/_static/manage.png)

单击另一个登录服务的链接，接受应用程序请求。 下图中，在 Facebook 是外部身份验证提供程序：

![管理外部登录名视图列出 Facebook](accconfirm/_static/fb.png)

已经合并两个帐户。 现在可以使用任一帐户登录。 你可能希望添加本地帐户，以防其社交登录身份验证服务已关闭，或更有可能已丢失到其社交帐户访问你用户。

## <a name="enable-account-confirmation-after-a-site-has-users"></a>启用帐户确认后某个站点具有用户

启用站点上与用户的帐户确认锁定更改所有现有用户。 现有用户被锁定，因为他们的帐户不确认。 若要解决退出用户锁定，请使用以下方法之一：

* 更新数据库，将标记为正在确认的所有现有用户
* 确认正在退出用户。 例如，批处理-发送电子邮件中的标注确认链接。
