---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: "使用在 ASP.NET 网页中的外部站点登录页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "此文章介绍了如何登录到 ASP.NET Web 页 (Razor) 站点上使用 Facebook、 Google、 Twitter、 Yahoo 和其他站点-即，如何支持..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>使用一个 ASP.NET Web 页 (Razor) 站点外部站点中的日志记录
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何登录到 ASP.NET Web 页 (Razor) 站点上使用 Facebook、 Google、 Twitter、 Yahoo 和其他站点-即，如何支持你的站点中的 OAuth 和 OpenID。
> 
> 你将学习：
> 
> - 如何使用 WebMatrix 入门站点模板时启用从其他站点的登录名。
> 
> 这是文章中引入的 ASP.NET 功能：
> 
> - `OAuthWebSecurity`帮助器。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 2
> - WebMatrix 3

ASP.NET 网页包含对支持[OAuth](http://oauth.net/)和[OpenID](http://openid.net/)提供程序。 使用这些提供程序，你可以到你的站点使用 Facebook、 Twitter、 Microsoft 和 Google 从其现有凭据让用户登录。 例如，若要使用 Facebook 帐户登录，用户可以只选择 Facebook 图标，将它们重定向到它们在其中输入他们的用户信息的 Facebook 登录页。 它们然后可以将 Facebook 登录名与你的站点上其帐户关联。 对网页成员资格功能的相关的增强与你的网站上的单个帐户是用户可以将多个登录名 （包括社交网络站点中的登录名） 相关联。

此图显示了从登录页**入门站点**模板，用户可以在其中选择一个 Facebook、 Twitter、 Google 或 Microsoft 图标，以允许使用外部帐户登录：

![外部提供程序](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

你可以通过对中的代码的几行取消注释启用 OAuth 和 OpenID 成员身份**入门站点**模板。 您使用的方法和属性才能使用 OAuth 和 OpenID 提供程序位于`WebMatrix.Security.OAuthWebSecurity`类。 **入门站点**模板包括完整的成员身份基础结构，登录页、 成员资格数据库，与所有的代码完成您需要允许用户登录到你使用本地凭据或来自另一个站点的站点.

本部分举例说明如何让用户从登录外部站点到站点基于**入门站点**模板。 创建一个入门站点之后, 你执行此 （详细信息按照）：

- 使用 OAuth 提供程序 （Facebook、 Twitter 和 Microsoft） 的站点，外部站点上创建应用程序。 这样，你将需要以调用这些站点的登录功能的应用程序密钥。
- 对于使用 OpenID 提供程序 (Google) 的站点，不需要创建应用程序。 对于所有这些站点，必须具有一个帐户，以便在中记录并创建开发人员应用程序。

    > [!NOTE]
    > Microsoft 应用程序只接受的实时工作网站，URL，因此不能用于本地网站 URL 测试登录名。
- 编辑你的网站中的几个文件，才能指定适当的身份验证提供程序和提交登录名添加到你想要使用的站点。

本文提供有关以下任务的单独说明：

- [启用 Google 登录名](#To_enable_Google_logins)
- [启用 Facebook 登录名](#To_enable_Facebook_logins)
- [启用 Twitter 登录名](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>启用 Google 登录名

1. 创建或打开基于 WebMatrix 入门站点模板的 ASP.NET Web Pages 站点。
2. 打开 *\_AppStart.cshtml*页上，并取消注释以下代码行。 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>测试 Google 登录

1. 运行*default.cshtml*您的网站上，选择**登录**按钮。
2. 上*登录*页上，在**使用另一个服务以要求在登录**部分中，选择以下两者之一**Google**或**Yahoo**提交按钮。 此示例使用 Google 登录名。 

    网页将请求重定向到 Google 登录页。

    ![Google 登录](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. 输入现有的 Google 帐户凭据。
4. 如果 Google 询问你是否想要允许*Localhost*若要使用帐户中的信息，请单击**允许**。

    代码使用 Google 令牌进行身份验证用户，然后在网站上返回到此页。 可以使用此页将其 Google 登录名与你的网站上的现有帐户相关联的用户或用户可以在注册你将使用的外部登录名相关联的站点上的新帐户。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. 选择**关联**按钮。 浏览器返回到应用程序的主页。

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>启用 Facebook 登录名

1. 转到[Facebook 开发人员站点](https://developers.facebook.com/apps)（如果尚未登录，则日志）。
2. 选择**创建新的应用程序**按钮，然后按照提示进行命名和创建新的应用程序。
3. 部分中**选择你的应用程序将如何集成到 Facebook**，选择**网站**部分。
4. 填写**站点 URL**字段替换你的站点的 URL (例如， `http://www.example.com`)。 **域**字段是可选的; 可以使用此提供对于整个域的身份验证 (如*example.com*)。 

    > [!NOTE]
    > 如果你正在使用 URL 你本地计算机上运行站点诸如`http://localhost:12345`（其中数是本地端口号），则可以添加到此值**站点 URL**字段用于测试你的站点。 但是，任何时候，只要你的本地站点更改的端口号，你将需要更新**站点 URL**你的应用程序的字段。
5. 选择**保存更改**按钮。
6. 选择**应用**再次，选项卡，然后查看你的应用程序的起始页。
7. 复制**应用程序 ID**和**应用程序密钥**你的应用程序的值并将其粘贴到一个临时的文本文件。 网站代码中，会将这些值传递到 Facebook 提供程序。
8. 退出 Facebook 开发人员网站。

现在你进行更改的两个页面中你的网站，以便用户将能够登录到使用其 Facebook 帐户的网站。

1. 创建或打开基于 WebMatrix 入门站点模板的 ASP.NET Web Pages 站点。
2. 打开 *\_AppStart.cshtml*页上，取消注释的代码，为 Facebook OAuth 提供程序。 取消注释的代码块如下所示： 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. 复制**应用程序 ID**从 Facebook 应用程序的值作为值`appId`（引号） 内的参数。
4. 复制**应用程序密钥**值从 Facebook 应用程序作为`appSecret`参数值。
5. 保存并关闭文件。

### <a name="testing-facebook-login"></a>测试 Facebook 登录名

1. 运行站点的*default.cshtml*页上，选择**登录**按钮。
2. 上*登录*页上，在**使用另一个服务以要求在登录**部分中，选择**Facebook**图标。 

    网页将请求重定向到 Facebook 登录页。

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. 登录到 Facebook 帐户。 

    代码使用 Facebook 令牌你进行身份验证，然后返回到页你可以在其中将 Facebook 登录名使用网站的登录名相关联。 你的用户名称或电子邮件地址填充到**电子邮件**窗体上字段。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. 选择**关联**按钮。 

    浏览器返回到主页页面并登录。

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>启用 Twitter 登录名

1. 浏览到[Twitter 开发人员网站](https://dev.twitter.com/)。
2. 选择**创建应用**链接，然后登录到网站。
3. 上**创建应用程序**窗体中，填写**名称**和**说明**字段。
4. 在**网站**字段中，输入你的站点的 URL (例如， `http://www.example.com`)。 

    > [!NOTE]
    > 如果你正在测试你的本地站点 (使用如 URL `http://localhost:12345`)，Twitter 可能不接受该 URL。 但是，你可能能够使用本地环回 IP 地址 (例如`http://127.0.0.1:12345`)。 这可以简化测试本地应用程序的过程。 但是，每次你的本地网站的端口号更改，你将需要更新**网站**你的应用程序的字段。
5. 在**回调 URL**字段中，输入你希望用户在登录后到 Twitter 到返回的网站中的页的 URL。 例如，若要将用户发送到 （它将识别其登录的状态） 的入门站点主页上，输入中输入的相同 URL**网站**字段。
6. 接受条款，然后选择**创建 Twitter 应用程序**按钮。
7. 上**我的应用程序**登陆页，选择你创建的应用程序。
8. 上**详细信息**选项卡上，滚动到底部，选择**创建我访问令牌**按钮。
9. 上**详细信息**选项卡上，复制**使用者密钥**和**使用者机密**你的应用程序的值并将其粘贴到一个临时的文本文件。 网站代码中，会将这些值传递到 Twitter 提供程序。
10. 退出 Twitter 网站。

现在你进行更改的两个页面中你的网站，以便用户能够登录到使用其 Twitter 帐户的网站。

1. 创建或打开基于 WebMatrix 入门站点模板的 ASP.NET Web Pages 站点。
2. 打开 *\_AppStart.cshtml*页上，取消注释的代码，Twitter OAuth 提供程序。 取消注释的代码块如下所示： 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. 复制**使用者密钥**值形式的值的 Twitter 应用程序从`consumerKey`（引号） 内的参数。
4. 复制**使用者机密**值形式的值的 Twitter 应用程序从`consumerSecret`参数。
5. 保存并关闭文件。

### <a name="testing-twitter-login"></a>测试 Twitter 登录

1. 运行*default.cshtml*您的网站上，选择**登录**按钮。
2. 上*登录*页上，在**使用另一个服务以要求在登录**部分中，选择**Twitter**图标。 

    网页将请求重定向到 Twitter 登录页为您创建的应用程序中。

    ![oauth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. 登录到 Twitter 帐户。
4. 该代码使用 Twitter 令牌来验证用户身份，然后返回到页你可以将关联您的登录名与你网站的帐户。 您的名称或电子邮件地址填充到**电子邮件**窗体上字段。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. 选择**关联**按钮。 

    浏览器返回到主页页面并登录。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


- [自定义站点范围的行为](https://go.microsoft.com/fwlink/?LinkId=202906)
- [将安全和成员身份添加到 ASP.NET 网站页](https://go.microsoft.com/fwlink/?LinkID=202904)
