---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "电子邮件确认及密码重置 (C#) 使用日志中，创建安全的 ASP.NET MVC 5 web 应用程序 |Microsoft 文档"
author: Rick-Anderson
description: "本教程演示了如何生成与电子邮件确认及密码重置使用 ASP.NET 标识成员资格系统的 ASP.NET MVC 5 web 应用程序。 Ca..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: d55b34135d5bab98ab8de31cc4b12dcc272cbc0a
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>创建安全的 ASP.NET MVC 5 web 应用程序日志中，使用电子邮件确认及密码重置 (C#)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程演示了如何生成与电子邮件确认及密码重置使用 ASP.NET 标识成员资格系统的 ASP.NET MVC 5 web 应用程序。 你可以下载已完成的应用程序[此处](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 下载包含能让你无需设置电子邮件或 SMS 提供程序中测试电子邮件确认和短信的调试帮助程序。
> 
> 本教程编写的[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>创建 ASP.NET MVC 应用程序

首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。

> [!NOTE]
> 警告： 你必须安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以完成本教程。


1. 创建一个新的 ASP.NET Web 项目，然后选择 MVC 模板中。 Web 窗体还支持 ASP.NET 标识，因此无法执行类似的步骤，在 web 窗体应用程序中。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. 保留默认的身份验证作为**单个用户帐户**。 如果你想要托管该应用程序在 Azure 中的，保留选中复选框。 稍后在本教程中我们将部署到 Azure。 你可以[免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。
3. 设置[项目以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. 运行应用程序，请单击**注册**链接并注册用户。 此时，电子邮件的唯一验证是使用[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。
5. 在服务器资源管理器，导航到**数据 Connections\DefaultConnection\Tables\AspNetUsers**，右键单击并选择**打开表定义**。

    下图显示`AspNetUsers`架构：

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. 右键单击**AspNetUsers**表，然后选择**显示表数据**。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 此时不已确认电子邮件。
7. 单击对应的行，并选择删除。 将在下一步的步骤中，再次添加此电子邮件并发送确认电子邮件。

## <a name="email-confirmation"></a>电子邮件确认

它是一种最佳做法以确认新的用户注册，以验证它们不模拟其他人的电子邮件 （即，它们尚未注册与其他人的电子邮件）。 假设你有论坛，你想要防止`"bob@example.com"`从将注册为`"joe@contoso.com"`。 而无需电子邮件确认`"joe@contoso.com"`无法从您的应用程序获取不需要的电子邮件。 假设 Bob 意外注册为`"bib@example.com"`和未注意到，他将无法使用密码恢复，因为此应用程序没有他正确的电子邮件。 电子邮件确认从机器人提供有限的保护，并从确定垃圾邮件发送者不对提供保护，它们具有许多可用于注册的工作电子邮件别名。

你通常想要使新用户不能发布到网站的任何数据之前已通过电子邮件，SMS 文本消息或其他机制对它们已确认。 <a id="build"></a>在下面部分中，我们将启用电子邮件确认并修改代码以防止新注册的用户登录，直到已确认其电子邮件。

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>挂钩 SendGrid

虽然本教程只显示如何添加通过电子邮件通知[SendGrid](http://sendgrid.com/)，你可以发送电子邮件使用 SMTP 和其他机制 (请参阅[其他资源](#addRes))。

1. 在程序包管理器控制台中，输入以下以下命令： 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. 转到[Azure SendGrid 注册页面](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)并注册一个免费的 SendGrid 帐户。 通过添加类似于在以下代码来配置 SendGrid *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

你将需要添加下列内容包括：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

为了简化此示例，我们将存储中的应用设置*web.config*文件：

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> 安全-永远不会存储在源代码中敏感数据。 帐户和凭据存储在 appSetting。 在 Azure 上，你可以安全地存储这些值在**[配置](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**在 Azure 门户中的选项卡。 请参阅[用于密码和其他敏感数据部署到 ASP.NET 和 Azure 的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。


### <a name="enable-email-confirmation-in-the-account-controller"></a>启用帐户控制器中的电子邮件确认

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

验证*Views\Account\ConfirmEmail.cshtml*文件具有正确的 razor 语法。 (在第一个字符 @ 行可能已丢失。 )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

运行应用并单击注册链接。 一旦提交注册表单，你已登录。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

检查你的电子邮件帐户，然后单击链接以确认你的电子邮件。

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>需要电子邮件确认之前登录

当前用户完成后注册表单，则将它们记录在中。 你通常想要在中记录日志之前，请确认其电子邮件。 在下面的部分中，我们将修改代码后，需要以具有确认电子邮件，然后将它们记录在 （经过身份验证） 的新用户。 更新`HttpPost Register`与以下突出显示的更改的方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

通过注释掉`SignInAsync`方法，用户不会进行签名的注册。 `TempData["ViewBagLink"] = callbackUrl;`行可用于[调试应用程序](#dbg)并测试而不发送电子邮件的注册。 `ViewBag.Message` 用于显示的确认说明进行操作。 [下载示例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)包含代码来测试电子邮件确认，无需设置电子邮件，并且也可以使用来调试该应用程序。

创建`Views\Shared\Info.cshtml`文件并添加以下 razor 标记：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

添加[Authorize 属性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)到`Contact`主控制器的操作方法。 你可以使用单击**联系人**链接以验证匿名用户无权访问和经过身份验证的用户具有访问权限。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

你还必须更新`HttpPost Login`操作方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

更新*Views\Shared\Error.cshtml*视图以显示错误消息：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

删除中的任何帐户**AspNetUsers**表包含你要测试的电子邮件别名。 运行应用程序并验证直到您已确认你的电子邮件地址无法登录。 一旦确认你的电子邮件地址，请单击**联系人**链接。

<a id="reset"></a>
## <a name="password-recoveryreset"></a>密码恢复/重置

删除注释字符从`HttpPost ForgotPassword`帐户控制器中的操作方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

删除注释字符从`ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)中*Views\Account\Login.cshtml* razor 视图文件：

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

登录页现在将显示的链接重置的密码。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>重新发送电子邮件确认链接

一旦用户创建新的本地帐户，它们发送需要它们来使用它们可以登录之前的确认链接。 如果用户意外删除确认电子邮件，或电子邮件永远不会到达时，他们将需要再次发送的确认链接。 以下代码更改显示如何启用此功能。

将下面的 helper 方法添加到底部*Controllers\AccountController.cs*文件：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

更新要使用的新的帮助程序的注册方法：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

更新要重新发送密码的登录方法时如果尚未确认的用户帐户：

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>合并社交和本地登录帐户

你可以通过单击电子邮件链接组合本地和社交帐户。 按以下顺序 **RickAndMSFT@gmail.com** 最初创建为本地登录名，但你可以创建帐户作为社交日志中的第一个，然后添加本地登录名。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

单击**管理**链接。 请注意与此帐户关联 0 外部 （社交登录名）。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

单击服务中的另一个日志的链接，接受应用程序请求。 现在合并两个帐户，你将能够使用任一帐户登录。 你可能希望用户身份验证服务中的其社交日志已关闭，或已对其社交帐户失去访问更有可能的情况下添加本地帐户。

在下图中，Tom 是一个社交登录 (您可以看到从该**外部登录名： 1**的页上显示)。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

单击**挑选一个密码**允许你上添加本地日志与相同的帐户关联。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>中更深入的电子邮件确认

我的教程[帐户确认和密码恢复 ASP.NET 标识](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)进入更多详细信息与本主题。

<a id="dbg"></a>
## <a name="debugging-the-app"></a>调试应用程序

如果你不会获得电子邮件包含链接：

- 请检查垃圾邮件或垃圾邮件文件夹。
- 登录到你的 SendGrid 帐户，然后单击[电子邮件活动链接](https://sendgrid.com/logs/index)。

若要测试验证链接不电子邮件的情况下，下载[已完成的示例](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 将页面上显示的确认链接和确认代码。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他资源

- [指向 ASP.NET Identity 的推荐资源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [帐户确认和密码恢复具有 ASP.NET 标识](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)进入密码恢复和帐户确认的详细信息。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录的 MVC 5 应用程序](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教程演示如何编写使用 Facebook 和 Google oauth2 授权 ASP.NET MVC 5 应用程序。 它还演示如何向标识数据库中添加额外的数据。
- [将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 应用程序部署到 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 本教程将添加 Azure 部署，如何保护使用角色，对应用程序如何使用成员资格 API 来添加用户和角色，以及其他安全功能。
- [创建 oauth2 的 Google 应用和将应用程序连接到项目](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [在 Facebook 中创建应用并将应用程序连接到项目](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [在项目中的 SSL 设置](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
