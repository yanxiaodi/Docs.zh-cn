---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: "创建安全的 ASP.NET Web 窗体应用程序通过用户注册，电子邮件确认及密码重置 (C#) |Microsoft 文档"
author: Erikre
description: "本教程演示了如何生成与用户注册，电子邮件确认和密码重置使用 ASP.NET 标识成员的 ASP.NET Web 窗体应用程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: ed39295ed1bcaa924336a1faf52049e291abeadb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>创建安全的 ASP.NET Web 窗体应用程序通过用户注册，电子邮件确认及密码重置 (C#)
====================
通过[艾力克 Reitan](https://github.com/Erikre)

> 本教程演示了如何生成与用户注册，电子邮件确认和密码重置使用 ASP.NET 标识成员资格系统的 ASP.NET Web 窗体应用程序。 本教程基于 Rick Anderson [MVC 教程](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)。


## <a name="introduction"></a>介绍

本教程将指导您完成创建 ASP.NET Web 窗体应用程序使用 Visual Studio 和 ASP.NET 4.5 来创建用户注册使用的安全 Web 窗体应用、 电子邮件确认及密码重置所需的步骤。

### <a name="tutorial-tasks-and-information"></a>教程任务和信息：

- [创建 ASP.NET Web 窗体应用程序](#createWebForms)
- [挂钩 SendGrid](#SG)
- [需要电子邮件确认之前登录](#require)
- [密码恢复和重置](#reset)
- [重新发送电子邮件确认链接](#rsend)
- [故障排除应用程序](#dbg)
- [其他资源](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>创建 ASP.NET Web 窗体应用

首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以及。

> [!NOTE]
> 警告： 你必须安装[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)或更高版本以完成本教程。


1. 创建新的项目 (**文件** - &gt; **新项目**)，然后选择**ASP.NET Web 应用程序**模板和最新的.NET Framework从版本**新项目**对话框。
2. 从**新建 ASP.NET 项目**对话框中，选择**Web 窗体**模板。 保留默认的身份验证作为**单个用户帐户**。 如果你想要托管该应用程序在 Azure 中的，将**在云中托管**选中复选框。   
 然后，单击**确定**创建新项目。  
    ![新建 ASP.NET 项目对话框](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. 为项目启用安全套接字层 (SSL)。 请按照步骤中提供**为项目启用 SSL**部分[入门 Web 窗体教程系列](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)。
4. 运行应用程序，请单击**注册**链接并注册新的用户。 在这种情况下，基于电子邮件的唯一验证[[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性来确保格式正确的电子邮件地址。 将修改代码以添加电子邮件确认。 关闭浏览器窗口。
5. 在**服务器资源管理器**的 Visual Studio (**视图** - &gt; **服务器资源管理器**)，导航到**数据 Connections\DefaultConnection\Tables\AspNetUsers**，右键单击并选择**打开表定义**。 

    下图显示`AspNetUsers`表架构：

    ![AspNetUsers 表架构](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. 在**服务器资源管理器**，右键单击**AspNetUsers**表，然后选择**显示表数据**。  
  
    ![AspNetUsers 表数据](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 此时已注册的用户的电子邮件尚未被确认。
7. 单击行并选择删除要删除的用户。 你将在下一步中重新添加此电子邮件，并将一条确认消息发送到的电子邮件地址。

## <a name="email-confirmation"></a>电子邮件确认

它是一种最佳做法以确认电子邮件以验证它们不模拟其他人的新用户在注册过程 （即，它们尚未注册与其他人的电子邮件）。 假设你有论坛，你想要防止`"bob@cpandl.com"`从将注册为`"joe@contoso.com"`。 而无需电子邮件确认`"joe@contoso.com"`无法从您的应用程序获取不需要的电子邮件。 假设 Bob 意外注册为`"bib@cpandl.com"`和未注意到，他将无法使用恢复密码，因为此应用程序没有他正确的电子邮件。 电子邮件确认从机器人提供有限的保护，并从确定垃圾邮件发送者不对提供保护。

你通常想要发布到你的网站的任何数据之前它们已确认通过任一电子邮件、 短信或另一种机制阻止新用户。 在下面部分中，我们将启用电子邮件确认并修改代码以防止新注册的用户登录，直到已确认其电子邮件。 你将在本教程中使用的电子邮件服务 SendGrid。

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>挂钩 SendGrid

虽然本教程只显示如何添加通过电子邮件通知[SendGrid](http://sendgrid.com/)，你可以发送电子邮件使用 SMTP 和其他机制 (请参阅[其他资源](#addRes))。

1. 在 Visual Studio 中，打开**程序包管理器控制台**(**工具** - &gt; **NuGet 包管理器** - &gt;**程序包管理器控制台**)，并输入以下命令：  
    `Install-Package SendGrid`
2. 转到[Azure SendGrid 注册页](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)和注册获取免费的 SendGrid 帐户。 你可以还注册一个免费的 SendGrid 帐户直接基于[SendGrid 的站点](http://www.sendgrid.com)。
3. 从**解决方案资源管理器**打开*IdentityConfig.cs*文件中*应用\_启动*文件夹并添加以下代码以的黄色突出显示`EmailService`用于配置类**SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. 此外，添加以下`using`语句开头*IdentityConfig.cs*文件： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. 为了简化此示例，将存储中的电子邮件服务帐户值`appSettings`部分*web.config*文件。 添加以下 XML 以到黄色突出显示*web.config*在你的项目的根目录的文件：

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > 安全-永远不会存储在源代码中敏感数据。 在此示例中，帐户和凭据存储在**appSetting**部分*Web.config*文件。 在 Azure 上，你可以安全地存储这些值在**[配置](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**在 Azure 门户中的选项卡。 有关相关信息请参阅标题为 Rick Anderson 主题[用于密码和其他敏感数据部署到 ASP.NET 和 Azure 的最佳做法](https://go.microsoft.com/fwlink/?LinkId=513141)。
6. 添加电子邮件服务值以反映你的 SendGrid 身份验证值 （用户名和密码），以便你可以成功从你的应用程序发送电子邮件。 请务必使用你的 SendGrid 帐户名称，而不是你提供 SendGrid 电子邮件地址。

### <a name="enable-email-confirmation"></a>启用电子邮件确认

 若要启用电子邮件确认，将修改使用以下步骤的注册代码。  
 

1. 在*帐户*文件夹中，打开*Register.aspx.cs*代码隐藏和更新`CreateUser_Click`方法，以使以下突出显示的更改： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. 在**解决方案资源管理器**，右键单击*Default.aspx*和选择**设为起始页**。
3. 运行应用程序通过按**F5。** 将显示的页后，单击**注册**链接以显示注册页。
4. 输入你的电子邮件和密码，然后单击**注册**按钮以发送电子邮件通过 SendGrid。  
 你的项目和代码的当前状态将允许用户登录后它们填写注册表单中，即使它们尚未确认其帐户。
5. 检查你的电子邮件帐户，然后单击链接以确认你的电子邮件。  
 一旦提交注册表单，你就可登录。  
    ![示例网站的登录](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>需要电子邮件确认之前登录

尽管你确认电子邮件帐户，此时你不需要单击要完全登录的验证电子邮件中包含的链接。 在以下部分中，将修改要求新用户能够确认电子邮件之前将它们记录在, （通过身份验证） 的代码。

1. 在**解决方案资源管理器**的 Visual Studio 中，更新`CreateUser_Click`中的事件*Register.aspx.cs*隐藏代码中包含*帐户*文件夹以下突出显示的更改： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. 更新`LogIn`中的方法*Login.aspx.cs*代码隐藏与以下突出显示的更改： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>运行应用程序

 现在，你已实现的代码以检查是否已确认用户的电子邮件地址，你可以检查两侧的功能**注册**和**登录**页。 

1. 删除中的任何帐户**AspNetUsers**表包含你要测试的电子邮件别名。
2. 运行应用程序 (**F5**) 并验证你无法注册为用户，直到您已确认你的电子邮件地址。
3. 在确认新帐户通过刚被发送的电子邮件之前, 在尝试的新帐户登录。  
 你将看到你不能登录，并且你必须确认电子邮件帐户。
4. 一旦确认你的电子邮件地址，请登录到应用程序。

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>密码恢复和重置

1. 在 Visual Studio 中，删除注释字符从`Forgot`中的方法*Forgot.aspx.cs*隐藏代码中包含*帐户*文件夹，因此，该方法显示为如下所示： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. 打开*Login.aspx*页。 替换结尾处的标记**loginForm**作为下面突出显示部分： 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. 打开*Login.aspx.cs*代码隐藏和取消注释以下行以从黄色突出显示的代码`Page_Load`事件处理程序： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. 运行应用程序通过按**F5。** 将显示的页后，单击**登录**链接。
5. 单击**忘记了密码？**链接以显示**忘记了密码**页。
6. 输入你的电子邮件地址，然后单击**提交**按钮将发送一封电子邮件给你的地址将允许你重置密码。   
 检查你的电子邮件帐户，然后单击链接以显示**重置密码**页。
7. 上**重置密码**页上，输入你的电子邮件、 密码和确认的密码。 然后，按**重置**按钮。  
 当你已成功重置你的密码，**更改密码**，将显示页。 现在，你可以登录你的新密码。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>重新发送电子邮件确认链接

一旦用户创建新的本地帐户，它们发送需要它们来使用它们可以登录之前的确认链接。 如果用户意外删除确认电子邮件，或电子邮件永远不会到达时，他们将需要再次发送的确认链接。 以下代码更改显示如何启用此功能。

1. 在 Visual Studio 中，打开**Login.aspx.cs**隐藏代码并添加以下事件处理程序后的`LogIn`事件处理程序：   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. 修改`LogIn`中的事件处理程序*Login.aspx.cs*代码隐藏通过更改，如下所示以黄色突出显示的代码： 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. 更新*Login.aspx*通过添加，如下所示以黄色突出显示的代码页： 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. 删除中的任何帐户**AspNetUsers**表包含你要测试的电子邮件别名。
5. 运行应用程序 (**F5**) 并注册你的电子邮件地址。
6. 在确认新帐户通过刚被发送的电子邮件之前, 在尝试的新帐户登录。  
 你将看到你不能登录，并且你必须确认电子邮件帐户。 此外，现在可以重新一条确认消息发送到你的电子邮件帐户。
7. 输入你的电子邮件地址和密码，然后按**重新发送确认**按钮。
8. 一旦确认你基于新发送的电子邮件的电子邮件地址，请登录到应用程序。

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>故障排除应用程序

如果你没有收到包含链接以验证您的凭据的电子邮件：

- 请检查垃圾邮件或垃圾邮件文件夹。
- 登录到你的 SendGrid 帐户，然后单击[电子邮件活动链接](https://sendgrid.com/logs/index)。
- 请确保使用你 SendGrid 帐户的用户名作为*Web.config*值而不是你的 SendGrid 帐户电子邮件地址。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他资源

- [指向 ASP.NET Identity 的推荐资源](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [帐户确认和密码恢复 ASP.NET 标识](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Web 窗体教程系列-添加 OAuth 2.0 提供程序](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [将包含成员资格、 OAuth 和 SQL 数据库的安全的 ASP.NET Web 窗体应用程序部署到 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web 窗体的系列教程的项目启用 SSL](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
