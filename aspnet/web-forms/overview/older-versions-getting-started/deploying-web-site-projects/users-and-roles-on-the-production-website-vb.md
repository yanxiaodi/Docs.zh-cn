---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: "用户和生产网站 (VB) 上的角色 |Microsoft 文档"
author: rick-anderson
description: "ASP.NET 网站管理工具 (WSAT) 提供基于 web 的用户界面，用于配置成员资格和角色设置和创建，编辑，..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: f3bfaa0e14e3e04a7faae1a78b566d7c2067785a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="users-and-roles-on-the-production-website-vb"></a>用户和生产网站 (VB) 上的角色
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> ASP.NET 网站管理工具 (WSAT) 提供基于 web 的用户界面，用于配置成员资格和角色设置和用于创建、 编辑和删除用户和角色。 遗憾的是，WSAT 时才有效访问从 localhost，这意味着你无法通过你浏览器访问生产网站管理工具。 好消息是解决方法，使用户可以管理用户和生产上的角色。 本教程会查看这些解决方法和其他人。


## <a name="introduction"></a>介绍

ASP.NET 2.0 引入了大量的*应用程序服务*，这是一套可添加到 web 应用程序的构建基块服务。 我们添加后的成员资格和角色服务添加到图书评论网站[*配置网站，使用应用程序服务*教程](configuring-a-website-that-uses-application-services-vb.md)。 成员资格服务有助于创建和管理用户帐户;角色服务提供用于对用户进行分类分组的 API。 图书评论站点具有三个用户帐户-Scott、 Jisun，Alice-和单个角色，管理员，使用 Scott 和 Jisun 管理员角色中。

ASP。NET 的应用程序服务不限于特定的实现。 相反，指示要使用特定的应用程序服务*提供程序*，并该提供程序实现使用特定的技术的服务。 我们配置图书评论 web 应用程序使用`SqlMembershipProvider`和`SqlRoleProvider`提供程序的成员资格和角色服务。 这些两个提供程序用户帐户和角色信息存储在 SQL Server 数据库，并托管在 web 托管公司的基于 Internet 的 web 应用程序最常使用的提供程序。

常见的难题，对于使用成员资格和角色服务开发人员负责管理用户和角色在生产环境。 你如何从生产网站中删除用户帐户、 添加新的角色，或将现有用户添加到现有的角色？ 本教程介绍了不同的技术来管理用户和生产网站上的角色。

## <a name="using-the-aspnet-web-site-administration-tool"></a>使用 ASP.NET 网站管理工具

ASP.NET 包括[网站管理工具](https://msdn.microsoft.com/library/yy40ytx0.aspx)(WSAT)，可轻松创建和管理用户帐户和角色以及指定基于用户和角色的授权规则。 若要使用 WSAT，单击 ASP.NET 配置图标，在解决方案资源管理器，或转到网站或项目菜单上，然后选择 ASP.NET 配置选项。 这两种方法来启动 web 浏览器并指向在这样的地址 WSAT:`http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT 分为三个部分：

- **安全**-管理用户、 角色和授权规则。
- **应用程序配置**-管理&lt;appSettings&gt;和 SMTP 设置，从此处。 你可以还使应用程序脱机和管理调试和跟踪从此处，设置，以及指定默认自定义错误页。
- **ProviderConfiguration** -配置应用程序服务使用的提供程序。

安全性部分 (中所示**图 1**) 包括链接，用于创建新用户、 管理用户、 创建和管理角色，以及创建和管理访问规则。 从此处你可以向系统添加新角色、 删除现有用户，或添加或删除特定的用户帐户角色。

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**图 1**: WSAT 安全部分包含用于管理用户和角色的选项  
([单击以查看实际尺寸的图像](users-and-roles-on-the-production-website-vb/_static/image3.png))

遗憾的是，WSAT 才可访问本地。 你无法在远程生产网站; 上访问 WSAT如果你访问`www.yoursite.com/asp.netwebadminfiles/default.aspx`获得 404 找不到响应。 加强了 WSAT 使用的代码`Membership`和`Roles`类在.NET Framework 中，若要创建，编辑和删除用户和角色。 这些类，请查阅 web 应用程序的配置信息来确定要使用; 哪些提供程序返回[*配置网站，使用应用程序服务*教程](configuring-a-website-that-uses-application-services-vb.md)我们设置图书评论网站以使用`SqlMembershipProvider`和`SqlRoleProvider`提供程序。 这需要执行添加`<membership>`和`<roleManager>`部分到`Web.config`。

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

请注意，`<membership>`和`<roleManager>`部分引用`SqlMembershipProvider`和`SqlRoleProvider`中的提供程序其`type`特性，分别。 这些提供程序在指定的 SQL Server 数据库中存储的用户和角色信息。 使用这些提供程序的数据库指定通过`connectionStringName`属性，`ReviewsConnectionString`中, 定义`~/ConfigSections/databaseConnectionStrings.config`文件。 回想一下，`databaseConnectionStrings.config`在开发环境中的文件包含开发数据库的连接字符串，而`databaseConnectionStrings.config`上生产文件包含生产数据库的连接字符串。

简而言之，必须开发环境中，通过本地访问 WSAT 和配合中指定的数据库中的用户和角色信息`databaseConnectionStrings.config`文件。 因此，如果我们更改中的连接字符串信息`databaseConnectionStrings.config`文件在开发环境我们可以使用 WSAT 本地来管理用户和生产环境中的角色。

若要演示此功能，打开`databaseConnectionStrings.config`上开发环境在 Visual Studio 中的文件并将开发数据库连接字符串替换为生产数据库连接字符串。 然后启动 WSAT，请转到安全选项卡，并添加新用户密码"password ！"的名为 Sam （小于的引号）。 **图 2**显示 WSAT 屏幕时创建此帐户。

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**图 2**： 创建名为 Sam 在生产环境中的新用户  
([单击以查看实际尺寸的图像](users-and-roles-on-the-production-website-vb/_static/image6.png))

因为我们更改中的连接字符串`databaseConnectionStrings.config`指向生产数据库服务器，Sam 已添加为生产环境中的用户。 若要验证这一点，更改中的连接字符串`databaseConnectionStrings.config`回开发数据库文件，然后访问`Login.aspx`在开发环境中的页。 尝试以 Sam 登录 (请参阅**图 3**)。

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**图 3**： 不能以在开发环境中的 Sam 登录  
([单击以查看实际尺寸的图像](users-and-roles-on-the-production-website-vb/_static/image9.png))

你无法登录，Sam 作为开发环境中因为本地数据库中不存在的用户帐户信息。 相反，是已添加到生产数据库。 若要验证这一点，查看的内容`aspnet_Users`开发和生产数据库中的表。 在开发环境中应该有 Scott、 Jisun 和 Alice 的用户只有三个记录。 但是，`aspnet_Users`生产数据库中的表具有四个记录： Scott、 Jisun、 Alice 和 Sam。 因此，Sam 可以登录通过网站在生产中，但无法通过开发环境。

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**图 4**: Sam 可以登录生产网站  
([单击以查看实际尺寸的图像](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> 不要忘记将更改中的连接字符串`databaseConnectionStrings.config`回开发数据库文件的连接字符串，完成后使用的 WSAT 否则为测试通过开发站点时将使用与生产数据环境。 此外请记住，虽然我们刚刚讨论的技术使我们能够使用 WSAT 来远程管理用户和角色，则对任何其他 WSAT 配置选项 （访问规则、 SMTP 设置、 调试和跟踪设置，等等） 的更改将修改`Web.config`文件。 因此，对设置进行任何更改应用于开发环境而不适用于生产环境。


## <a name="creating-custom-user-and-role-management-web-pages"></a>创建自定义用户和角色管理 Web 页

WSAT 外框系统提供的管理用户和角色，但仅本地启动，并且需要为了管理用户和角色在生产上的更改连接字符串信息。 支持用户帐户的大多数网站还包括大量的用户和角色管理 web 页，使管理员能够管理从站点中的页面的用户和角色。 此类基于 web 的管理页使其可以更轻松地管理用户和角色以及站点必需其中可能有许多管理员或不具有访问或使用 Visual Studio 启动 WSAT 的技术背景的管理员。

ASP.NET 包括大量内置进行实现许多这些管理网页一样简单，如拖放的登录名相关的 Web 控件。 例如，你可以创建管理员将通过拖动到该页面上然后设置的几个属性创建新的用户帐户的页。 在事实中，在中所示 WSAT 中创建用户的页**图 2**使用相同的通过可添加到页面。 此外，该成员资格和角色服务的功能能够以编程方式通过`Membership`和`Roles`.NET Framework 中的类。 使用这些类可以编写代码以创建、 编辑和删除用户和角色，也可以添加或删除用户角色，以确定用户在哪些角色中，并执行其他用户和角色相关的任务。

在[*配置网站，使用应用程序服务*教程](configuring-a-website-that-uses-application-services-vb.md)我添加到页`Admin`文件夹名为`CreateAccount.aspx`。 此页面允许管理员将新的用户帐户添加到站点并指定新创建的用户是否是管理员角色中 (请参阅**图 5**)。

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**图 5**： 管理员可以创建新用户帐户  
([单击以查看实际尺寸的图像](users-and-roles-on-the-production-website-vb/_static/image15.png))

了解生成用户和角色的管理页，以及使用的分步说明的更多详细信息`Membership`和`Roles`类和登录名相关的 ASP.NET Web 控件，请务必阅读我[网站的安全教程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。 那里你将如何生成用于创建新帐户、 创建和管理角色，网页将用户分配到角色，以及其他常见的管理任务找到指导。

若要实现 WSAT 类似的功能，生产网站上始终可以生成实现 WSAT 的功能的网页的序列。 若要帮助开始，请查看位于文件夹中的 WSAT 源代码`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`。 另一个选项是使用他共享在他的文章中的 Dan Clem WSAT 备选[滚动你自己网站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)。 Dan 指导通过生成一个自定义的 WSAT 类似的工具的进程的读取器，包括其应用程序的源代码 （在 C# 中)，下载和提供将其自定义 WSAT 添加到托管网站的分步说明。

## <a name="summary"></a>摘要

ASP.NET 网站管理工具 (WSAT) 可相继使用成员资格和角色应用程序服务来管理为你的网站的用户和角色信息。 遗憾的是，WSAT 本地只能为可访问，并且不能从你的生产网站访问。 但是，通过更改连接字符串在开发环境以点到生产数据库可以使用 WSAT 来管理用户和生产网站上的角色。

虽然 WSAT 方法提供了快速轻松地管理用户和角色，它要求启动到的连接字符串信息从 Visual Studio，以及临时更改 WSAT。 WSAT 提供了一个快速方法来管理用户和角色在生产环境，但繁琐，不与多个管理员或管理员不具有或不熟悉 Visual Studio 和 WSAT 适用于网站不适用。 出于这些原因，支持用户帐户的大多数网站包括一组管理的 web 页面。 此类组的网页无需进行 WSAT 且使用的各种管理用户从任何计算机。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [正在检查 ASP。NET 的成员资格、 角色和配置文件](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [滚动你自己的网站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [网站管理工具概述](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [网站安全教程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[上一篇](precompiling-your-website-vb.md)
