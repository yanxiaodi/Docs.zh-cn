---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: "帐户确认和密码恢复具有 ASP.NET 标识 (C#) |Microsoft 文档"
author: HaoK
description: "在进行你应首先完成本教程之前具有登录、 电子邮件确认及密码重置创建安全的 ASP.NET MVC 5 web 应用程序。 本教程中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 548baaaa06980fb793c079b66b6edc34422eb579
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>帐户确认和密码恢复 ASP.NET 标识 (C#)
====================
通过[Hao 永远](https://github.com/HaoK)， [Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 在学习本教程之前你应首先完成[创建安全的 ASP.NET MVC 5 web 应用程序具有登录、 电子邮件确认及密码重置](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。 本教程包含更多详细信息，并将向你演示如何设置本地帐户确认的电子邮件，并允许用户在 ASP.NET Identity 中的忘记了的密码重置。 由 Rick Anderson 撰写本文时 ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))，Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Hao 永远和 Suhas Joshi。 NuGet 示例主要由 Hao 永远编写。


本地用户帐户要求用户创建的帐户密码，该密码 （安全） 存储在 web 应用。 ASP.NET 标识还支持社交帐户，不需要用户创建应用程序的密码。 [社交帐户](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)使用第三方 （如 Google、 Twitter、 Facebook 或 Microsoft） 用户进行身份验证。 本主题涵盖以下方面：

- [创建的 ASP.NET MVC 应用](#createMvc)和浏览 ASP.NET 标识功能。
- [生成标识示例](#build)
- [设置电子邮件确认](#email)

新用户注册其电子邮件别名，这将创建本地帐户。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

单击注册按钮会将发送确认电子邮件包含到其电子邮件地址的验证令牌。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

向用户发送包含其帐户的确认令牌的电子邮件。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

单击链接确认该帐户。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>密码恢复/重置

本地用户忘记其密码时都可以具有安全令牌发送到其电子邮件帐户，使他们能够重置其密码。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
允许在它们重置其密码的链接，则用户很快将收到一封电子邮件。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
单击该链接会将他们重置页。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
单击**重置**按钮将确认重置密码。  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>创建 ASP.NET Web 应用

首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安装 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。

> [!NOTE]
> 警告： 你必须安装 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)来完成本教程。


1. 创建一个新的 ASP.NET Web 项目，然后选择 MVC 模板中。 Web 窗体还支持 ASP.NET 标识，因此无法执行类似的步骤，在 web 窗体应用程序中。
2. 保留默认的身份验证作为**单个用户帐户**。
3. 运行应用程序，请单击**注册**链接并注册用户。 此时，电子邮件的唯一验证是使用[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。
4. 在服务器资源管理器，导航到**数据 Connections\DefaultConnection\Tables\AspNetUsers**，右键单击并选择**打开表定义**。

    下图显示`AspNetUsers`架构：

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. 右键单击**AspNetUsers**表，然后选择**显示表数据**。  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
 此时不已确认电子邮件。

ASP.NET 标识的默认数据存储是实体框架，但你可以配置为使用其他数据存储，以及添加其他字段。 请参阅[其他资源](#addRes)在本教程末尾的部分。

[OWIN startup 类](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md)( *Startup.cs* ) 时应用程序启动并调用调用`ConfigureAuth`中的方法*应用\_Start\Startup.Auth.cs*，用于配置 OWIN 管道，并初始化 ASP.NET 标识。 检查`ConfigureAuth`方法。 每个`CreatePerOwinContext`调用注册的回调 (保存在`OwinContext`)，将调用一次每个请求来创建指定类型的实例。 你可以构造函数中设置一个断点和`Create`的每种类型的方法 (`ApplicationDbContext, ApplicationUserManager`) 并验证它们在每个请求上调用。 实例`ApplicationDbContext`和`ApplicationUserManager`存储在 OWIN 上下文中，可以在整个应用程序中访问。 ASP.NET 标识挂钩到 OWIN 管道通过 cookie 中间件。 有关详细信息，请参阅[每个请求 UserManager 在 ASP.NET Identity 中的类的生存期管理](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx)。

当你更改你的安全配置文件时，请生成并存储在新的安全戳`SecurityStamp`字段*AspNetUsers*表。 请注意，`SecurityStamp`字段是不同的安全 cookie。 安全 cookie 不存储在`AspNetUsers`表 （或在标识数据库中的其他任何位置）。 使用自签名安全 cookie 令牌[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)并使用创建`UserId, SecurityStamp`和过期时间信息。

Cookie 中间件检查每个请求上的 cookie。 `SecurityStampValidator`中的方法`Startup`类的命中率数据库和我们会定期检查安全戳指定与`validateInterval`。 除非您更改安全配置文件，这仅发生 （在我们的示例） 每隔 30 分钟。 30 分钟时间间隔内已选择最大程度减少对数据库的访问。 请参阅我[双因素身份验证教程](index.md)有关详细信息。

在代码中，注释每`UseCookieAuthentication`方法支持 cookie 身份验证。 `SecurityStamp`字段和关联的代码提供了一层额外的向应用程序的安全性，如果你更改密码，你将记录超出您登录所用的浏览器。 `SecurityStampValidator.OnValidateIdentity`方法使应用程序来验证安全令牌，当用户登录时更改密码或使用外部登录名时使用。 需要这些信息来确保使用旧密码生成任何标记 (cookie) 将会失效。 在示例项目中，如果更改为用户生成的用户密码然后新的令牌，任何以前的令牌失效和`SecurityStamp`更新字段。

标识系统，可以配置你的应用，用户的安全配置文件更改时 (例如，当用户更改其密码或更改关联的登录名 (如 Facebook、 Google、 Microsoft 帐户，等等)，从所有注销用户浏览器实例。 例如，下面的图像显示[单点注销示例](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)应用，这样用户就可以通过单击一个按钮注销 （在此情况下，IE、 Firefox 和 Chrome） 的所有浏览器实例。 或者，该示例可以仅注销的特定浏览器实例。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

[单点注销示例](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt)应用演示如何 ASP.NET 标识允许你重新生成安全令牌。 需要这些信息来确保使用旧密码生成任何标记 (cookie) 将会失效。 此功能提供一层额外的安全到你的应用程序;如果你更改密码，你将记录出其中您已登录到此应用程序。

*应用\_Start\IdentityConfig.cs*文件包含`ApplicationUserManager`，`EmailService`和`SmsService`类。 `EmailService`和`SmsService`的类，每个实现`IIdentityMessageService`接口，因此你必须在每个类以配置电子邮件和短信的公共方法。 虽然本教程只显示如何添加通过电子邮件通知[SendGrid](http://sendgrid.com/)，你可以发送电子邮件使用 SMTP 和其他机制。

`Startup`类还包含样板添加社交登录名 （Facebook、 Twitter 等），请参阅我的教程[MVC 5 应用程序使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)有关详细信息。

检查`ApplicationUserManager`类，该类包含的用户标识信息并配置以下功能：

- 密码强度要求。
- 用户在锁住 （尝试次数和时间）。
- 双因素身份验证 (2FA)。 将另一个教程中介绍 2FA 和短信。
- 挂接电子邮件和 SMS 服务。 （我将介绍 SMS 另一个教程中）。

`ApplicationUserManager`类派生自泛型`UserManager<ApplicationUser>`类。 `ApplicationUser`派生自[IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx)。 `IdentityUser`派生自泛型`IdentityUser`类：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

上面的属性中的属性与同时发生`AspNetUsers`表，如上所示。

上的泛型参数`IUser`使你能够为主键使用不同类型的类。 请参阅[ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)示例演示如何将为主键从字符串更改为 int 或 GUID。

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser`(`public class ApplicationUserManager : UserManager<ApplicationUser>`) 中定义*Models\IdentityModels.cs*作为：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

上面的突出显示的代码生成[ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx)。 ASP.NET 标识和 OWIN Cookie 身份验证都是基于声明的因此，框架需要应用程序以生成`ClaimsIdentity`用户。 `ClaimsIdentity`具有信息有关的用户，如用户名、 的所有声明 age 和用户属于哪些角色。 在此阶段，你还可以添加用户的多个的声明。

OWIN`AuthenticationManager.SignIn`方法通过中`ClaimsIdentity`并对用户进行签名：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录的 MVC 5 应用程序](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)演示如何将添加到的其他属性`ApplicationUser`类。

## <a name="email-confirmation"></a>电子邮件确认

它是一个好办法确认新用户注册以验证它们不模拟其他人的电子邮件 （即，它们尚未注册与其他人的电子邮件）。 假设你有论坛，你想要防止`"bob@example.com"`从将注册为`"joe@contoso.com"`。 而无需电子邮件确认`"joe@contoso.com"`无法从您的应用程序获取不需要的电子邮件。 假设 Bob 意外注册为`"bib@example.com"`和未注意到，他将无法使用密码恢复，因为此应用程序没有他正确的电子邮件。 电子邮件确认从机器人提供有限的保护，并从确定垃圾邮件发送者不对提供保护，它们具有许多可用于注册的工作电子邮件别名。在下面的示例中，用户将无法更改其密码，直到已确认其帐户 (由它们在它们注册的电子邮件帐户上确认链接上单击收到。)你可以应用此工作流到其他方案中，例如发送链接以确认并重置由管理员，向用户发送一封电子邮件，当它们等更改其配置文件创建的新帐户的密码。 你通常想要使新用户不能发布到网站的任何数据之前已通过电子邮件，SMS 文本消息或其他机制对它们已确认。 <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>生成更完整示例

在本部分中，你将使用 NuGet 下载的更完整示例，我们会使用。

1. 创建一个新***空***ASP.NET Web 项目。
2. 在程序包管理器控制台中，输入以下以下命令： 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

 在本教程中，我们将使用[SendGrid](http://sendgrid.com/)发送电子邮件。 `Identity.Samples`程序包将安装我们将使用的代码。
3. 设置[项目以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. 通过运行应用程序中，单击测试本地帐户创建**注册**链接，然后发布注册表单。
5. 单击演示电子邮件链接，这将模拟电子邮件确认。
6. 删除的示例演示电子邮件链接确认代码 (`ViewBag.Link`帐户控制器中的代码。 请参阅`DisplayEmail`和`ForgotPasswordConfirmation`操作方法和 razor 视图)。

> [!NOTE]
> 警告： 如果你更改任何在此示例中的安全设置，生产应用将需要接受安全性审核，其中明确所做的更改。


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>检查应用中的代码\_Start\IdentityConfig.cs

此示例演示如何创建一个帐户并将其添加到*管理员*角色。 应将替换为你将使用管理帐户的电子邮件的示例中的电子邮件。 现在创建管理员帐户的最简单方法是以编程方式在`Seed`方法。 我们希望能够在将来将允许你用于创建和管理用户和角色的工具。 示例代码确实允许您创建和管理用户和角色，但你必须首先具有管理员帐户以运行的角色和用户管理页面。 在此示例中，在数据库设置种子值时创建的管理员帐户。

更改密码，将名称更改为可以收到电子邮件通知的位置的帐户。

> [!WARNING]
> 安全-永远不会存储在源代码中敏感数据。

如前所述， `app.CreatePerOwinContext` startup 类中的调用添加到的回调`Create`应用 DB 内容，用户管理器和角色管理器类的方法。 OWIN 管道调用`Create`方法这些类为每个请求，并将存储每个类的上下文。 在 account 控制器公开从 HTTP 上下文 （其中包含 OWIN 上下文） 的用户管理器：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

当用户注册一个本地帐户，`HTTP Post Register`调用方法：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

上面的代码中使用模型数据创建新的用户帐户使用的电子邮件和输入的密码。 如果的电子邮件别名是数据存储区，帐户创建失败，并再次显示窗体。 `GenerateEmailConfirmationTokenAsync`方法创建一个安全确认令牌并将其存储在 ASP.NET Identity 数据存储区。 [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx)方法创建一个链接，其中包含`UserId`和确认令牌。 然后向用户电子邮件发送此链接，用户可以单击其电子邮件应用以确认其帐户中的链接。

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>设置电子邮件确认

转到[Azure SendGrid 注册页面](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)并注册免费帐户。 添加类似于以下内容来配置 SendGrid 的代码：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> 电子邮件客户端频繁地只接受文本消息 (无 HTML)。 应提供文本和 HTML 中的消息。 在上面的 SendGrid 示例中，这完成与`myMessage.Text`和`myMessage.Html`上面所示的代码。


下面的代码演示如何发送电子邮件使用[MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx)类 where`message.Body`返回仅的链接。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> 安全-永远不会存储在源代码中敏感数据。 帐户和凭据存储在 appSetting。 在 Azure 上，你可以安全地存储这些值在**[配置](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**在 Azure 门户中的选项卡。 请参阅[用于密码和其他敏感数据部署到 ASP.NET 和 Azure 的最佳做法](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。


输入你的 SendGrid 凭据、 运行应用，注册的电子邮件别名可以单击你的电子邮件中的确认链接。 若要了解如何执行此操作与你[Outlook.com](http://outlook.com)电子邮件帐户，请参阅 John 输入[的 Outlook.Com SMTP 主机的 C# SMTP 配置](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx)和他[ASP.NET 标识 2.0： 设置帐户验证和双因素授权](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)文章。

用户单击后**注册**确认电子邮件包含的验证令牌发送到其电子邮件地址的按钮。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

向用户发送包含其帐户的确认令牌的电子邮件。

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>检查代码

以下代码演示了 `POST ForgotPassword` 方法。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

如果尚未确认的用户电子邮件，该方法将以静默方式失败。 如果错误已发布的无效电子邮件地址，恶意用户无法使用该信息来查找有效 userId （电子邮件别名） 攻击。

下面的代码演示`ConfirmEmail`当用户单击电子邮件发送到它们中的确认链接时调用的帐户控制器中的方法：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

一旦已使用忘记了的密码令牌，它会失效。 在下面的代码更改`Create`方法 (在*应用\_Start\IdentityConfig.cs*文件) 设置为 3 小时内过期的令牌。

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

使用上面的代码中，忘记了的密码和电子邮件确认令牌将在过期 3 小时。 默认值`TokenLifespan`为一天。

下面的代码演示了电子邮件确认方法：

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 若要使你的应用程序更安全的情况下，ASP.NET 标识支持双因素身份验证 (2FA)。 请参阅[ASP.NET 标识 2.0： 设置帐户验证和双因素授权](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)通过 John 输入。 虽然可以在登录密码尝试失败次数中设置帐户锁定，这种方法使你的登录名容易遭受[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)锁定。 我们建议只适用于 2FA 使用帐户锁定。  
<a id="addRes"></a>

## <a name="additional-resources"></a>其他资源

- [ASP.NET 标识的自定义存储提供程序概述](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录的 MVC 5 应用程序](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)还演示如何将配置文件信息添加到用户表。
- [ASP.NET MVC 和标识 2.0： 了解基础知识](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)通过 John 输入。
- [ASP.NET 标识简介](../getting-started/introduction-to-aspnet-identity.md)
- [宣布推出适用的 ASP.NET 标识 2.0.0 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi 通过。
