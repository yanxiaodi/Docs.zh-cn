---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: "创建 ASP.NET Web 窗体应用程序与 SMS 双因素身份验证 (C#) |Microsoft 文档"
author: Erikre
description: "本教程演示了如何生成使用双因素身份验证的 ASP.NET Web 窗体应用程序。 本教程旨在补充标题为 Cr 教程..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: b1f0ec0fdefa12eb7f7b2714dbc224fef735f4bb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>创建 ASP.NET Web 窗体应用程序与 SMS 双因素身份验证 (C#)
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载包含电子邮件和短信双因素身份验证的 ASP.NET Web 窗体应用程序](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> 本教程演示了如何生成使用双因素身份验证的 ASP.NET Web 窗体应用程序。 本教程旨在补充标题为教程[创建用户注册使用的安全的 ASP.NET Web 窗体应用、 电子邮件确认及密码重置](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)。 此外，本教程基于 Rick Anderson [MVC 教程](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)。


## <a name="introduction"></a>介绍

本教程将指导您完成创建支持双因素身份验证使用 Visual Studio 的 ASP.NET Web 窗体应用程序所需的步骤。 双因素身份验证是一个额外的用户身份验证步骤。 在登录期间，此额外步骤生成唯一个人标识号 (PIN)。 PIN 通常发送到的用户为电子邮件或 SMS 消息。 你的应用程序的用户登录时，然后将 PIN 输入作为额外的身份验证度量值。

### <a name="tutorial-tasks-and-information"></a>教程任务和信息：

- [创建 ASP.NET Web 窗体应用](#createWebForms)
- [安装程序 SMS 和双因素身份验证](#SMS)
- [启用已注册用户的双因素身份验证](#use2FA)
- [其他资源](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>创建 ASP.NET Web 窗体应用

首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以及。 此外，你将需要创建[Twilio](https://www.twilio.com/try-twilio)帐户，如下所述。

> [!NOTE]
> 重要提示： 你必须安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以完成本教程。


1. 创建新的项目 (**文件** - &gt; **新项目**)，然后选择**ASP.NET Web 应用程序**模板以及.NET Framework从版本 4.5.2**新项目**对话框。
2. 从**新建 ASP.NET 项目**对话框中，选择**Web 窗体**模板。 保留默认的身份验证作为**单个用户帐户**。 然后，单击**确定**创建新项目。  
    ![新建 ASP.NET 项目对话框](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. 为项目启用安全套接字层 (SSL)。 请按照步骤中提供**为项目启用 SSL**部分[入门 Web 窗体教程系列](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)。
4. 在 Visual Studio 中，打开**程序包管理器控制台**(**工具** - &gt; **NuGet 包管理器** - &gt;**程序包管理器控制台**)，并输入以下命令：  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>安装程序 SMS 和双因素身份验证

本教程中使用 Twilio，但你可以使用任何 SMS 提供程序。

1. 创建[Twilio](https://www.twilio.com/try-twilio)帐户。
2. 从**仪表板**选项卡上的 Twilio 帐户，复制**帐户 SID**和**身份验证令牌。** 你将添加到你的应用程序更高版本。
3. 从**数字**选项卡上，复制你的 Twilio**电话号码**以及。
4. Twilio**帐户 SID**，**身份验证令牌**和**电话号码**可用于应用。 若要为简单起见将存储这些值*web.config*文件。 在部署到 Azure 时，可以安全地在值存储**appSettings**网站上的部分配置选项卡。此外，在添加时的电话号码，只能使用数字。   
 请注意，你还可以添加 SendGrid 凭据。 SendGrid 是一电子邮件通知的服务。 有关如何启用 SendGrid，有关详细信息，请参阅本教程标题为挂钩向上 SendGrid 一部分[创建用户注册使用的安全 ASP.NET Web 窗体应用、 电子邮件确认及密码重置。](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > 安全-永远不会存储在源代码中敏感数据。 在此示例中，帐户和凭据存储在**appSettings**部分*Web.config*文件。 在 Azure 上，你可以安全地存储这些值在**[配置](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**在 Azure 门户中的选项卡。 有关相关信息，请参阅标题为 Rick Anderson 主题[用于密码和其他敏感数据部署到 ASP.NET 和 Azure 的最佳做法](https://go.microsoft.com/fwlink/?LinkId=513141)。
5. 配置`SmsService`类*应用\_Start\IdentityConfig.cs*文件通过进行以下更改用黄色突出显示： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. 添加以下`using`语句开头*IdentityConfig.cs*文件： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. 更新*Account/Manage.aspx*通过删除以黄色突出显示的行的文件：  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. 在`Page_Load`处理程序*Manage.aspx.cs*隐藏代码，请取消注释的代码以黄色突出显示，使其显示为行遵循： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. 中的代码隐藏*帐户*/*TwoFactorAuthenticationSignIn.aspx.cs*，更新`Page_Load`通过添加以下代码以黄色突出显示的处理程序： 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

 通过使上面的代码更改，"提供程序"下拉列表包含的身份验证选项将不重置为第一个值。 这将允许用户成功选择时进行身份验证，而不仅仅是第一个要使用的所有选项。
10. 在**解决方案资源管理器**，右键单击*Default.aspx*和选择**设为起始页**。
11. 通过测试你的应用，首先需要构建应用程序 (**Ctrl**+**Shift**+**B**)，然后运行应用程序 (**F5**) 和请选择**注册**以创建新的用户帐户或选择**登录**如果已注册的用户帐户。
12. 您 （作为用户） 具有登录后，单击用户 ID （电子邮件地址） 在导航栏中显示**管理帐户**页 (Manage.aspx)。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. 单击**添加**旁边**电话号码**上**管理帐户**页。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. 添加电话号码 （作为用户） 你想要接收 SMS 消息 （文本消息） 并单击**提交**按钮。   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
 此时，应用将使用的凭据从*Web.config*联系 Twilio。 将与用户帐户关联的移动电话发送短信 （文本消息）。 你可以通过验证 Twilio 消息已发送查看 Twilio 仪表板。
15. 几秒钟后，与用户帐户关联的手机将收到包含验证码的短信。 输入验证代码并按**提交**。  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>启用双因素身份验证的已注册用户

此时，你已为你的应用启用双因素身份验证。 若要使用双因素身份验证的用户，他们可以只需更改其设置，请使用用户界面。 

1. 为你的应用程序的用户你可以启用双因素身份验证您的特定帐户的单击用户 ID （电子邮件别名） 在导航栏中显示**管理帐户**页。然后，单击**启用**链接以启用双因素身份验证的帐户。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. 注销，然后重新登录。 如果已启用电子邮件，你可以选择短信或电子邮件双因素身份验证。 如果尚未启用电子邮件，请参阅标题为教程[创建用户注册、 电子邮件确认和密码重置使用的安全 ASP.NET Web 窗体应用](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. 将显示两个双因素身份验证页，你可以 （从 SMS 或电子邮件） 输入代码。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 单击**记住此浏览器**复选框将免除你无需使用双因素身份验证登录时使用的浏览器和设备选中的复选框的位置。 恶意用户无法访问你的设备，前提是启用双因素身份验证，并单击**记住此浏览器**将为你提供方便的一次性密码访问权限，同时还可以保留强从非受信任设备的所有访问的双因素身份验证保护。 可以在任何您经常使用的专用设备上执行此操作。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他资源

- [使用 SMS 和 ASP.NET 标识的双因素身份验证](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [指向 ASP.NET Identity 的推荐资源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [将包含成员资格、 OAuth 和 SQL 数据库的安全的 ASP.NET Web 窗体应用程序部署到 Azure 网站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web 窗体教程系列-添加 OAuth 2.0 提供程序](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web 窗体的系列教程的项目启用 SSL](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [帐户确认和密码恢复 ASP.NET 标识](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [在 Facebook 中创建应用并将应用程序连接到项目](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
