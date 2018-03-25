---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "保护应用程序使用身份验证和授权 |Microsoft 文档"
author: microsoft
description: "步骤 9 演示如何添加身份验证和授权，以保护我们 NerdDinner 的应用程序，以便用户需要注册和登录到要创建的网站..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a>保护应用程序使用身份验证和授权
====================
通过[Microsoft](https://github.com/microsoft)

[下载 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 这是个免费的步骤 9 ["NerdDinner"应用程序教程](introducing-the-nerddinner-tutorial.md)，查找步程取如何生成一个较小，但完成，使用 ASP.NET MVC 1 web 应用程序。
> 
> 步骤 9 演示如何添加身份验证和授权，以保护我们 NerdDinner 的应用程序，以便用户需要注册和登录到站点创建新晚餐，而仅承载 dinner 的用户可在以后编辑它。
> 
> 如果你使用的 ASP.NET MVC 3，我们建议你遵循[获取启动使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC 音乐商店](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教程。


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner 步骤 9： 身份验证和授权

现在我们 NerdDinner 应用程序授予任何人访问站点能够创建和编辑任何 dinner 的详细信息。 将此项更改，以便用户需要注册和登录到要创建新晚餐和添加的限制，以便只有承载 dinner 的用户可以更高版本编辑的站点。

要启用此功能我们将使用身份验证和授权保护我们的应用程序。

### <a name="understanding-authentication-and-authorization"></a>了解身份验证和授权

*身份验证*是标识和验证客户端访问应用程序的标识的过程。 更简单地说，它是有关标识""最终用户是谁在访问网站时。 ASP.NET 支持通过多种方式浏览器用户进行身份验证。 对于 Internet web 应用程序，使用的最常见的身份验证方法被称为"窗体身份验证"。 窗体身份验证使开发人员可以创作自己的应用程序中的 HTML 登录窗体，然后验证用户名/密码的最终用户提交针对数据库或其他密码凭据存储区。 如果用户名/密码组合均正确无误，开发人员然后可以要求 ASP.NET 颁发的加密的 HTTP cookie，用于标识用户跨今后的请求。 我们将使用与我们 NerdDinner 应用程序的窗体身份验证。

*授权*是确定已经过身份验证的用户是否有权限以访问特定的 URL/资源或执行某些操作的过程。 例如，我们 NerdDinner 应用程序中我们将需要授权，只有登录的用户可以访问*/晚餐/创建*URL 并创建新晚餐。 我们还需要添加授权逻辑，以便只有承载 dinner 的用户可以编辑它 – 和拒绝的所有其他用户编辑访问。

### <a name="forms-authentication-and-the-accountcontroller"></a>窗体身份验证和 AccountController 中

创建新的 ASP.NET MVC 应用程序时，ASP.NET MVC 的默认 Visual Studio 项目模板会自动启用表单身份验证。 它还会自动向项目 – 这使得真正轻松地集成在站点内的安全预建的帐户登录页的实现。

访问它的用户不进行身份验证时，默认 Site.master 母版页显示在右上角的站点的"登录"链接：

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

单击"登录"链接可转到用户*/帐户/登录*URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

尚未注册者可以这样通过单击"注册"链接 – 这会将他们到*/帐户/注册*URL，并让他们输入帐户的详细信息：

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

单击"注册"按钮将创建一个新用户在 ASP.NET 成员资格系统中，并使用 forms 身份验证的站点到用户进行身份验证。

Site.master 登录的用户时，更改右上角的页后，可以输出"欢迎使用 [username] ！" 消息和呈现"注销"链接而不是"Log On"一个。 单击"注销"链接注销用户：

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

已添加到我们的项目由 Visual Studio 创建项目时 AccountController 类中实现上述的登录、 注销和注册功能。 使用 \Views\Account 目录中的视图模板实现 AccountController 中的 UI:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

AccountController 类使用 ASP.NET 窗体身份验证系统以发出加密的身份验证 cookie 和 ASP.NET 成员资格 API 来存储和验证用户名/密码。 ASP.NET 成员资格 API 可扩展，并允许使用任何密码凭据存储区。 ASP.NET 附带存储用户名/密码在 SQL 数据库中，或在 Active Directory 中的内置成员资格提供程序实现。

我们可以将配置我们 NerdDinner 应用程序应使用通过打开位于项目的根"web.config"文件和查找的成员资格提供程序&lt;成员资格&gt;其中的部分。 添加创建项目时的默认 web.config 注册 SQL 成员资格提供程序，并将其配置为使用名为"ApplicationServices"的连接字符串指定的数据库位置。

默认值"ApplicationServices"连接字符串 (其内指定&lt;connectionStrings&gt; web.config 文件的部分) 配置为使用 SQL Express。 它指向名为"ASPNETDB SQL Express 数据库。MDF"该应用程序下的"应用\_数据"目录。 如果此数据库不存在首次应用程序中使用成员资格 API，ASP.NET 将自动创建数据库，以及设置相应的成员关系数据库架构中它：

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

如果而不是使用 SQL Express 我们想要使用完整的 SQL Server 实例 （或连接到远程数据库），我们将需要待办事项是以更新 web.config 文件中的"ApplicationServices"连接字符串，请确保相应的成员关系架构具有已添加到它所指向的数据库。 你可以运行"aspnet\_regsql.exe"实用工具 \Windows\Microsoft.NET\Framework\v2.0.50727\ 目录将用于成员身份和其他 ASP.NET 应用程序服务的相应架构添加到数据库中。

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>授权使用 [Authorize] 筛选器晚餐/创建 URL

我们无需编写任何代码来启用安全的身份验证和帐户管理实现 NerdDinner 应用程序。 用户可以注册新帐户，使用我们的应用程序和登录/注销的站点。

现在我们可以将授权逻辑添加到该应用程序，并使用身份验证状态和用户名的访问者来控制他们可以并不能执行的站点内。 让我们首先将授权逻辑添加到我们 DinnersController 类的"创建"操作方法。 具体而言，我们将要求用户访问*/晚餐/创建*必须登录 URL。 如果在用户未登录我们会将它们重定向到登录页，以便它们可以在登录。

实现此逻辑是这么简单。 我们只需待办事项是将 [Authorize] 筛选器属性添加到我们创建的操作方法如下所示：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC 支持的功能"操作，创建筛选器"可以用来实现能以声明方式应用于操作方法的可重用逻辑。 [Authorize] 筛选器是一个内置操作筛选器提供的 ASP.NET MVC，使开发人员能够以声明方式将授权规则应用于操作方法和控制器类。

应用不带任何参数 （如上所述） 时的 [Authorize] 筛选器会强制执行，发出操作方法请求的用户都必须登录 – 和它将自动将浏览器重定向到登录 URL 如果它们不是。 进行此重定向最初请求的 URL 传递作为查询字符串自变量时 (例如: / 帐户/登录？ReturnUrl = %2fDinners %2fcreate)。 AccountController 将然后将用户重定向回最初请求的 URL 后它们登录。

[Authorize] 筛选器可以选择支持的功能，可以指定可用于要求，用户都记录在中，在允许的用户或允许的安全角色的成员的列表的"用户"或"角色"属性。 例如，下面的代码仅允许两个特定用户、"scottgu"和"说比尔 · 盖茨"中，若要访问晚餐/创建 URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

嵌入在代码中的特定用户名称往往是非常并且可维护未通过。 更好的方法是定义较高级别的"角色"，代码会检查对，然后将用户映射到使用数据库或 active directory 系统 （启用要从代码外部存储的实际用户映射列表） 的角色。 ASP.NET 包括内置角色管理 API，以及角色提供程序 （包括针对 SQL 和 Active Directory），可帮助执行此用户/角色映射一内置组。 然后，我们无法更新代码以仅允许特定的"admin"角色中的用户访问晚餐/创建 URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>在创建时使用 User.Identity.Name 属性晚餐

我们可以检索使用控制器基类上公开的 User.Identity.Name 属性的请求当前登录的用户的用户名。

前面我们实现了我们的 create （） 的操作方法的 HTTP POST 版本当我们具有硬编码为静态字符串 Dinner"HostedBy"属性。 我们可以现在更新此代码以改用 User.Identity.Name 属性，以及自动创建 Dinner 将主机添加的回复：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

因为我们已添加到 create （） 方法的 [Authorize] 属性，则可以确保 ASP.NET MVC，操作方法才会执行站点上登录用户访问晚餐/创建 URL。 在这种情况下，User.Identity.Name 属性值将始终包含一个有效的用户名。

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>在编辑时使用 User.Identity.Name 属性晚餐

让我们现在将添加一些授权逻辑，可对用户的限制，以便它们只能编辑的晚餐它们本身托管的属性。

若要帮助实现这一点，我们将首先到我们 Dinner 对象 （在我们之前生成的 Dinner.cs 分部类） 中添加"IsHostedBy(username)"帮助器方法。 此帮助器方法返回 true 或 false，具体取决于是否提供的用户名匹配 Dinner HostedBy 属性，并封装执行不区分大小写的字符串比较其中所需的逻辑：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

然后，我们将添加 [Authorize] 属性对 edit （） 操作方法在我们 DinnersController 类中。 这将确保用户必须记录请求到*/Dinners/编辑 / [id]* URL。

然后，我们可以将代码添加到使用 Dinner.IsHostedBy(username) 帮助器方法来验证登录的用户匹配 Dinner 主机我们编辑方法。 如果用户不是主机，我们将显示"InvalidOwner"视图，并终止请求。 要执行此操作的代码类似于下面：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

我们可以右键单击 \Views\Dinners 目录，然后选择添加-&gt;查看菜单命令以创建新的"InvalidOwner"视图。 我们将填充其与以下错误消息：

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

并且，现在当用户尝试编辑它们不属于 dinner，它们将获取一条错误消息：

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

我们可以在我们的控制器来锁定，删除晚餐并确保仅 Dinner 主机可以删除它的权限内的 delete （） 操作方法重复相同的步骤。

### <a name="showinghiding-edit-and-delete-links"></a>显示/隐藏编辑和删除链接

我们要从我们的详细信息 URL 链接到我们 DinnersController 类的编辑和删除操作方法：

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

目前我们展示，无论到详细信息的 URL 访问者是否 dinner 主机的编辑和删除操作链接。 将此项更改，以便仅显示链接，如果正在访问的用户是 dinner 的所有者。

在我们 DinnersController Details() 操作方法检索 Dinner 对象，然后将其作为模型对象中传递到我们的视图模板：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

我们可以更新我们的视图模板，以便有条件地显示/隐藏的编辑和删除链接使用 Dinner.IsHostedBy() 帮助器方法与下面类似：

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>后续步骤

让我们现在看一下，我们可以如何启用到晚餐使用 AJAX 的回复的经过身份验证的用户。

>[!div class="step-by-step"]
[上一页](implement-efficient-data-paging.md)
[下一页](use-ajax-to-deliver-dynamic-updates.md)
