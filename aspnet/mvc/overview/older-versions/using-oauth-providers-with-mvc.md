---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: "OAuth 提供程序使用 MVC 4 |Microsoft 文档"
author: tfitzmac
description: "本教程演示了如何生成的 ASP.NET MVC 4 web 应用程序使用户能够从 Facebo 之类的外部提供程序的凭据登录..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: f0d053cecbf9a59f258470ee370852e3f112908c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="using-oauth-providers-with-mvc-4"></a>OAuth 提供程序使用 MVC 4
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示如何生成的 ASP.NET MVC 4 web 应用程序使用户能够从外部提供程序，如 Facebook、 Twitter、 Microsoft 或 Google 凭据登录，然后集成到这些访问接口中的功能的一些你web 应用程序。 为简单起见，本教程重点介绍使用从 Facebook 的凭据。
> 
> 若要在一个 ASP.NET MVC 5 web 应用程序中使用外部凭据，请参阅[创建 ASP.NET MVC 5 应用程序使用 Facebook 和 Google OAuth2 和 OpenID 登录](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
> 
> 启用这些凭据在您的网站提供一个明显的优势，因为支持数百万个用户已与这些外部提供程序的帐户。 这些用户可能更倾向于注册为您的网站，如果它们不需要创建并记住一组新的凭据。 此外，通过这些提供程序之一的用户已登录后，你可以集成社交操作从提供程序。


## <a name="what-youll-build"></a>你将生成

在本教程中有两个主要目标：

1. 使用户能够从 OAuth 提供程序的凭据登录。
2. 从提供程序检索帐户信息和将该信息与你的站点的帐户注册集成。

虽然本教程中的示例重点关注将 Facebook 用作身份验证提供程序，你可以修改代码以使用任何提供程序。 实现任何提供程序的步骤都非常类似于将在本教程中看到的步骤。 只会注意到大的差异进行对提供程序的 API 的直接调用时设置。

## <a name="prerequisites"></a>系统必备

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)或[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Or

- Microsoft Visual Studio 2010 SP1 或[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

此外，本主题假定你具有有关 ASP.NET MVC 和 Visual Studio 的基础知识。 如果你需要 ASP.NET MVC 4 的简介，请参阅[ASP.NET MVC 4 简介](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。

## <a name="create-the-project"></a>创建项目

在 Visual Studio 中，创建新 ASP.NET MVC 4 Web 应用程序，并将其命名&quot;OAuthMVC&quot;。 你可以面向.NET Framework 4.5 或 4。

![创建项目](using-oauth-providers-with-mvc/_static/image1.png)

在新建 ASP.NET MVC 4 项目窗口中，选择**Internet 应用程序**并使**Razor**作为视图引擎。

![选择 Internet 应用程序](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>启用提供程序

如果使用 Internet 应用程序模板创建 MVC 4 web 应用程序时，项目将创建具有一个名为应用程序中的 AuthConfig.cs 文件\_开始文件夹。

![AuthConfig 文件](using-oauth-providers-with-mvc/_static/image3.png)

AuthConfig 文件包含代码以注册外部身份验证提供程序的客户端。 默认情况下，此代码已被注释掉，因此未启用任何外部提供程序。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

你必须取消注释以下代码以使用外部身份验证客户端。 取消注释仅想要包括在你的站点中的提供程序。 对于本教程中，你将仅启用的 Facebook 凭据。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

请注意，在上面的示例中，此方法包含空字符串用于注册参数。 如果你尝试运行该应用程序现在，应用程序将引发参数异常，因为空字符串不被允许的参数。 若要提供有效的值，你必须注册您的网站外部提供程序，如在下一部分中所示。

## <a name="registering-with-an-external-provider"></a>使用外部提供程序注册

若要使用从外部提供程序的凭据的用户进行身份验证，你必须注册您的网站提供程序。 在注册你的站点，你将收到参数 （如密钥或 id 和密码） 以包括注册客户端时。 你必须有你想要使用的提供程序的帐户。

本教程不显示所有必须执行以便注册这些提供程序的步骤。 步骤通常并不困难。 若要成功注册你的站点，请按照这些网站上提供的说明。 若要开始使用注册您的网站，请参阅开发者网站为:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

当向 Facebook 注册你的站点，你可以提供&quot;localhost&quot;站点域和`&quot;http://localhost/&quot;`对于 URL，如下面的图像中所示。 使用 localhost 适用于大多数提供程序，但目前并不适用于 Microsoft 提供程序。 Microsoft 提供程序，必须包含有效的网站 URL。

![注册站点](using-oauth-providers-with-mvc/_static/image4.png)

在上图中，已删除的应用程序 id、 应用密钥和联系人电子邮件的值。 实际注册你的站点时，这些值将会显示。 你将想要注意的应用程序 id 和应用程序密钥的值，因为将添加到你的应用程序。

## <a name="creating-test-users"></a>创建测试用户

如果你不介意使用现有的 Facebook 帐户来测试你的站点，则可以跳过本节。

你的应用程序在 Facebook 应用程序管理页，可以轻松创建测试用户。 你可以使用这些测试帐户登录到你的站点。 通过单击创建测试用户**角色**在左侧的导航窗格中，单击链接**创建**链接。

![创建测试用户](using-oauth-providers-with-mvc/_static/image5.png)

Facebook 站点将自动创建你请求的测试帐户数。

## <a name="adding-application-id-and-secret-from-the-provider"></a>从提供程序中添加应用程序 id 和机密

现在，你已从 Facebook 接收的 id 和机密，请返回到 AuthConfig 文件并将它们添加为参数值。 下面显示的值不是实际值。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>外部凭据登录

这是你所要做，以启用你的站点中的外部凭据。 运行你的应用程序并单击右上角的登录链接。 模板自动识别你已注册为提供程序的 Facebook 和提供程序包括一个按钮。 如果你注册多个提供程序，将自动包含每个按钮。

![外部登录名](using-oauth-providers-with-mvc/_static/image6.png)

本教程不涉及如何为外部提供程序自定义按钮中的日志。 该信息，请参阅[时使用 OAuth/OpenID 自定义登录用户界面](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)。

单击 Facebook 按钮 Facebook 凭据进行登录。 当你选择其中一个外部提供程序时，你重定向到该站点并且提示由该服务进行登录。

下图显示 Facebook 登录屏幕。 它指出，使用的你的 Facebook 帐户登录到名为 oauthmvcexample 的站点。

![facebook 身份验证](using-oauth-providers-with-mvc/_static/image7.png)

登录后的 Facebook 凭据，页面通知用户该站点将有权访问的基本信息。

![请求权限](using-oauth-providers-with-mvc/_static/image8.png)

选择后**转到应用**，用户必须注册为该站点。 用户已使用 Facebook 凭据登录后下, 图显示在注册页。 用户名称是通常预先填入从提供程序的名称。

![register](using-oauth-providers-with-mvc/_static/image9.png)

单击**注册**来完成注册。 关闭浏览器。

你可以看到，已将新帐户添加到你的数据库。 在服务器资源管理器，打开**DefaultConnection**数据库，然后打开**表**文件夹。

![数据库表](using-oauth-providers-with-mvc/_static/image10.png)

右键单击**UserProfile**表，然后选择**显示表数据**。

![显示数据](using-oauth-providers-with-mvc/_static/image11.png)

你将看到你添加的新帐户。 查看中的数据**网页\_OAuthMembership**表。 你将看到与用于你刚添加的帐户的外部提供程序相关的更多数据。

如果你只想要启用外部身份验证，你已完成。 但是，你可以进一步将集成从提供程序的信息到新的用户注册过程中，如以下各节中所示。

## <a name="create-models-for-additional-user-information"></a>为其他用户信息创建模型

如前面的部分中，你注意到，你不需要检索要工作的内置帐户注册任何其他信息。 但是，最外部提供程序将传递有关用户的其他信息。 以下部分说明如何保留该信息并将其保存到数据库。 具体而言，你将保留用户的完整名称，用户的个人的 web 页的 URI 的值和一个值，该值指示是否 Facebook 已验证帐户。

你将使用[Code First 迁移](https://msdn.microsoft.com/data/jj591621)添加用于存储的其他用户信息的表。 要添加到现有数据库，表，因此你将首先需要创建当前数据库的快照。 通过创建当前数据库的快照，稍后可以创建包含新的表的迁移。 若要创建当前数据库的快照：

1. 打开**程序包管理器控制台**
2. 运行命令**启用迁移**
3. 运行命令**IgnoreChanges 添加迁移初始 –**
4. 运行命令**更新数据库**

现在，你将添加新属性。 在 Models 文件夹中，打开 AccountModels.cs 文件，并查找 RegisterExternalLoginModel 类。 RegisterExternalLoginModel 类包含来自身份验证提供程序返回的值。 添加命名 FullName 和链接，为下面突出显示的属性。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

此外在 AccountModels.cs，添加名 ExtraUserInformation 的新类。 此类表示将在数据库中创建的新表。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

在 UsersContext 类中，添加突出显示下面的代码以创建新的类的 DbSet 属性。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

现在你就可以创建新的表。 再次和此时间，请打开程序包管理器控制台：

1. 运行命令**添加迁移 AddExtraUserInformation**
2. 运行命令**更新数据库**

数据库中现在存在新的表。

## <a name="retrieve-the-additional-data"></a>检索附加的数据

有两种方法来检索其他用户数据。 第一种方法是保留传回，默认情况下，身份验证请求过程的用户数据。 第二种方式是专门调用提供程序 API 和请求的详细信息。 返回由 Facebook 自动传递个 FullName 和链接值。 通过 Facebook API 调用中检索值，该值指示是否 Facebook 已验证帐户。 首先，将 FullName 和链接，填充值，然后更高版本，你将获取已验证的值。

若要检索其他用户数据，请打开**AccountController.cs**文件中**控制器**文件夹。

此文件包含日志记录、 注册和管理帐户的逻辑。 具体而言，请注意调用的方法**ExternalLoginCallback**和**ExternalLoginConfirmation**。 在这些方法，你可以添加代码以自定义你的应用程序的外部登录名操作。 第一行**ExternalLoginCallback**方法包含：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

其他用户数据传递回**ExtraData**属性**AuthenticationResult**从返回的对象**VerifyAuthentication**方法。 Facebook 客户端包含中的以下值**ExtraData**属性：

- id
- name
- 链接
- 性别
- accesstoken

其他提供程序将 ExtraData 属性中有类似但略有不同的数据。

如果用户是您的网站的新手，您将检索某些附加的数据，并将该数据传递给确认视图。 仅当用户为您的网站的新手，运行代码的方法中的最后一个块。 将以下行：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

用下面这行：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

此更改仅包括 FullName 和链接的属性的值。

在**ExternalLoginConfirmation**方法中，修改下面的代码的突出显示部分以保存的其他用户信息。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>调整视图

在注册页中，将显示从提供程序检索其他用户数据。

在**视图**/**帐户**文件夹中，打开**ExternalLoginConfirmation.cshtml**。 用户名称的现有字段下, 为 FullName、 链接和 PictureLink 中添加字段。

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

你现差不多已做好运行该应用程序并将新用户注册保存的其他信息。 你必须具有与站点中不能已注册的帐户。 您可以使用不同的测试帐户，或删除中的行**UserProfile**和**网页\_OAuthMembership**你想要重复使用的帐户的表。 通过删除这些行，将确保该帐户再次注册。

运行应用程序并注册新的用户。 请注意，这一次确认页包含多个值。

![register](using-oauth-providers-with-mvc/_static/image12.png)

注册后，关闭浏览器。 请注意中的新值的数据库中查找**ExtraUserInformation**表。

## <a name="install-nuget-package-for-facebook-api"></a>为 Facebook API 安装 NuGet 包

Facebook 提供[API](https://developers.facebook.com/docs/reference/apis/)可以调用以执行操作。 你可以通过将定向发送 HTTP 请求，或通过使用安装 NuGet 包，它方便了发送这些请求调用 Facebook API。 使用 NuGet 包显示在此教程中，但安装 NuGet 包是不重要。 本教程演示如何使用 Facebook C# SDK 包。 有其他帮助调用 Facebook API 的 NuGet 程序包。

从**管理 NuGet 包**windows 中，选择的 Facebook C# SDK 包。

![安装包](using-oauth-providers-with-mvc/_static/image13.png)

你将使用 Facebook C# SDK 可以调用用户需要访问令牌的操作。 下一节演示如何获取访问令牌。

## <a name="retrieve-access-token"></a>检索访问令牌

验证用户的凭据后，最外部提供程序返回传递访问令牌。 此访问令牌是非常重要，因为它使您能够调用仅供经过身份验证的用户的操作。 因此，检索并将存储访问令牌时，将基本你想要提供更多的功能。

具体取决于外部提供程序，访问令牌可能有效仅将有限的时间量。 若要确保您有一个有效的访问令牌，您将检索它每次用户登录，并将其存储为一个会话值而不是将其保存到数据库。

在**ExternalLoginCallback**方法，返回还传递访问令牌**ExtraData**属性**AuthenticationResult**对象。 添加到突出显示的代码**ExternalLoginCallback**保存访问令牌中的**会话**对象。 每次用户登录时的 Facebook 帐户时，运行此代码。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

虽然此示例从 Facebook 检索访问令牌，可以通过相同的键名为从任何外部提供程序检索访问令牌&quot;accesstoken&quot;。

## <a name="logging-off"></a>注销

默认值**注销**方法记录将用户从你的应用程序，但不会记录将用户从外部提供程序。 若要登录将用户从 Facebook，并禁止令牌保持用户已注销后，可以添加以下突出显示的代码**注销**在 AccountController 中的方法。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

在中提供的值`next`参数是要使用后用户已从 Facebook 注销的 URL。 当测试本地计算机上时，你将为您的本地站点提供正确的端口号。 在生产中，你将提供一个默认页，例如 contoso.com。

## <a name="retrieve-user-information-that-requires-the-access-token"></a>检索需要访问令牌的用户信息

现在，使用存储访问令牌，并且安装的 Facebook C# SDK 包，你可以使用它们一起从 Facebook 请求的其他用户信息。 在**ExternalLoginConfirmation**方法，创建的实例**FacebookClient**类通过将访问令牌值传递。 请求的值**验证**当前经过身份验证的用户的属性。 **验证**属性指示 Facebook 是否已验证的帐户通过某些其他方式，如向移动电话发送消息。 将此值保存在数据库中。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

将再次需要删除的用户数据库中的记录或使用其他 Facebook 帐户。

运行该应用程序，并注册新的用户。 查看**ExtraUserInformation**表以查看已验证属性的值。

## <a name="conclusion"></a>结束语

你可以在本教程中，创建与 Facebook 集成以支持用户身份验证和注册数据的站点。 您学习了如何为 MVC 4 web 应用程序，以及如何自定义该默认行为设置的默认行为。

## <a name="related-topics"></a>相关主题

- [使用身份验证和 SQL 数据库中创建的 ASP.NET MVC 应用并部署到 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
