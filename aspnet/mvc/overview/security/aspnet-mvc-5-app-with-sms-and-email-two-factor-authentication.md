---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: "使用 SMS 和电子邮件双因素身份验证的 ASP.NET MVC 5 应用程序 |Microsoft 文档"
author: Rick-Anderson
description: "本教程演示了如何生成使用双因素身份验证的 ASP.NET MVC 5 web 应用程序。 您应该完成与创建安全的 ASP.NET MVC 5 web 应用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: d6bc92f3cbe6b61332e33e8a507b4516bf5c15a5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>使用 SMS 和电子邮件双因素身份验证的 ASP.NET MVC 5 应用程序
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程演示了如何生成使用双因素身份验证的 ASP.NET MVC 5 web 应用程序。 您应该完成[创建安全的 ASP.NET MVC 5 web 应用程序具有登录、 电子邮件确认及密码重置](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)然后再继续。 你可以下载已完成的应用程序[此处](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)。 下载包含能让你无需设置电子邮件或 SMS 提供程序中测试电子邮件确认和短信的调试帮助程序。
> 
> 本教程编写的[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


- [创建 ASP.NET MVC 应用程序](#createMvc)
- [设置 SMS 进行双因素身份验证](#SMS)
- [启用双因素身份验证](#enable2)
- [其他资源](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>创建 ASP.NET MVC 应用程序

首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本。

> [!NOTE]
> 警告： 您应该完成[创建安全的 ASP.NET MVC 5 web 应用程序具有登录、 电子邮件确认及密码重置](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)然后再继续。 必须安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以完成本教程。


1. 创建一个新的 ASP.NET Web 项目，然后选择 MVC 模板中。 Web 窗体还支持 ASP.NET 标识，因此无法执行类似的步骤，在 web 窗体应用程序中。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. 保留默认的身份验证作为**单个用户帐户**。 如果你想要托管该应用程序在 Azure 中的，保留选中复选框。 稍后在本教程中我们将部署到 Azure。 你可以[免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。
3. 设置[项目以使用 SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 地址:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 命名空间:  
    `ASPSMSX2`
3. **了解 SMS 提供程序用户凭据**  
  
 Twilio:  
 从**仪表板**选项卡上的 Twilio 帐户，复制**帐户 SID**和**身份验证令牌**。  
  
 ASPSMS:  
 从你的帐户设置，导航到**用户密钥**并将其复制以及你自定义**密码**。  
  
 我们将更高版本将存储这些值*web.config*中密钥文件`"SMSAccountIdentification"`和`"SMSAccountPassword"`。
4. **指定 SenderID / 发起方**  
  
 Twilio:  
 从**数字**选项卡上，复制你的 Twilio 电话号码。  
  
 ASPSMS:  
 在**解锁原始发件人**菜单上，解锁一个或多个发送方或选择 （不支持的所有网络） 的字母数字发起方。  
  
 我们将更高版本存储中的此值*web.config*文件在项`"SMSAccountFrom"`。
5. **将 SMS 提供程序凭据传输到应用程序**  
  
 提供的凭据和发件人的电话号码向应用程序。 为了简单起见我们将存储这些值*web.config*文件。 当我们将部署到 Azure 时，我们可以将安全地在值存储**应用设置**网站上的部分配置选项卡。 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > 安全-永远不会存储在源代码中敏感数据。 帐户和凭据添加到上面为了让示例比较简单的代码。 请参阅[用于密码和其他敏感数据部署到 ASP.NET 和 Azure 的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
6. **数据传输到 SMS 提供程序的实现**  
  
 配置`SmsService`类*应用\_Start\IdentityConfig.cs*文件。  
  
 具体取决于使用 SMS 提供程序激活或者**Twilio**或**ASPSMS**部分： 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. 更新*Views\Manage\Index.cshtml* Razor 视图: (注意： 不只需删除现有代码中的注释，使用下面的代码。)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. 验证`EnableTwoFactorAuthentication`和`DisableTwoFactorAuthentication`中的操作方法`ManageController`具有[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx)属性：  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. 运行应用程序和你之前注册的帐户登录。
10. 单击你的用户 ID，将激活`Index`中的操作方法`Manage`控制器。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. 单击添加。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber`操作方法会显示一个对话框，输入电话号码可以接收 SMS 消息。

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. 几秒钟后你将获取验证码短信。 输入它，然后按**提交**。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. 管理视图显示已添加你的电话号码。

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>启用双因素身份验证

在模板生成应用程序中，你需要使用 UI 启用双因素身份验证 (2FA)。 若要启用 2FA，单击你在导航栏中的用户 ID （电子邮件别名）。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

单击启用 2FA。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

注销，然后注销再重新登录。 如果已启用电子邮件 (请参阅我[以前一教程](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md))，你可以选择 SMS 或 2FA 的电子邮件。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

将显示验证代码页，你可以 （从 SMS 或电子邮件） 输入代码。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

单击**记住此浏览器**复选框将免除你无需使用 2FA 以要求在登录时使用的浏览器和设备选中的复选框的位置。 只要恶意用户无法访问你的设备，启用 2FA 和单击**记住此浏览器**将为你提供方便的一次性密码访问权限，同时还可以保留的所有访问的强 2FA 保护从非受信任的设备。 可以在任何您经常使用的专用设备上执行此操作。

本教程提供了启用 2FA 上新的 ASP.NET MVC 应用程序的快速介绍。 我的教程[双因素身份验证使用 SMS 和电子邮件与 ASP.NET 标识](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)进入后面的示例代码中的详细信息。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他资源

- [具有 ASP.NET 标识使用 SMS 和电子邮件的双因素身份验证](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)进入双因素身份验证的详细信息
- [指向 ASP.NET Identity 的推荐资源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [帐户确认和密码恢复具有 ASP.NET 标识](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)进入密码恢复和帐户确认的详细信息。
- [使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录的 MVC 5 应用程序](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)本教程演示如何编写使用 Facebook 和 Google oauth2 授权 ASP.NET MVC 5 应用程序。 它还演示如何向标识数据库中添加额外的数据。
- [将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 应用程序部署到 Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 本教程将添加 Azure 部署，如何保护使用角色，对应用程序如何使用成员资格 API 来添加用户和角色，以及其他安全功能。
- [创建 oauth2 的 Google 应用和将应用程序连接到项目](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [在 Facebook 中创建应用并将应用程序连接到项目](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [在项目中的 SSL 设置](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
