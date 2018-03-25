---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: "与用户进行身份验证 Forms 身份验证 (C#) |Microsoft 文档"
author: microsoft
description: "了解如何使用 [Authorize] 属性用密码保护 MVC 应用程序中的特定页。 了解如何使用网站管理太..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 17bcf02e1351587d64b72ee2b40393e0f748f23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-forms-authentication-c"></a>使用 Forms 身份验证 (C#) 的用户进行身份验证
====================
通过[Microsoft](https://github.com/microsoft)

> 了解如何使用 [Authorize] 属性用密码保护 MVC 应用程序中的特定页。 了解如何使用网站管理工具来创建和管理用户和角色。 你还了解了如何配置用户帐户和角色信息的存储位置。


本教程旨在说明如何使用窗体身份验证保护 ASP.NET MVC 应用程序中的视图。 了解如何使用网站管理工具来创建用户和角色。 你还了解了如何防止未经授权的用户调用控制器操作。 最后，你将了解如何配置存储用户名和密码。

#### <a name="using-the-web-site-administration-tool"></a>使用网站管理工具

我们执行任何其他操作之前，我们应首先创建一些用户和角色。 创建新用户和角色的最简单方法是利用 Visual Studio 2008 网站管理工具。 你可以通过选择菜单选项来启动此工具**项目中，ASP.NET 配置**。 或者，你可以通过单击命中将显示在解决方案资源管理器窗口顶部世界 hammer （某种程度上可怕） 图标来启动网站管理工具 （请参见图 1）。

**图 1 – 启动网站管理工具**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

在网站管理工具，你创建新用户和角色通过选择安全选项卡。单击**创建用户**链接创建一个名为 Stephen 的新用户 （请参见图 2）。 Stephen 用户提供所需的任何密码 (例如，*机密*)。

**图 2 – 创建新用户**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

通过第一个启用角色并定义一个或多个角色创建新的角色。 通过单击启用角色**启用角色**链接。 接下来，创建名为的角色*管理员*通过单击**创建或管理角色**链接 （请参见图 3）。

**图 3 – 创建一个新角色**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

最后，创建名为 Sally 的新用户并将 Sally 相关联具有管理员角色，通过单击创建用户链接并选择管理员创建 Sally 时 （请参见图 4）。

**图 4 – 向角色添加用户**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

当所有是说，而且完成时，你应具有名为 Stephen 和 Sally 的两个新用户。 你还应具有名为管理员的新角色。 Sally 是管理员角色的成员，并且 Stephen 不是。

#### <a name="requiring-authorization"></a>需要授权

你可以要求用户进行身份验证的用户通过将 [Authorize] 属性添加到操作调用的控制器操作之前。 你可以将 [Authorize] 特性应用于单个控制器操作或可以将此特性应用于整个控制器类。

例如，列表 1 中的控制器公开名为 CompanySecrets() 的操作。 因为此操作带有 [Authorize] 属性修饰的除非用户进行身份验证，不能调用此操作。

**列表 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

如果通过在浏览器中的地址栏中输入 URL /Home/CompanySecrets 调用 CompanySecrets() 操作并且你不是经过身份验证的用户，则你将重定向到登录视图自动 （参见图 5）。

**图 5-登录视图**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

登录视图可用于输入用户名和密码。 如果你不是已注册的用户，则可以单击**注册**链接以导航到注册查看 （请参阅图 6）。 注册视图可用于创建新的用户帐户。

**图 6-注册视图**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

成功登录后，你可以看到 CompanySecrets 查看 （请参阅图 7）。 默认情况下，你将继续登录状态，直到关闭浏览器窗口。

**图 7-CompanySecrets 视图**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>用户名或用户角色授权

[Authorize] 属性可用于限制对为特定的一组的用户或一组特定的用户角色的控制器操作的访问。 例如，列出 2 中修改后的主控制器包含名为 StephenSecrets() 和 AdministratorSecrets() 的两个新操作。

**列出 2 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

只有具有用户名 Stephen 的用户可以调用 StephenSecrets() 操作。 所有其他用户获取重定向到登录视图。 用户属性接受用户帐户名称的逗号分隔的列表。

只有管理员角色中的用户可以调用 AdministratorSecrets() 操作。 例如，因为 Sally 是 Administrators 组的成员，她可以调用 AdministratorSecrets() 操作。 所有其他用户获取重定向到登录视图。 角色属性接受以逗号分隔列表的角色的名称。

#### <a name="configuring-authentication"></a>配置身份验证

此时，你可能想知道存储用户帐户和角色信息。 默认情况下，信息存储在命名的 ASPNETDB.mdf 位于在 MVC 应用程序的应用程序中的 (RANU) SQL Express 的数据库\_数据文件夹。 此数据库是由 ASP.NET 框架时自动生成你开始使用成员资格。

若要查看 ASPNETDB.mdf 数据库在解决方案资源管理器窗口中的，首先需要选择菜单选项项目中，显示所有文件。

开发应用程序时，使用默认 SQL Express 数据库是允许的。 最有可能，但是，不会想要使用的默认 ASPNETDB.mdf 数据库对生产应用程序。 在这种情况下，你可以更改用户帐户信息通过完成以下两个步骤的存储位置：

1. 将应用程序服务数据库对象添加到你的生产数据库-更改应用程序连接字符串，以指向您的生产数据库

第一步是添加所有必要的数据库对象 （表和存储的过程） 到你的生产数据库。 将这些对象添加到新数据库的最简单方法是利用 ASP.NET SQL Server 安装向导 （请参阅图 8）。 可以通过从 Microsoft Visual Studio 2008 程序组中打开 Visual Studio 2008 命令提示并从命令提示符下执行以下命令来启动此工具：

aspnet\_regsql

**图 8-ASP.NET SQL Server 安装向导**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

ASP.NET SQL Server 安装向导，可选择你的网络上的 SQL Server 数据库并安装所有所需的 ASP.NET 应用程序服务数据库对象。 数据库服务器不需要位于要在本地计算机上。

> [!NOTE] 
> 
> 如果你不想使用 ASP.NET SQL Server 安装向导，然后你可以找到 SQL 脚本的以下文件夹中添加应用程序服务数据库对象：
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


创建必要的数据库对象后，你需要修改 MVC 应用程序使用的数据库连接。 修改你的 web 配置 (web.config) 文件中的 ApplicationServices 连接字符串，使它指向生产数据库。 例如，列出 3 中修改后的连接指向名为的 MyProductionDB （原始 ApplicationServices 连接字符串已被注释掉） 的数据库。

**列出 3 – Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>配置数据库权限

如果你使用集成安全性连接到你的数据库将需要将正确的 Windows 用户帐户作为登录名添加到你的数据库。 正确的帐户取决于作为 web 服务器使用的 Internet 信息服务的 ASP.NET 开发服务器。 正确的用户帐户还取决于你的操作系统。

如果正在使用 ASP.NET 开发服务器 （由 Visual Studio 使用的默认 web 服务器） 你的应用程序将在您的 Windows 用户帐户的上下文中执行。 在这种情况下，你需要将您的 Windows 用户帐户添加为数据库服务器登录名。

或者，如果你使用 Internet 信息服务然后你需要添加 ASPNET 帐户或 NT 机构/网络服务帐户作为数据库服务器登录名。 如果你正在使用 Windows XP，请将 ASPNET 帐户作为登录名添加到你的数据库。 如果你正在使用较新操作系统，如 Windows Vista 或 Windows Server 2008 中，请将 NT 机构/网络服务帐户添加为数据库登录名。

可以通过使用 Microsoft SQL Server Management Studio 到你的数据库中添加新的用户帐户 （请参阅图 9）。

**图 9-创建新的 Microsoft SQL Server 登录名**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

创建所需的登录名后，你需要登录名映射到具有正确的数据库角色的数据库用户。 双击该登录名并选择用户映射选项卡。选择一个或多个应用程序服务数据库角色。 例如，若要对用户进行身份验证，你需要启用 aspnet\_成员资格\_BasicAccess 数据库角色。 若要创建新用户，你需要启用 aspnet\_成员资格\_FullAccess 数据库角色 （请参阅图 10）。

**图 10 – 添加应用程序服务数据库角色**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>摘要

在本教程中，您学习了如何构建一个 ASP.NET MVC 应用程序时，使用窗体身份验证。 首先，您学习了如何通过利用网站管理工具创建新用户和角色。 接下来，您学习了如何使用 [Authorize] 属性以防止未经授权的用户调用控制器操作。 最后，您学习了如何配置你的 MVC 应用程序在生产数据库中存储用户和角色信息。

>[!div class="step-by-step"]
[下一篇](authenticating-users-with-windows-authentication-cs.md)
