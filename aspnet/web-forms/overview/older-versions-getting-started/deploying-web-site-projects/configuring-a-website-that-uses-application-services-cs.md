---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: "配置网站，以使用应用程序服务 (C#) |Microsoft 文档"
author: rick-anderson
description: "ASP.NET 版本 2.0 引入了一系列的应用程序服务，这是.NET Framework 的一部分，并且作为构建基块的一套服务，则..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f2b8e395505c1d13b914399b8de2196f0ba230a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-website-that-uses-application-services-c"></a>配置网站，以使用应用程序服务 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip)或[下载 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> ASP.NET 版本 2.0 引入了一系列的应用程序服务，这是.NET Framework 的一部分并且用作一套可用于将丰富的功能添加到 web 应用程序的构建基块服务。 本教程探究了如何在要使用应用程序服务的生产环境中配置网站，并介绍了与管理用户帐户和角色在生产环境的常见问题。


## <a name="introduction"></a>介绍

ASP.NET 版本 2.0 引入了一系列*应用程序服务*，这是.NET Framework 的一部分，并且作为构建基块的一套服务，您可以使用将丰富的功能添加到 web 应用程序。 应用程序服务包括：

- **成员资格**-的 API，用于创建和管理用户帐户。
- **角色**-API 对用户进行分类分组的。
- **配置文件**-的 API，用于存储自定义的、 特定于用户的内容。
- **站点地图**-的用于定义在层次结构，然后可以通过导航控件，如菜单和痕迹导航，显示的窗体中的站点 s 逻辑结构的 API。
- **个性化**-的 API，用于维护自定义项首选项，最常用于[ *web 部件*](https://msdn.microsoft.com/library/e0s9t4ck.aspx)。
- **运行状况监视**-的 API，用于监视性能、 安全、 错误和运行的 web 应用程序其他系统运行状况度量值。
  

应用程序服务 Api 不限于特定的实现。 相反，指示要使用特定的应用程序服务*提供程序*，并该提供程序实现使用特定的技术的服务。 最常用的提供程序托管在 web 托管公司的基于 Internet 的 web 应用程序都是使用 SQL Server 数据库实现这些提供程序。 例如，`SqlMembershipProvider`是将用户帐户信息存储在 Microsoft SQL Server 数据库中的成员资格 api 提供程序。

部署应用程序时，使用的应用程序服务和 SQL Server 提供程序将添加一些难题。 对于初学者，必须在开发和生产数据库上正确创建应用程序服务数据库对象并将其正确初始化。 也有需要进行的重要的配置设置。

> [!NOTE]
> 应用程序服务 Api 设计使用[*提供程序模型*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，允许 API s 的实现详细信息，以在运行时提供的设计模式。 .NET Framework 附带有很多应用程序服务提供商，可以使用，如`SqlMembershipProvider`和`SqlRoleProvider`、 它们的成员身份提供程序和角色使用的 Api，SQL Server 数据库实现。 你还可以创建和插件自定义提供程序。 事实上，图书评论 web 应用程序已包含的自定义提供程序站点映射 API (`ReviewSiteMapProvider`)，它构造中的数据中的站点映射`Genres`和`Books`数据库中的表。


本教程开头看我如何扩展图书评论 web 应用程序使用的成员资格和角色 Api。 它然后指导完成部署的 web 应用程序使用 SQL Server 数据库实现，使用应用程序服务，并通过解决常见问题的管理用户帐户和角色在生产环境来结束。

## <a name="updates-to-the-book-reviews-application"></a>对簿评审应用程序的更新

过去的几教程簿查看 web 应用程序已更新从静态网站，以动态、 数据驱动的 web 应用程序的一组用于管理风格和评论的管理页面完成。 但是，当前不受保护管理本节-任何用户都知道 （或猜出） 管理页的 URL 可以 waltz 中和创建、 编辑或删除在我们站点上的评审。 若要保护的某些部分的常用方法是网站的实现用户帐户，然后使用 URL 授权规则来限制对某些用户或角色的访问。 可供下载本教程的学习图书评论 web 应用程序支持用户帐户和角色。 它具有一个定义角色名为管理员，并且只有此角色中的用户可以访问管理页。

> [!NOTE]
> 我已在图书评论 web 应用程序中创建三个用户帐户： Scott、 Jisun 和 Alice。 所有三个用户都具有相同的密码：**密码 ！** Scott 和 Jisun 是管理员角色中，Alice 不是。 站点 s 非管理页面都匿名用户仍可以访问。 也就是说，不需要登录以访问站点，除非你想要管理它，在这种情况下您必须登录为管理员角色中的用户。


簿评审应用程序 s master 页已更新以包括不同的用户界面为经过身份验证和匿名用户。 如果匿名用户访问该站点她将看到右上角的登录链接。 身份验证的用户将看到此消息，"欢迎回来，*用户名*！" 并将其注销的链接。此处 s 还登录页 (`~/Login.aspx`)，其中包含用于进行身份验证访问者提供的用户界面和逻辑的登录 Web 控件。 只有管理员才能创建新帐户。 (已用于创建和管理中的用户帐户的页`~/Admin`文件夹。)

### <a name="configuring-the-membership-and-roles-apis"></a>配置成员资格和角色 Api

图书评论 web 应用程序使用的成员资格和角色 Api 以支持用户帐户并分组为角色 （即管理员角色） 的这些用户。 `SqlMembershipProvider`和`SqlRoleProvider`使用提供程序类进行，因为我们想要在 SQL Server 数据库中存储帐户和角色的信息。

> [!NOTE]
> 本教程不是要在配置 web 应用程序以支持的成员资格和角色 Api 的详细的信息。 全面查看这些 Api 和需要采取以配置网站以使用它们的步骤，请阅读我[*网站安全教程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


若要使用应用程序服务和 SQL Server 数据库必须首先添加到数据库这些提供程序用于你在这些位置的用户帐户的数据库对象和存储的角色信息。 这些必备项的数据库对象包括各种表、 视图和存储的过程。 除非另行指定，否则，`SqlMembershipProvider`和`SqlRoleProvider`提供程序类使用名为的 SQL Server Express Edition 数据库`ASPNETDB`位于应用程序的`App_Data`文件夹; 如果此类数据库不存在，它将自动创建通过这些提供程序在运行时的必要的数据库对象。

可能实现，且通常理想，若要创建应用程序服务网站 s 应用程序特定数据的存储位置位于同一数据库中的数据库对象。 .NET Framework 附带有一个名为工具`aspnet_regsql.exe`用于指定数据库上安装的数据库对象。 我已继续进入并使用此工具将添加到这些对象`Reviews.mdf`数据库中`App_Data`文件夹 （开发数据库）。 我们将了解如何在本教程后面使用此工具，当我们将这些对象添加到生产数据库。

如果应用程序服务到数据库的数据库对象而不添加`ASPNETDB`将需要自定义`SqlMembershipProvider`和`SqlRoleProvider`提供程序类配置，以便他们使用相应的数据库。 若要自定义成员资格提供程序将添加[ *&lt;成员资格&gt;元素*](https://msdn.microsoft.com/library/1b9hw62f.aspx)内`<system.web>`主题中`Web.config`; 使用[ *&lt;roleManager&gt;元素*](https://msdn.microsoft.com/library/ms164660.aspx)配置角色提供程序。 下面的代码段取自簿评审应用程序的`Web.config`和显示的成员资格和角色 Api 的配置设置。 请注意同时注册新的提供程序-`ReviewMembership`和`ReviewRole`-使用`SqlMembershipProvider`和`SqlRoleProvider`提供程序，分别。

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

`Web.config`文件的`<authentication>`元素也已配置为支持基于窗体的身份验证。
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>限制访问管理页

ASP.NET，可以轻松地授予或拒绝对特定文件或文件夹由用户或角色通过访问其*URL 授权*功能。 (我们简要讨论中的 URL 授权*核心差异之间 IIS 和 ASP.NET Development Server*教程并显示 IIS 和 ASP.NET Development Server 将以不同方式适用于静态 URL 授权规则的应用而不是动态的内容。）因为我们想要禁止访问`~/Admin`除外管理员角色中的这些用户的文件夹，我们需要将 URL 授权规则添加到此文件夹。 具体而言，URL 授权规则需要允许管理员角色中的用户和拒绝所有其他用户。 这通过添加实现`Web.config`文件为`~/Admin`文件夹具有以下内容：

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

为 ASP.NET 的 URL 授权功能以及如何使用它来拼写出用户和角色，授权规则的详细信息请务必阅读[*基于用户的授权*](../../older-versions-security/membership/user-based-authorization-cs.md)和[*基于角色的授权*](../../older-versions-security/roles/role-based-authorization-cs.md)从教程我[*网站安全教程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。

## <a name="deploying-a-web-application-that-uses-application-services"></a>部署的 Web 应用程序使用应用程序服务

在部署时使用应用程序服务和应用程序服务信息存储在数据库中的提供程序的网站，则务必，在生产数据库上创建应用程序服务所需的数据库对象。 最初生产数据库不包含这些对象，因此第一次部署应用程序时 （或在添加应用程序服务后首次部署时），你必须采取额外的步骤来获取这些必备项的数据库对象上生产数据库。

部署使用应用程序服务，如果你想要复制到生产环境开发环境中创建的用户帐户的网站时，可能会出现另一个难题。 根据成员资格和角色的配置，有可能，即使你已成功复制到生产数据库开发环境中创建的用户帐户，这些用户将无法登录到 web 应用程序在生产环境中。 我们将看一下此问题的原因，并讨论如何防止发生。

ASP.NET 附带 nice [*网站管理工具 (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) ，可以从 Visual Studio 启动，并允许用户帐户、 角色和授权规则，以通过基于 web 的管理接口。 遗憾的是，WSAT 仅适用于本地网站，这意味着不能用来远程管理用户帐户、 角色和 web 应用程序在生产环境中的授权规则。 我们将查看不同的方式来实现从您的生产网站 WSAT 类似的行为。

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>添加的数据库对象使用 aspnet\_regsql.exe

*部署数据库*教程介绍了如何将从开发数据库的表和数据复制到生产数据库中，和肯定可以使用这些技术将复制到的应用程序服务数据库对象生产数据库。 另一个选项是`aspnet_regsql.exe`工具，它将添加或从数据库中删除应用程序服务数据库对象。

> [!NOTE]
> `aspnet_regsql.exe`工具在指定的数据库上创建数据库对象。 它不会为生产数据库从开发数据库迁移这些数据库对象中的数据。 如果您想要开发数据库中的用户帐户和角色信息复制到生产数据库使用中介绍的技术*部署数据库*教程。


让我们来看一下如何将数据库对象添加到生产数据库使用`aspnet_regsql.exe`工具。 首先，打开 Windows 资源管理器并导航到你的计算机，%WINDIR%\ 上的.NET Framework 2.0 版目录Microsoft.NET\Framework\v2.0.50727。 你应会发现那里`aspnet_regsql.exe`工具。 可以从命令行中，使用此工具，但它还包括图形用户界面;双击`aspnet_regsql.exe`文件以启动其图形的组件。

此工具将启动通过显示说明其用途的初始屏幕。 单击旁边前进到"选择安装选项"屏幕，其中图 1 所示。 从此处你可以选择添加应用程序服务数据库对象，或从数据库中删除它们。 因为我们想要将这些对象添加到生产数据库，选择"配置应用程序服务的 SQL Server"选项，然后单击下一步。


[![选择将 SQL Server 配置为应用程序服务](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**图 1**： 为应用程序服务选择到配置 SQL Server ([单击以查看实际尺寸的图像](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))


在"选择服务器和数据库"屏幕提示输入信息以连接到数据库。 输入数据库服务器、 安全凭据和按您的 web 托管公司提供给你的数据库名称，然后单击下一步。

> [!NOTE]
> 输入您的数据库服务器和凭据后可能会收到错误，当展开数据库下拉列表时。 `aspnet_regsql.exe`工具查询`sysdatabases`系统表以检索上服务器，但一些 web 托管公司锁定其数据库服务器，因此就不公开提供此信息的数据库列表。 如果收到此错误可以直接在下拉列表中键入数据库名称。


[![提供你的数据库的连接信息的工具](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**图 2**： 提供工具与您的数据库 s 连接信息 ([单击以查看实际尺寸的图像](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))


后续屏幕总结了要执行的位置，即的操作的应用程序服务数据库对象添加到指定的数据库。 单击旁边完成此操作。 几分钟后的最后一个屏幕会显示，注意数据库对象已添加的 （请参见图 3）。


[![成功 ！应用程序服务数据库对象已添加到生产数据库](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**图 3**： 成功 ！ 应用程序服务数据库对象已添加到生产数据库 ([单击以查看实际尺寸的图像](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))


若要验证的应用程序服务数据库对象已成功添加到生产数据库，请打开 SQL Server Management Studio 并连接到你的生产数据库。 如图 4 所示，你现在应该在数据库中，会看到应用程序服务数据库表`aspnet_Applications`， `aspnet_Membership`， `aspnet_Users`，依次类推。


[![确认已将数据库对象添加到生产数据库](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**图 4**： 确认已将数据库对象添加到生产数据库 ([单击以查看实际尺寸的图像](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))


你只需使用`aspnet_regsql.exe`工具时部署 web 应用程序第一次或首次使用应用程序服务启动后。 这些数据库对象是在生产数据库后它们获胜 t 需要重新添加或修改。

### <a name="copying-user-accounts-from-development-to-production"></a>从开发到生产环境的复制用户帐户

使用时`SqlMembershipProvider`和`SqlRoleProvider`提供程序类，以将应用程序服务信息存储在 SQL Server 数据库中，用户帐户和角色信息存储在数据库表，包括各种`aspnet_Users`， `aspnet_Membership`，`aspnet_Roles`，和`aspnet_UsersInRoles`，等等。 如果在开发过程中创建用户帐户在开发环境中你可以通过将相应的记录复制适用的数据库表中复制在生产环境中的这些用户帐户。 如果你使用数据库发布向导来部署应用程序服务数据库对象您可能还选择要复制的记录，可能会导致在开发也是在生产中创建的用户帐户。 但是，具体取决于你的配置设置，你可能会发现这些用户已在开发过程中创建其帐户并将其复制到生产环境都无法从生产网站登录。 提供什么？

`SqlMembershipProvider`和`SqlRoleProvider`提供程序类已设计，单个数据库可用作用户存储为多个应用程序，其中每个应用程序可以从理论上讲，都具有重叠的用户名的用户和角色具有相同的名称。 若要允许这种灵活性，数据库将维护中的应用程序列表`aspnet_Applications`表和每个用户都使用这些应用程序之一相关联。 具体而言，`aspnet_Users`表具有`ApplicationId`会绑定到中的记录每个用户的列`aspnet_Applications`表。

除了`ApplicationId`列，`aspnet_Applications`该表还包括`ApplicationName`列，提供应用程序的多用户友好名称。 当尝试使用用户帐户，如正在验证的登录页上，从用户的凭据必须告知网站`SqlMembershipProvider`类哪些应用程序以使用。 它通常执行这一点提供应用程序名称，并且此值来自中的提供程序的配置`Web.config`-专门通过`applicationName`属性。

但如果会发生什么情况`applicationName`中未指定属性`Web.config`？ 在这种情况下成员资格系统使用应用程序根路径为`applicationName`值。 如果`applicationName`未显式设置属性`Web.config`，然后，则可能会出现的开发环境和生产环境使用不同的应用程序的根，因此将无法与不同的应用程序相关联在应用程序服务的名称。 如果出现此类不匹配，然后在开发环境中创建这些用户将拥有`ApplicationId`与不匹配的值`ApplicationId`生产环境的值。 最终结果是获胜 t 这些用户能够登录。

> [!NOTE]
> 如果你发现自己在此情况下-复制到生产环境且不匹配的用户帐户与`ApplicationId`值-无法编写查询以更新这些不正确`ApplicationId`值复制到`ApplicationId`用于生产。 后更新，在开发环境创建其帐户的用户现在将能够登录到 web 应用程序在生产环境。


好消息是一个简单的步骤，可以采取用来确保两种环境使用相同`ApplicationId`-显式设置`applicationName`属性中`Web.config`所有应用程序服务提供商。 我显式设置`applicationName`属性设为"BookReviews"中`<membership>`和`<roleManager>`元素作为此代码段从`Web.config`显示。

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

有关设置的详细讨论`applicationName`属性和的基本原理，请参阅[ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s 博客文章[*始终设置应用程序名称属性配置 ASP.NET 成员资格和其他提供程序时*](https://weblogs.asp.net/scottgu/443634)。

### <a name="managing-user-accounts-in-the-production-environment"></a>在生产环境中的管理用户帐户

ASP.NET 网站管理工具 (WSAT)，可以轻松创建和管理用户帐户、 定义和应用角色和拼写出用户和基于角色的授权规则。 您可以在启动从 Visual Studio WSAT 通过转到解决方案资源管理器并单击 ASP.NET 配置图标，或通过转到网站或项目菜单并选择 ASP.NET 配置菜单项。 遗憾的是，WSAT 可以只适用于本地的网站。 因此，不能从工作站使用 WSAT 管理生产环境中的网站。

好消息是功能的所有公开由 WSAT 可用的成员资格和角色 Api; 通过以编程方式此外，许多 WSAT 屏幕使用 ASP.NET 登录相关的标准控件。 简单地说，你可以将 ASP.NET 页添加到你提供必要的管理功能的网站。

回想一下之前教程更新图书评论 web 应用程序，以包括`~/Admin`已配置为仅允许管理员角色中的用户文件夹和此文件夹。 我添加到名为该文件夹中的页`CreateAccount.aspx`该管理员可以通过创建新的用户帐户。 此页使用通过来显示用于创建新的用户帐户的用户界面和后端逻辑。 自的新增功能，定义要包括一个提示是否还应将新用户添加到管理员角色的复选框的控件 （请参见图 5）。 通过工作很少可以生成实现用户和角色管理相关的任务，否则将由 WSAT 提供一自定义组页面。

> [!NOTE]
> 有关详细信息使用的成员资格和角色 Api 以及与登录相关的 ASP.NET Web 控件中，请务必阅读我[*网站安全教程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。 有关如何通过自定义的详细信息，请参阅[*创建用户帐户*](../../older-versions-security/membership/creating-user-accounts-cs.md)和[*存储的其他用户信息*](../../older-versions-security/membership/storing-additional-user-information-cs.md)教程，或签出[ *Erich Peterson* ](http://www.erichpeterson.com/) s 文章[*自定义通过*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![管理员可以创建新用户帐户](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**图 5**： 管理员可以创建新的用户帐户 ([单击以查看实际尺寸的图像](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))


如果你需要该 WSAT 签出的完整功能[*滚动你自己网站管理工具*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)，在哪个作者 Dan Clem 逐步的过程生成一个自定义的 WSAT 类似的工具。 Dan 共享其应用程序的源代码 （在 C# 中)，并提供有关将其添加到托管网站的分步说明。

## <a name="summary"></a>摘要

部署的 web 应用程序使用的应用程序服务数据库实现时你首先必须确保生产数据库具有所需的数据库对象。 这些对象可添加使用中讨论的技术*部署数据库*教程; 或者，可以使用`aspnet_regsql.exe`工具，如我们在本教程中看到。 上以同步中 （这是重要信息： 如果你希望用户并在开发环境中创建的角色才能对生产有效） 的开发和生产环境和技术的使用的应用程序名称为中心，我们接触其他挑战管理用户和生产环境中的角色。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [*ASP.NET SQL 服务器注册工具 (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*为 SQL Server 中创建应用程序服务数据库*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*在 SQL Server 中创建成员身份架构*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*检查 ASP.NET s 成员资格、 角色和配置文件*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*滚动你自己的网站管理工具*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*网站安全教程*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*网站管理工具概述*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

>[!div class="step-by-step"]
[上一页](configuring-the-production-web-application-to-use-the-production-database-cs.md)
[下一页](strategies-for-database-development-and-deployment-cs.md)
