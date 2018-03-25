---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: "创建 MVC 5 应用程序与 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录 (C#) |Microsoft 文档"
author: Rick-Anderson
description: "本教程演示了如何生成的 ASP.NET MVC 5 web 应用程序使用户能够使用从外部身份验证的凭据使用 OAuth 2.0 登录..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8dc2221ea19a33f1d34ba3aae8c60e365423e140
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>创建 ASP.NET MVC 5 应用程序使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登录 (C#)
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> 本教程演示如何生成的 ASP.NET MVC 5 web 应用程序使用户能够登录使用[OAuth 2.0](http://oauth.net/2/)使用从外部身份验证提供程序，如 Facebook、 Twitter、 LinkedIn、 Microsoft 或 Google 凭据。 为简单起见，本教程重点介绍使用 Facebook 和 Google 凭据。
> 
> 启用这些凭据在您的网站提供一个明显的优势，因为支持数百万个用户已与这些外部提供程序的帐户。 这些用户可能更倾向于注册为您的网站，如果它们不需要创建并记住一组新的凭据。
> 
> 另请参阅[使用 SMS 和电子邮件双因素身份验证的 ASP.NET MVC 5 应用](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)。
> 
> 本教程还将说明如何添加配置文件数据的用户，以及如何使用成员资格 API 来添加角色。 本教程编写的[Rick Anderson](https://blogs.msdn.com/rickAndy) (请在 Twitter 上关注我： [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="start"></a>
## <a name="getting-started"></a>入门

首先，安装并运行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 安装 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。 有关帮助 Dropbox、 GitHub、 Linkedin、 Instagram、 缓冲区、 salesforce、 流、 堆栈 Exchange、 Tripit、 分组对抗、 Twitter、 Yahoo 和的详细信息，请参阅此[一站式指南](http://www.oauthforaspnet.com/)。

> [!NOTE]
> 你必须安装 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本以使用 Google OAuth 2 和本地调试而 SSL 警告。


单击**新项目**从**启动**页上，也可以使用菜单并选择**文件**，，然后**新项目**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>创建第一个应用程序

单击**新项目**，然后选择**Visual C#**在左侧，然后**Web** ，然后选择**ASP.NET Web 应用程序**。 将你的项目"MvcAuth"，然后单击**确定**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

在**新建 ASP.NET 项目**对话框中，单击**MVC**。 如果身份验证不**单个用户帐户**，单击**更改身份验证**按钮，然后选择**单个用户帐户**。 通过检查**在云中托管**，应用程序将会非常轻松地在 Azure 中托管。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

如果你选择**在云中托管**，完成配置对话框。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>使用 NuGet 来更新到最新的 OWIN 中间件

使用 NuGet 包管理器更新[OWIN 中间件](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)。 选择**更新**左侧菜单中。 你可以单击**更新所有**按钮或你可以搜索仅 OWIN 包 （下一步的图中所示）：

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

在下图中，将显示仅 OWIN 包：

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

从包管理器控制台 (PMC) 中，你可以输入`Update-Package`命令，将更新所有包。

按**F5**或**Ctrl + F5**运行该应用程序。 在下图中，端口号是 1234年。 运行应用程序时，你将看到不同的端口号。

根据你的浏览器窗口的大小，你可能需要单击导航图标可查看**主页**，**有关**，**联系人**，**注册**和**登录**链接。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>在项目中的 SSL 设置

若要连接到 Google 和 Facebook 等的身份验证提供程序，你将需要设置为使用 SSL 的 IIS Express。 务必要在登录后使用 SSL 来保留并不将放回到 HTTP，登录 cookie 一样机密作为你的用户名和密码，也无需使用的 SSL 要跨网络以明文形式发送它。 此外，你已取得执行握手和安全通道 （这是大部分特质 HTTPS 慢于 HTTP） 的时间运行 MVC 管道之前，因此将重定向回 HTTP 在登录后不会带来的当前请求或将来请求快得多。

1. 在**解决方案资源管理器**，单击**MvcAuth**项目。
2. 按 F4 键以显示项目属性。 或者，从**视图**可以选择的菜单**属性窗口**。
3. 更改**已启用 SSL**为 True。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. 复制 SSL URL (这将是`https://localhost:44300/`除非你已创建其他 SSL 项目)。
5. 在**解决方案资源管理器**，右键单击**MvcAuth**项目，然后选择**属性**。
6. 选择**Web**选项卡，然后再粘贴到的 SSL URL**项目 Url**框。 保存文件 (Ctl + S)。 你将需要此 URL 以配置 Facebook 和 Google 身份验证应用程序。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. 添加[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)属性设为`Home`控制器需要的所有请求必须使用 HTTPS。 更安全的方法是将添加[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)到应用程序的筛选器。 请参阅明&quot;保护应用程序通过 SSL 和授权属性&quot;中我 tutoral[使用身份验证和 SQL 数据库中创建的 ASP.NET MVC 应用并部署到 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 下面显示了主控制器的一部分。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. 按 Ctrl+F5 运行应用程序。 如果在过去，在已安装了证书，你可以跳过本部分的其余部分并跳转到[创建 oauth2 的 Google 应用和将应用程序连接到项目](#goog)，否则，请按照说明信任自签名IIS Express 生成的证书。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. 读取**安全警告**对话框，然后单击**是**如果你想要安装表示本地主机的证书。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE 会显示*主页*页上，并不会出现 SSL 警告。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome 也接受该证书，将显示 HTTPS 内容而不发出警告。 Firefox 会使用其自己的证书存储，因此它将显示一条警告。 对于我们的应用程序，您可以安全地单击**我了解风险**。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>创建 oauth2 的 Google 应用和将应用程序连接到项目

> [!WARNING]
> 当前的 Google OAuth 说明，请参阅[ASP.NET 核心中的配置 Google 身份验证](/aspnet/core/security/authentication/social/google-logins)。

1. 导航到[Google 开发人员控制台](https://console.developers.google.com/)。
1. 如果你尚未创建之前的项目，选择**凭据**在左侧选项卡，然后选择**创建**。
1. 在左侧选项卡中，单击**凭据**。
1. 单击**创建凭据**然后**OAuth 客户端 ID**。 

    1. 在**创建客户端 ID**对话框中，保留默认值**Web 应用程序**的应用程序类型。
    2. 设置**授权 JavaScript**来源为上面使用的 SSL URL (`https://localhost:44300/`除非你已创建其他 SSL 项目)
    3. 设置**已授权重定向 URI**到：  
         `https://localhost:44300/signin-google`
5. 单击 OAuth Consent 屏幕菜单项，然后设置你的电子邮件地址和产品名称。 完成窗体中单击**保存**。
6. 单击库菜单项，请搜索**Google + API**，单击它，然后按启用。
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 下图显示已启用的 Api。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. 从 Google Api API 管理器中，请访问**凭据**选项卡以获取**客户端 ID**。 若要使用应用程序机密保存 JSON 文件的下载。 复制并粘贴**ClientId**和**ClientSecret**到`UseGoogleAuthentication`在找到方法*Startup.Auth.cs*文件中*App_Start*文件夹。 **ClientId**和**ClientSecret**如下所示的值是示例，不起作用。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > 安全-永远不会存储在源代码中敏感数据。 帐户和凭据添加到上面为了让示例比较简单的代码。 请参阅[用于密码和其他敏感数据部署到 ASP.NET 和 Azure App Service 的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
8. 按**CTRL + F5**生成并运行应用程序。 单击**登录**链接。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. 下**使用另一个服务以要求在登录**，单击**Google**。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > 如果缺少任何上述步骤将收到 HTTP 401 错误。 重新检查上述步骤。 如果你缺少必需的设置 (例如**产品名称**)，添加缺少的项，并保存，可能需要几分钟的身份验证正常工作。
10. 你将重定向到 google 站点将在其中输入你的凭据。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. 输入你的凭据后，你将会提示您刚创建的 web 应用程序对其授予权限：
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. 单击**接受**。 你现在会定向回**注册**MvcAuth 应用程序可以在其中注册你的 Google 帐户页。 你可以选择更改 Gmail 帐户使用的本地电子邮件注册名称，但你通常想要保留的默认电子邮件别名 （即，一个用于身份验证）。 单击“注册”。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>在 Facebook 中创建应用并将应用程序连接到项目

> [!WARNING]
> 当前 Facebook OAuth2 身份验证说明，请参阅[配置 Facebook 身份验证](/aspnet/core/security/authentication/social/facebook-logins)

对于 Facebook OAuth2 身份验证，你需要从中进行复制到你的项目的某些设置的应用程序在 Facebook 中创建。

1. 在浏览器中，导航到[https://developers.facebook.com/apps](https://developers.facebook.com/apps) ，然后通过输入你的 Facebook 凭据登录。
2. 如果你不已注册为 Facebook 开发人员，请单击**作为开发人员注册**并按照说明注册。
3. 上**应用**选项卡上，单击**创建新的应用程序**。

    ![创建新的应用程序](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. 输入**应用名称**和**类别**，然后单击**创建应用**。

    这必须是唯一的 Facebook。 **应用 Namespace**是你的应用程序将用于访问 Facebook 应用程序进行身份验证 (例如，https://apps.facebook.com/ {应用 Namespace}) 的 URL 的一部分。 如果没有指定**应用 Namespace**、**应用程序 ID**用于 URL。 **应用程序 ID**是你将在下一步中看到一个长时间的系统生成的数字。

    ![创建新的应用程序对话框](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. 提交的标准安全检查。

    ![安全检查](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. 选择**设置**的左侧的菜单栏![Facebook 开发人员的菜单栏](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. 上**基本**页的设置部分，选择**添加平台**来指定要添加的网站应用程序。 ![基本设置](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. 选择**网站**从平台提供的选项。  
  
    ![平台选项](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. 记下你**应用程序 ID**和你**应用程序密钥**，以便你可以添加到 MVC 应用程序在此教程的后面。 此外，添加你站点的 URL (`https://localhost:44300/`) 测试 MVC 应用程序。 此外，将添加**联系人电子邮件**。 然后，选择**保存更改**。   

    ![基本应用程序详细信息页](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > 请注意，你将只能够使用已注册的电子邮件别名进行身份验证。 其他用户和测试帐户将无法注册。 你可以授予其他 Facebook 帐户对 Facebook 上的应用程序**角色为开发人员**选项卡。
10. 在 Visual Studio 中，打开*应用\_Start\Startup.Auth.cs*。
11. 复制并粘贴**AppId**和**应用程序密钥**到`UseFacebookAuthentication`方法。 **AppId**和**应用程序密钥**如下所示的值是示例，不起作用。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. 单击**保存更改**。
13. 按**CTRL + F5**运行该应用程序。


选择**登录**以显示登录页。 单击**Facebook**下**使用其他服务进行登录。**

输入你的 Facebook 凭据。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

将提示你为应用程序访问你的公共配置文件和友元列表授予权限。

![Facebook 应用程序详细信息](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

您现在登录。 您现在可以与应用程序注册此帐户。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

注册时，将条目添加到*用户*的成员资格数据库的表。

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>检查成员身份数据

在**视图**菜单上，单击**服务器资源管理器**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

展开**DefaultConnection (MvcAuth)**，展开**表**，右键单击**AspNetUsers**单击**显示表数据**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers 表数据](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>将配置文件数据添加到用户类

在本节中你将添加出生日期和主镇对用户数据在注册期间，如下图中所示。

![使用主镇和 Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

打开*Models\IdentityModels.cs*文件并添加出生日期和主页镇属性：

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

打开*Models\AccountViewModels.cs*文件和集出生日期和主页中的镇属性`ExternalLoginConfirmationViewModel`。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

打开*Controllers\AccountController.cs*文件并添加代码中的出生日期和主页镇`ExternalLoginConfirmation`操作方法如下所示：

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

添加出生日期和到主镇*Views\Account\ExternalLoginConfirmation.cshtml*文件：

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

删除成员资格数据库，使您可以再次向你的应用程序注册您的 Facebook 帐户，并验证你可以添加新的出生日期和家庭镇配置文件信息。

从**解决方案资源管理器**，单击**显示所有文件**图标，然后右键单击*添加\_Data\aspnet-MvcAuth-&lt;日期时间戳&gt;.mdf*单击**删除**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

从**工具**菜单上，单击**NuGet 包管理器**，然后单击**程序包管理器控制台**(PMC)。 在 PMC 中输入以下命令。

1. Enable-Migrations
2. 添加迁移 Init
3. Update-Database

运行应用程序并使用 FaceBook 和 Google 登录并注册某些用户。

## <a name="examine-the-membership-data"></a>检查成员身份数据

在**视图**菜单上，单击**服务器资源管理器**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

右键单击**AspNetUsers**单击**显示表数据**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown`和`BirthDate`字段如下所示。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>关闭你的应用程序日志记录和与另一个帐户中的日志记录

如果登录到使用 Facebook、 对应用程序，然后注销并尝试登录再次使用其他 Facebook 帐户 （使用的同一浏览器），你就可立即登录到你使用的以前的 Facebook 帐户。 若要使用另一个帐户，你需要导航到 Facebook 和注销在 Facebook。 相同的规则适用于任何其他第三方身份验证提供程序。 或者，你可以将另一个帐户登录通过使用不同的浏览器。

## <a name="next-steps"></a>后续步骤

请参阅[简介 owin 在 Yahoo 和 LinkedIn OAuth 安全提供程序](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)通过 Jerrie Pelser 有关 Yahoo 和 LinkedIn 说明。 请参阅 Jerrie 的美观社交登录按钮的 ASP.NET MVC 5，若要获取启用社交登录按钮。

遵循我的教程[使用身份验证和 SQL 数据库中创建的 ASP.NET MVC 应用并部署到 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)，其将继续本教程并显示以下：

1. 如何将您的应用程序部署到 Azure。
2. 如何保护与角色的应用。
3. 如何保护你的应用程序[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)和[Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)筛选器。
4. 如何使用成员资格 API 来添加用户和角色。

请在如何喜欢本教程的方式，我们可以提高上，留下反馈。 你还可以请求新主题[教我编写代码](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。 甚至可以寻求并对要添加到 ASP.NET 的新功能投票。 例如，你可以在其中投票到工具的[创建和管理用户和角色。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

ASP.NET 外部身份验证服务的工作方式很好的说明，请参阅 Robert McMurray[外部身份验证服务](https://asp.net/web-api/overview/security/external-authentication-services)。 Robert 的文章还将进入中启用 Microsoft 和 Twitter 身份验证的详细信息。 Tom Dykstra 的绝佳[EF/MVC 教程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)演示如何使用实体框架。
