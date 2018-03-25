---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "具有 ASP.NET 标识使用 SMS 和电子邮件的双因素身份验证 |Microsoft 文档"
author: HaoK
description: "本教程将演示如何设置双因素身份验证 (2FA) 使用 SMS 和电子邮件。 由 Rick Anderson 撰写本文时 ( @RickAndMSFT )、 Pr...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0f9ff7cf74048a008b150da1e843ff15333269ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>具有 ASP.NET 标识使用 SMS 和电子邮件的双因素身份验证
====================
通过[Hao 永远](https://github.com/HaoK)， [Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Suhas Joshi](https://github.com/suhasj)

> 本教程将演示如何设置双因素身份验证 (2FA) 使用 SMS 和电子邮件。
> 
> 由 Rick Anderson 撰写本文时 ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))，Pranav Rastogi ([@rustd](https://twitter.com/rustd))，Hao 永远和 Suhas Joshi。 NuGet 示例主要由 Hao 永远编写。


本主题涵盖以下方面：

- [生成标识示例](#build)
- [设置 SMS 进行双因素身份验证](#SMS)
- [启用双因素身份验证](#enable2)
- [如何注册双因素身份验证提供程序](#reg)
- [合并社交和本地登录帐户](#combine)
- [帐户锁定免受暴力攻击](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>生成标识示例

在本部分中，你将使用 NuGet 下载我们会使用的示例。 首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安装 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。

> [!NOTE]
> 警告： 你必须安装 Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)来完成本教程。


1. 创建一个新***空***ASP.NET Web 项目。
2. 在程序包管理器控制台中，输入以下以下命令：  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 在本教程中，我们将使用[SendGrid](http://sendgrid.com/)发送电子邮件和[Twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)有关 sms texting。 `Identity.Samples`程序包将安装我们将使用的代码。
3. 设置[项目以使用 SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
4. *可选*： 按照中的说明我[电子邮件确认教程](account-confirmation-and-password-recovery-with-aspnet-identity.md)以挂钩 SendGrid 然后运行应用并注册电子邮件帐户。
5. * 可选: * 的示例删除演示电子邮件链接确认代码 (`ViewBag.Link`帐户控制器中的代码。 请参阅`DisplayEmail`和`ForgotPasswordConfirmation`操作方法和 razor 视图)。
6. * 可选: * 删除`ViewBag.Status`代码从管理和帐户控制器和*Views\Account\VerifyCode.cshtml*和*Views\Manage\VerifyPhoneNumber.cshtml* razor 视图。 或者，你仍可以保留`ViewBag.Status`显示效果以测试本地而无需挂钩和发送电子邮件和短信的此应用程序工作原理。

> [!NOTE]
> 警告： 如果你更改任何在此示例中的安全设置，生产应用将需要经过显式调出所做的更改的安全审核。


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>设置 SMS 进行双因素身份验证

本教程提供有关使用 Twilio 或 ASPSMS 的说明，但你可以使用任何其他 SMS 提供程序。

1. **使用 SMS 提供程序创建的用户帐户**  
  
 创建[Twilio](https://www.twilio.com/try-twilio)或[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)帐户。
2. **安装其他包或添加服务引用**  
  
 Twilio:  
 在程序包管理器控制台中，输入以下命令：  
    `Install-Package Twilio`  
  
 ASPSMS:  
 下面的服务引用需要添加：  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 地址:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 命名空间:  
    `ASPSMSX2`
3. **了解 SMS 提供程序用户凭据**  
  
 Twilio:  
 从**仪表板**选项卡上的 Twilio 帐户，复制**帐户 SID**和**身份验证令牌**。  
  
 ASPSMS:  
 从你的帐户设置，导航到**用户密钥**并将其复制以及你自定义**密码**。  
  
 我们将更高版本将这些值存储在变量中`SMSAccountIdentification`和`SMSAccountPassword`。
4. **指定 SenderID / 发起方**  
  
 Twilio:  
 从**数字**选项卡上，复制你的 Twilio 电话号码。  
  
 ASPSMS:  
 在**解锁原始发件人**菜单上，解锁一个或多个发送方或选择 （不支持的所有网络） 的字母数字发起方。  
  
 我们将更高版本将此值存储在变量`SMSAccountFrom`。
5. **将 SMS 提供程序凭据传输到应用程序**  
  
 提供的凭据和发件人的电话号码向应用程序：

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > 安全-永远不会存储在源代码中敏感数据。 帐户和凭据添加到上面为了让示例比较简单的代码。 请参阅 Jon 输入[ASP.NET MVC： 保留源控件的专用设置扩展](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。
6. **数据传输到 SMS 提供程序的实现**  
  
 配置`SmsService`类*应用\_Start\IdentityConfig.cs*文件。  
  
 具体取决于使用 SMS 提供程序激活或者**Twilio**或**ASPSMS**部分： 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. 运行应用程序和你之前注册的帐户登录。
8. 单击你的用户 ID，将激活`Index`中的操作方法`Manage`控制器。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. 单击添加。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. 几秒钟后你将获取验证码短信。 输入它，然后按**提交**。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. 管理视图显示已添加你的电话号码。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>检查代码

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index`中的操作方法`Manage`控制器设置基于上一个操作的状态消息，并提供更改您的本地密码或添加一个本地帐户的链接。 `Index`方法还会显示状态或你 2FA 电话版本号、 外部登录名、 2FA 启用，并请记住 （稍后进行说明） 此浏览器的 2FA 方法。 单击你的标题栏中的用户 ID （电子邮件） 不会将消息传递。 单击**电话号码： 删除**链接传递`Message=RemovePhoneSuccess`作为查询字符串。  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber`操作方法会显示一个对话框，输入电话号码可以接收 SMS 消息。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

单击**发送验证码**按钮发送到 HTTP POST 的电话号码`AddPhoneNumber`操作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync`方法生成将在 SMS 消息中设置的安全令牌。 如果已配置 SMS 服务，会在字符串令牌发送&quot;你的安全代码是&lt;令牌&gt;&quot;。 `SmsService.SendAsync`以异步方式调用方法，则应用将重定向到`VerifyPhoneNumber`操作方法 （其中显示以下对话框），你可以在其中输入验证代码。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

一旦你输入代码，然后单击提交，将代码发布到 HTTP POST`VerifyPhoneNumber`操作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync`方法检查已发布的安全代码。 如果代码正确无误，电话号码将添加到`PhoneNumber`字段`AspNetUsers`表。 如果该调用成功，`SignInAsync`调用方法：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent`参数设置是否跨多个请求身份验证会话保持不变。

当你更改你的安全配置文件时，请生成并存储在新的安全戳`SecurityStamp`字段*AspNetUsers*表。 请注意，`SecurityStamp`字段是不同的安全 cookie。 安全 cookie 不存储在`AspNetUsers`表 （或在标识数据库中的其他任何位置）。 使用自签名安全 cookie 令牌[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)并使用创建`UserId, SecurityStamp`和过期时间信息。

Cookie 中间件检查每个请求上的 cookie。 `SecurityStampValidator`中的方法`Startup`类的命中率数据库和我们会定期检查安全戳指定与`validateInterval`。 除非您更改安全配置文件，这仅发生 （在我们的示例） 每隔 30 分钟。 30 分钟时间间隔内已选择最大程度减少对数据库的访问。

`SignInAsync`方法所需的安全配置文件进行任何更改时调用。 该数据库时的安全配置文件更改，是更新`SecurityStamp`字段，和而不调用`SignInAsync`方法将保持登录*仅*直到下一次 OWIN 管道命中数据库 ( `validateInterval`). 你可以通过更改测试此`SignInAsync`方法以立即返回，并设置 cookie`validateInterval`属性从 30 分钟为 5 秒：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

使用上面的代码更改，你可以更改你的安全配置文件 (例如，通过更改状态的**两个因素启用**) 和你将被注销在 5 秒内时`SecurityStampValidator.OnValidateIdentity`方法失败。 删除中的返回行`SignInAsync`方法，请更改的另一个安全配置文件和你将不被注销。`SignInAsync`方法将生成新的安全 cookie。

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>启用双因素身份验证

在示例应用中，你需要使用 UI 启用双因素身份验证 (2FA)。 若要启用 2FA，单击你在导航栏中的用户 ID （电子邮件别名）。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
单击启用 2FA。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) 注销，然后重新登录。 如果已启用电子邮件 (请参阅我[以前一教程](account-confirmation-and-password-recovery-with-aspnet-identity.md))，你可以选择 SMS 或 2FA 的电子邮件。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) 将显示验证代码页，你可以 （从 SMS 或电子邮件） 输入代码。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) 单击**记住此浏览器**复选框将免除你无需使用 2FA 具有该计算机和浏览器登录。 启用 2FA 和单击**记住此浏览器**将为你提供强 2FA 保护免受恶意用户尝试访问你的帐户，只要它们无权访问你的计算机。 可以经常使用的所有专用计算机上执行此操作。 通过设置**记住此浏览器**、 从计算机不经常使用的情况下，获取 2FA 的附加的安全性，并可以在无需在你自己的计算机上经历 2FA 方便地。 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>如何注册双因素身份验证提供程序

当你创建新的 MVC 项目中， *IdentityConfig.cs*文件包含以下代码以注册双因素身份验证提供程序：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>添加 2FA 的电话号码

`AddPhoneNumber`中的操作方法`Manage`控制器生成的安全令牌，并且发送到电话号码你已经提供。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

后发送令牌，它将重定向到`VerifyPhoneNumber`操作方法，你可以在其中输入要为 2FA 注册 SMS 的代码。 在验证的电话号码之前未使用 SMS 2FA。

## <a name="enabling-2fa"></a>启用 2FA

`EnableTFA`操作方法使 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

请注意`SignInAsync`必须调用，因为启用 2FA 是做的安全配置文件的更改。 启用 2FA 后，用户将需要使用 2FA 登录，使用用户已注册 （SMS 和在此示例中的电子邮件） 的 2FA 方法。

您可以添加多个 2FA 提供程序，如 QR 代码生成器也可以编写你自己的 (请参阅[使用 Google 身份验证器具有 ASP.NET 标识](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。

> [!NOTE]
> 使用生成 2FA 代码[基于时间的一次性密码算法](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)和代码的有效期为 6 分钟。 如果需要六个分钟以上输入的代码，你将收到无效的代码错误消息。


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>合并社交和本地登录帐户

你可以通过单击电子邮件链接组合本地和社交帐户。 按以下顺序&quot; RickAndMSFT@gmail.com &quot;最初创建为本地登录名，但你可以创建帐户作为社交日志中的第一个，然后添加本地登录名。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

单击**管理**链接。 请注意与此帐户关联 0 外部 （社交登录名）。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

单击服务中的另一个日志的链接，接受应用程序请求。 现在合并两个帐户，你将能够使用任一帐户登录。 你可能希望用户身份验证服务中的其社交日志已关闭，或已对其社交帐户失去访问更有可能的情况下添加本地帐户。

在下图中，Tom 是一个社交登录 (您可以看到从该**外部登录名： 1**的页上显示)。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

单击**挑选一个密码**允许你上添加本地日志与相同的帐户关联。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>帐户锁定免受暴力攻击

你可以通过启用用户锁定保护字典式攻击你的应用上的帐户。 下面的代码中`ApplicationUserManager Create`方法配置在锁住：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

上面的代码中启用双重身份验证的锁定。 虽然你可以通过更改启用的登录名在锁住`shouldLockout`为 true`Login`在 account 控制器的方法，我们建议则不能启用的登录名在锁住，因为它会使该帐户容易遭受[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)登录名攻击。 在示例代码中，为管理员帐户创建中禁用锁定`ApplicationDbInitializer Seed`方法：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>要求用户具有一个已验证电子邮件帐户

下面的代码要求用户具有一个已验证电子邮件帐户，然后它们可以在登录：

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>SignInManager 如何检查 2FA 要求

同时本地登录和社交日志中检查，以查看是否已启用 2FA 中。 如果启用了 2FA，`SignInManager`登录方法返回`SignInStatus.RequiresVerification`，并将用户重定向到`SendCode`操作方法，它们将需要输入代码以完成日志序列中。 如果用户具有记住我设置在用户本地 cookie 上,`SignInManager`将返回`SignInStatus.Success`，它们将不需要经历 2FA。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

下面的代码演示`SendCode`操作方法。 A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)使用为用户启用的所有 2FA 方法创建。 [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)传递给[DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx)帮助器，这样用户就可以选择 2FA 方法 （通常电子邮件和短信）。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

一旦用户发送 2FA 方法中，`HTTP POST SendCode`调用操作方法，`SignInManager`发送 2FA 代码和用户重定向到`VerifyCode`它们可以在其中输入要完成的日志中的代码的操作方法。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA 锁定

虽然可以在登录密码尝试失败次数中设置帐户锁定，这种方法使你的登录名容易遭受[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)锁定。 我们建议只适用于 2FA 使用帐户锁定。 当`ApplicationUserManager`是创建，示例代码将设置 2FA 锁定和`MaxFailedAccessAttemptsBeforeLockout`为五。 一旦用户 （通过本地帐户或社交帐户） 登录，存储在 2FA 尝试每个失败，而且如果达到最大尝试次数，则锁定用户五分钟时间 (你可以设置与时间锁定`DefaultAccountLockoutTimeSpan`)。

<a id="addRes"></a>

## <a name="additional-resources"></a>其他资源

- [ASP.NET 标识的推荐资源](../getting-started/aspnet-identity-recommended-resources.md)因此链接的标识博客、 视频、 教程和出色的完整列表。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录的 MVC 5 应用程序](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)还演示如何将配置文件信息添加到用户表。
- [ASP.NET MVC 和标识 2.0： 了解基础知识](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)通过 John 输入。
- [帐户确认和密码恢复 ASP.NET 标识](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET 标识简介](../getting-started/introduction-to-aspnet-identity.md)
- [宣布推出适用的 ASP.NET 标识 2.0.0 RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi 通过。
- [ASP.NET 标识 2.0： 设置帐户验证和双因素授权](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)通过 John 输入。
