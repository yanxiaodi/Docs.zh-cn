---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: "安全性基础知识和 ASP.NET 支持 (VB) |Microsoft 文档"
author: rick-anderson
description: "这是一系列将浏览进行身份验证通过 web 窗体的访问者授予对 partic 访问权限的技术的教程中的第一个教程..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: 43edf6745627e92d20c26377404e44f07e649a75
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="security-basics-and-aspnet-support-vb"></a>安全性基础知识和 ASP.NET 支持 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> 这是一系列将浏览进行身份验证通过 web 窗体的访问者、 授权访问特定页面和功能，以及管理 ASP.NET 应用程序中的用户帐户的技术的教程中的第一个教程。


## <a name="introduction"></a>介绍

什么是一件事论坛、 电子商务站点、 在线电子邮件网站、 门户网站和所有具有共同的社交网络站点？ 它们都提供*用户帐户*。 提供用户帐户的站点必须提供多个服务。 至少，新的访问者需要能够创建帐户并返回访问者必须能够登录。 此类 web 应用程序可以做出决策基于在已登录的用户： 某些页或操作可能限制为仅记录在用户或特定的用户; 子集其他页可能会显示信息特定向登录的用户，或可能显示更多或更少信息，具体取决于哪些用户正在查看的页。

这是一系列将浏览进行身份验证通过 web 窗体的访问者、 授权访问特定页面和功能，以及管理 ASP.NET 应用程序中的用户帐户的技术的教程中的第一个教程。 我们将检查这些教程之中如何：

- 确定并在用户登录到网站
- 使用 ASP.NET 的成员资格框架来管理用户帐户
- 创建、 更新和删除用户帐户
- 限制对网页、 目录或基于在已登录用户的特定功能的访问
- 使用 ASP.NET 的角色框架来将用户帐户与角色关联
- 管理用户角色
- 限制对网页、 目录或特定功能基于在已登录的用户的角色的访问
- 自定义和扩展 ASP。NET 的安全 Web 控件

这些教程旨在是简洁并提供引导你完成此过程以可视方式并提供充足的屏幕截图的分步说明。 每个教程是在 C# 和 Visual Basic 版本中可用，包括使用的完整代码下载。 （此第一个教程重点介绍从高级的角度来看的安全概念和因此不包含任何关联的代码。）

在本教程中我们将讨论重要的安全概念而 ASP.NET 以帮助实现窗体身份验证、 授权、 用户帐户和角色提供的哪些功能。 让我们进入正题！

> [!NOTE]
> 安全是跨越物理、 技术，任何应用程序的一个重要方面和策略决策，需要高度的规划和域知识。 本教程系列不用于开发安全的 web 应用程序应作为指南。 相反，它重点说明特定于窗体身份验证、 授权、 用户帐户和角色。 这一系列中讨论了旋转解决这些问题的一些安全概念，而其他处于未探测。


## <a name="authentication-authorization-user-accounts-and-roles"></a>身份验证、 授权、 用户帐户和角色

身份验证、 授权、 用户帐户和角色是将在非常频繁整个本系列教程，因此我想要花快速一些时间来定义这些条款 web 安全的上下文中的四个术语。 在客户端 / 服务器模型中，如 Internet，有很多情况下，服务器需要标识客户端发出请求。 *身份验证*是有助于确定客户端的标识的过程。 已成功发现的客户端被认为是*身份验证*。 无法识别客户端被认为是*未经身份验证*或*匿名*。

安全身份验证系统涉及至少一个以下三个方面：[你知道的东西，你拥有的东西，或者内容您](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html)。 大多数 web 应用程序依赖于某些客户端知道，例如密码或 PIN。 用于标识用户的她的用户名和密码，例如-的信息被称为*凭据*。 本系列教程侧重于*窗体身份验证*，即其中用户登录到站点通过提供其凭据以网页形式的身份验证模型。 我们会所有遇到这种类型的身份验证之前。 转到任何电子商务站点。 当你准备好签出系统会要求你登录到文本框中输入你的用户名和密码，在网页上。

除了标识客户端，可能需要限制哪些资源或功能可以根据发出请求的客户端访问服务器。 *授权*是确定特定的用户是否有权访问特定资源或功能的过程。

A*用户帐户*一个存储用于保存有关特定用户的信息。 用户帐户必须至少包括唯一标识该用户，如用户的登录名和密码的信息。 此基本信息，以及用户帐户可能包括诸如： 用户的电子邮件地址;日期和时间创建帐户;日期和时间在用户上次登录;第一个和最后一个名称;电话号码;和邮寄地址。 在使用窗体身份验证时，通常类似于 Microsoft SQL Server 关系数据库中存储用户帐户信息。

支持用户帐户的 web 应用程序 （可选） 可以分组到用户*角色*。 角色是只应用于用户，并提供用于定义授权规则和页面级别功能的抽象的标签。 例如，网站可能包括使用禁止任何人但管理员可以访问一组特定的网页的授权规则的管理员角色。 此外，各种 （包括非管理员） 的所有用户都都可以访问的页可能会显示其他数据，或提供额外的功能时访问过的管理员角色中的用户。 使用角色，我们可以在角色的角色，而不是用户通过用户定义这些授权规则。

## <a name="authenticating-users-in-an-aspnet-application"></a>ASP.NET 应用程序中的用户进行身份验证

当用户链接到其浏览器的地址窗口或单击输入 URL 时，该浏览器建立[超文本传输协议 (HTTP)](http://en.wikipedia.org/wiki/HTTP)请求到指定的内容 web 服务器，它是 ASP.NET 页上，一个映像，JavaScript文件或任何其他类型的内容。 Web 服务器担负返回请求的内容。 在此情况下，它必须确定大量请求，包括谁做了请求，并且是否标识有权检索请求的内容有关的操作。

默认情况下，浏览器发送 HTTP 请求中缺少任何种类的标识信息。 但如果浏览器包括身份验证信息，然后 web 服务器启动身份验证工作流，它会尝试标识客户端发出请求。 身份验证工作流的步骤取决于 web 应用程序正在使用的身份验证类型。 ASP.NET 支持的身份验证的三种类型： Windows、 Passport 和窗体。 本系列教程侧重于窗体身份验证，但让我们花一些时间进行比较和对比 Windows 身份验证用户存储和工作流。

### <a name="authentication-via-windows-authentication"></a>通过 Windows 身份验证的身份验证

Windows 身份验证工作流使用以下身份验证方法之一：

- 基本身份验证
- 摘要式身份验证
- Windows 集成身份验证

所有这三种技术大致相同的方式工作： 当未经授权匿名请求到达时，web 服务器发送回 HTTP 响应，该值指示该授权才能继续。 浏览器然后显示一个模式对话框，提示用户输入其用户名和密码 （请参见图 1）。 然后，此信息将发送回 web 服务器通过 HTTP 标头。


![模式对话框提示用户提供其凭据](security-basics-and-asp-net-support-vb/_static/image1.png)

**图 1**： 模式对话框提示用户提供其凭据


提供的凭据进行验证对 web 服务器的 Windows 用户存储区。 这意味着 web 应用程序中的每个经过身份验证的用户，必须在你的组织中的 Windows 帐户。 这是司空见惯内部网应用场景中。 事实上，在 intranet 设置中使用 Windows 集成身份验证，浏览器自动向 web 服务器提供的凭据用于登录到网络时，从而阻止图 1 所示的对话框。 尽管 Windows 身份验证非常适合 intranet 应用程序，它通常可行 Internet 应用程序是由于您不希望创建注册您的站点上的每个用户的 Windows 帐户。

### <a name="authentication-via-forms-authentication"></a>通过窗体身份验证的身份验证

窗体身份验证，另一方面，非常适合于 Internet web 应用程序。 回想一下，窗体身份验证标识的用户通过提示他们输入其凭据通过 web 窗体。 因此，当用户尝试访问未经授权的资源，则会自动重定向到他们可以在其中输入其凭据的登录页。 针对自定义用户存储-通常数据库然后验证的已提交的凭据。

验证已提交的凭据后,*窗体身份验证票证*为用户创建。 此票证指示用户已经过身份验证，并包含标识信息，例如用户名。 窗体身份验证票证 （通常） 将存储为客户端计算机上的 cookie。 因此，后续访问网站包括在 HTTP 请求中，从而支持的 web 应用程序后用户登录标识用户的窗体身份验证票证。

图 2 演示了高级的观察点从窗体身份验证工作流。 请注意如何在 ASP.NET 中的身份验证和授权部分充当两个单独的实体。 窗体身份验证系统标识用户 （或报告它们是匿名）。 授权系统是什么确定用户是否有权访问请求的资源。 如果用户未获授权的 （因为它们是在图 2 中尝试以匿名方式访问 ProtectedPage.aspx 时），授权系统报告用户已被拒绝，这将导致窗体身份验证系统，自动将用户重定向到登录页。

用户已成功登录后，后续的 HTTP 请求包含窗体身份验证票证。 窗体身份验证系统只标识的用户-它是确定用户是否可以访问所请求的资源的授权系统。


![窗体身份验证工作流](security-basics-and-asp-net-support-vb/_static/image2.png)

**图 2**： 窗体身份验证工作流


我们将深入探讨在随后两个教程中，窗体大得多的详细信息中的身份验证[概述窗体身份验证的](an-overview-of-forms-authentication-vb.md)和[窗体身份验证配置和高级主题](forms-authentication-configuration-and-advanced-topics-vb.md)。 ASP 的详细信息。NET 的身份验证选项，请参阅[ASP.NET 身份验证](https://msdn.microsoft.com/library/eeyk640h.aspx)。

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>限制对 Web 页、 目录和页面功能的访问

ASP.NET 包括两种方法可以确定特定的用户是否有权访问的特定文件或目录：

- **文件授权**-由于如驻留在 web 服务器的文件系统，对这些文件的访问的文件可通过访问控制列表 (Acl) 指定实现 ASP.NET 页和 web 服务。 由于 Acl 是适用于 Windows 帐户的权限，文件授权最常用于使用 Windows 身份验证。 时使用窗体身份验证，所有的操作系统和文件系统级请求执行的相同的 Windows 帐户，而不考虑用户访问网站。
- **URL 授权**-使用 URL 授权页开发人员所指定的 Web.config 中的授权规则。这些授权规则中指定哪些用户或角色允许访问或拒绝访问某些页或应用程序中的目录。

文件授权和 URL 授权特定目录中定义用于访问特定的 ASP.NET 页或所有 ASP.NET 页面的授权规则。 使用这些技术，我们可以指示 ASP.NET 拒绝对特定用户的特定页的请求或允许访问一组用户并授予其他人拒绝访问。 有关方案，其中的所有用户可以访问的页面，但该页面的功能依赖于用户的新增功能？ 例如，支持用户帐户的多个站点具有显示不同的内容或与匿名用户的经过身份验证用户的数据的页。 匿名用户可能会看到一个链接，用于登录到站点，而身份验证的用户改为将看到一条消息，如、 欢迎回来，*用户名*以及将其注销的链接。另一个示例： 查看拍卖站点处的项时你会看到不同的信息，具体取决于您是 bidder 还是 auctioning 项的一个。

以声明方式或以编程方式，可以实现此类页级调整。 若要显示不同的内容，为匿名身份验证的用户，只需拖比[LoginView 控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)拖动到页面并输入到其 AnonymousTemplate 和 LoggedInTemplate 模板以及合适的内容。 或者，你可以以编程方式确定是否当前的请求进行身份验证、 用户身份，以及哪些角色它们属于 （如果有）。 此信息可用于然后显示或隐藏在网格或面板页上的列。

这一系列包含三个专注于授权的教程。 ***基于用户的授权***检查如何限制特定用户帐户; 某页或在目录中的页面的访问***角色基于授权***考察级别; 最后，提供在角色的授权规则***显示内容根据当前记录中的用户***教程探讨修改特定页的内容和根据用户访问的页面的功能。 ASP 的详细信息。NET 的授权选项，请参阅[ASP.NET 授权](https://msdn.microsoft.com/library/wce3kxhd.aspx)。

## <a name="user-accounts-and-roles"></a>用户帐户和角色

ASP。NET 的窗体身份验证提供了基础结构以用户可以登录到站点并记住跨网页访问其经过身份验证的状态。 和 URL 授权提供一个框架，用于限制对特定文件或 ASP.NET 应用程序中的文件夹的访问。 两者，但是，提供了存储用户帐户信息或管理角色的一种方法。

在 ASP.NET 2.0 中之前, 开发人员负责创建其自己的用户和角色存储。 它们也是在设计用户界面和写入如登录页和页后，可以创建新的帐户，以及其他与帐户相关的页的基本用户的代码的挂钩。 而无需在 ASP.NET 中，实现的用户帐户必须在其自己的设计决策到达问题，例如，每个开发人员的任何内置用户帐户框架如何存储密码或其他敏感信息？和哪些准则我应施加有关密码长度和强度？

现在，在 ASP.NET 应用程序中实现用户帐户是要简单得多，谢谢到*成员资格 framework*和内置登录 Web 控件。 成员资格框架是中的类少量[System.Web.Security 命名空间](https://msdn.microsoft.com/library/system.web.security.aspx)执行基本的用户帐户相关的任务提供功能。 成员资格 framework 中的键类是[成员资格类](https://msdn.microsoft.com/library/system.web.security.membership.aspx)，它具有等方法：

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- 验证用户

成员资格框架使用[提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，这能完全分隔从其实现的成员资格框架的 API。 这使开发人员可以使用通用 API，但使它们以使用符合其应用程序的自定义需要实现。 简单地说，成员资格类定义框架 （方法、 属性和事件） 的基本功能，但不会实际提供任何实现详细信息。 相反，成员身份类的方法调用配置的提供程序，这是什么执行的实际工作。 例如，当调用的成员资格类 CreateUser 方法时，成员资格类不知道用户存储的详细信息。 它不知道用户数据库或 XML 文件或某些其他存储受到维护。 成员资格类将检查 web 应用程序的配置，以确定哪些提供商联系，以委托的调用，并且该提供程序类负责实际上在相应的用户存储区中创建新的用户帐户。 这种交互图 3 所示。

Microsoft.NET Framework 中附带了两个成员资格提供程序类：

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -实现在 Active Directory 和 Active Directory 应用程序模式 (ADAM) 服务器中的成员资格 API。
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -实现 SQL Server 数据库中的成员资格 API。

本系列教程着重 SqlMembershipProvider 上以独占方式。


[![提供程序模型使不同的实现要无缝地插入到的 Framework](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**图 03**: 提供程序模型使不同的实现要无缝地插入到的 Framework ([单击以查看实际尺寸的图像](security-basics-and-asp-net-support-vb/_static/image5.png))


提供程序模型的好处是可以由 Microsoft、 第三方供应商或各个开发人员开发和无缝地插入到成员资格 framework 备用实现。 例如，Microsoft 发布了[的成员资格提供程序的 Microsoft Access 数据库](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi)。 有关成员资格提供程序的详细信息，请参阅[提供程序工具包](https://msdn.microsoft.com/asp.net/aa336558.aspx)，其中包括成员资格提供程序、 示例自定义提供程序、 100 多个页上提供程序模型中，文档的演练和完整的内置成员资格提供程序 （即 ActiveDirectoryMembershipProvider 和 SqlMembershipProvider） 的源代码。

ASP.NET 2.0 还引入了角色 framework。 成员资格 framework 中，如角色 framework 是在提供程序模型之上构建的。 通过公开其 API[角色类](https://msdn.microsoft.com/library/system.web.security.roles.aspx)和.NET Framework 附带三个提供程序类：

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -管理角色，如 Active Directory 或 ADAM 授权管理器策略存储区中的信息。
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -实现 SQL Server 数据库中的角色。
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -将基于访问者的 Windows 组的角色信息相关联。 此方法通常用于 Windows 身份验证。

本系列教程重点介绍专门 SqlRoleProvider。

由于提供程序模型包括单个进面向 API （成员资格和角色类），可以生成围绕该 API 的功能，而无需担心实现详细信息-这些由选定的页面的提供程序开发人员。 此统一的 API 允许 Microsoft 和第三方供应商，以生成与成员资格和角色框架该接口的 Web 控件。 ASP.NET 附带的许多[登录 Web 控制](https://msdn.microsoft.com/library/ms178329.aspx)用于实现常见的用户帐户的用户界面。 例如，[登录控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)提示用户输入其凭据验证，然后将其在记录通过窗体身份验证。 [LoginView 控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)提供用于显示与经过身份验证的用户的匿名用户的不同标记或基于用户的角色的不同标记的模板。 与[通过](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx)提供分步的用户界面，用于创建新的用户帐户。

实际上各种登录控件与成员资格和角色框架进行交互。 大多数登录控件可以实现无需编写一行代码。 我们将在将来的教程，包括用于扩展和自定义其功能的技术中查看更详细介绍这些控件。

## <a name="summary"></a>摘要

所有 web 应用程序支持用户帐户还都需要类似的功能，例如： 用户登录并记住网页访问; 跨状态中有其日志的能力某一网页寻求新访问者创建帐户;并向页开发人员能够指定哪些资源、 数据和功能可供哪些用户或角色。 进行身份验证和授权用户和管理用户帐户和角色的任务是非常简单，可以用感谢窗体身份验证、 URL 授权和的成员资格和角色的框架的 ASP.NET 应用程序实现。

在过程中的下一步的几个教程，我们将检查这些方面通过从零开始的工作 web 应用以分步的方式构建。 在下面的两个教程中，我们将探究详细信息中的窗体身份验证。 我们将看到在操作的窗体身份验证工作流剖析窗体身份验证票证、 讨论安全问题，并请参阅如何配置窗体身份验证系统-所有构建 web 应用程序时允许访问者登录和注销。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET 2.0 成员资格、 角色、 窗体身份验证和安全资源](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0 的安全指导原则](https://msdn.microsoft.com/library/ms998258.aspx)
- [ASP.NET 身份验证](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET 授权](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [ASP.NET 登录控件概述](https://msdn.microsoft.com/library/ms178329.aspx)
- [检查 ASP.NET 2.0 的成员资格、 角色和配置文件](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [I： 如何安全我使用成员资格和角色的站点？](https://asp.net/learn/videos/video-45.aspx) （视频）
- [成员资格简介](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN 安全开发人员中心](https://msdn.microsoft.com/security/default.aspx)
- [专业 ASP.NET 2.0 安全、 成员资格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [提供程序工具包](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已系列已由许多有用的审阅者评审本教程。 本教程中的前导审阅者包括 Alicja Maziarz、 John Suru 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](forms-authentication-configuration-and-advanced-topics-cs.md)
[下一页](an-overview-of-forms-authentication-vb.md)
