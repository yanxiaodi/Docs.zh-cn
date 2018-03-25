---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: "基于用户的授权 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将考察限制对页的访问并限制通过各种技术的页级功能。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bee98878b5191a096b851c65aaea19ad989f608
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="user-based-authorization-c"></a>基于用户的授权 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip)或[下载 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> 在本教程中我们将考察限制对页的访问并限制通过各种技术的页级功能。


## <a name="introduction"></a>介绍

提供用户帐户的大多数 web 应用程序中执行此操作来限制访问站点内的某些页某些访问者的一部分。 中最 online 论坛站点，例如，所有用户的匿名和经过身份验证-都就能够查看论坛的文章，但仅经过身份验证的用户可以访问 web 页后，可以创建新的 post。 和可能存在才可供特定用户 （或一组特定的用户） 访问的管理页。 此外，基于用户的用户可以与不同页级功能。 时查看文章的列表，经过身份验证的用户将显示一个接口，使评级每次发布，而此接口不是可供匿名访客。

ASP.NET，可以轻松定义基于用户的授权规则。 只要稍加中标记与`Web.config`，特定的网页或整个目录可以锁定，以便它们仅可以访问到用户的指定子集。 页级功能可以打开或关闭基于当前登录用户通过编程和声明性方式。

在本教程中我们将考察限制对页的访问并限制通过各种技术的页级功能。 让我们进入正题！

## <a name="a-look-at-the-url-authorization-workflow"></a>URL 授权工作流查看

中所述[*概述窗体身份验证的*](../introduction/an-overview-of-forms-authentication-cs.md)教程中，当 ASP.NET 运行时处理的请求，为 ASP.NET 资源请求在其生命周期期间引发的事件数。 *HTTP 模块*是在响应特定事件在请求生命周期中执行其代码的托管的类。 ASP.NET 附带许多执行所需的任务在后台的 HTTP 模块。

一个此类 HTTP 模块是[ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)。 如前面的教程，主要功能的中所述`FormsAuthenticationModule`是确定当前请求的标识。 通过检查窗体身份验证票证，位于 cookie 或嵌入在 URL 内完成此操作。 此标识期间发生[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)。

另一个重要的 HTTP 模块是[ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)，这在响应中引发[`AuthorizeRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)(该优化发生后`AuthenticateRequest`事件)。 `UrlAuthorizationModule`检查中的配置标记`Web.config`以确定是否当前的标识有权访问指定的页。 此过程称为*URL 授权*。

我们将检查在步骤 1 中的 URL 授权规则的语法但首先让我们看一下什么`UrlAuthorizationModule`未具体取决于是否对请求进行授权或不。 如果`UrlAuthorizationModule`确定对请求进行授权，则它不执行任何操作，并请求将继续其生命周期。 但是，如果请求*不*授权，则`UrlAuthorizationModule`中止生命周期，并指示`Response`要返回对象[HTTP 401 未授权](http://www.checkupdown.com/status/E401.html)状态。 使用窗体身份验证时此 HTTP 401 状态从不返回到客户端因为如果`FormsAuthenticationModule`检测到 HTTP 401 状态是修改到[HTTP 302 重定向](http://www.checkupdown.com/status/E302.html)到登录页。

图 1 显示了 ASP.NET 管道中，工作流`FormsAuthenticationModule`，和`UrlAuthorizationModule`当未授权的请求到达时。 具体而言，图 1 显示为匿名访问者的请求`ProtectedPage.aspx`，这是对匿名用户拒绝访问的页面。 由于访问者是匿名的`UrlAuthorizationModule`中止请求并返回 HTTP 401 未授权的状态。 `FormsAuthenticationModule`然后将 401 状态转换到 302 重定向到登录页。 用户进行身份验证通过登录页后，他将重定向到`ProtectedPage.aspx`。 这一次`FormsAuthenticationModule`标识基于其身份验证票证的用户。 现在，在距访客进行身份验证，`UrlAuthorizationModule`允许对页的访问。


[![窗体身份验证和 URL 授权工作流](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**图 1**: 窗体身份验证和 URL 授权工作流 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image3.png))


图 1 所示时发生匿名访问者尝试访问不是对匿名用户可用的资源的交互。 在这种情况下，匿名访问者将重定向到她尝试访问在查询字符串中指定的页面的登录页。 用户已成功登录后，系统将自动将她重定向回她最初尝试查看的资源。

此工作流由匿名用户进行未经授权的请求时, 非常简单，便于在距访客，若要了解已发生了什么情况及其原因。 但请记住，`FormsAuthenticationModule`将重定向*任何*未经授权的用户到登录页上，即使请求发出者身份验证的用户。 如果经过身份验证的用户尝试访问其她缺少颁发机构页，这可能导致混乱的用户体验。

假设我们的网站，必须配置其 URL 授权规则以便 ASP.NET 页`OnlyTito.aspx`仅对 Tito 看见已。 现在，假定 Sam 访问该网站时，登录，并且会尝试访问`OnlyTito.aspx`。 `UrlAuthorizationModule`将暂停请求生命周期，并返回 HTTP 401 未授权状态，其中`FormsAuthenticationModule`将检测，然后将 Sam 重定向到登录页。 由于 Sam 已登录，不过，她可能想知道为什么她具有已发送回登录页。 她可能原因的某种程度上，她登录凭据已丢失或她输入凭据无效。 如果 Sam 重新输入其凭据从登录页她将记录 （再次），并在重定向到`OnlyTito.aspx`。 `UrlAuthorizationModule`将会检测到 Sam 无法访问此页面，然后她将返回到登录页。

图 2 描绘了此令人困惑的工作流。


[![默认工作流可能会导致令人困惑周期](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**图 2**: 默认工作流可能会导致混淆周期 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image6.png))


图 2 所示的工作流可以快速 befuddle 甚至大多数计算机聪明访问者。 我们将了解如何防止这种令人费解在步骤 2 中的周期。

> [!NOTE]
> ASP.NET 使用两种机制来确定当前用户是否可以访问特定的 web 页： URL 授权和文件授权。 由实现文件授权[ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx)，用于确定颁发机构咨询服务的请求的文件 Acl。 由于 Acl 是适用于 Windows 帐户的权限，文件授权最常用于使用 Windows 身份验证。 时使用窗体身份验证，所有的操作系统和文件系统级请求执行的相同的 Windows 帐户，而不考虑用户访问网站。 由于本系列教程侧重于窗体身份验证，因此，我们不会在讨论文件授权。


### <a name="the-scope-of-url-authorization"></a>URL 授权作用域

`UrlAuthorizationModule`是托管代码的是 ASP.NET 运行时的一部分。 之前的 Microsoft 的版本 7 [Internet 信息服务 (IIS)](https://www.iis.net/) web 服务器没有之间 IIS 的 HTTP 管道和 ASP.NET 运行时的管道的不同屏障。 简单地说，在 IIS 6 和更早版本，ASP。NET 的`UrlAuthorizationModule`才会执行时请求从 IIS 委派给 ASP.NET 运行时。 默认情况下，IIS 处理如 HTML 页和 CSS、 JavaScript 和图像文件的静态内容本身的并仅将传送到 ASP.NET 运行时的扩展的页的请求`.aspx`， `.asmx`，或`.ashx`请求。

IIS 7，但是，可以集成的 IIS 和 ASP.NET 的管道。 使用其他一些配置设置，你可以设置 IIS 7 以调用`UrlAuthorizationModule`为*所有*请求，这意味着，可以为任何类型的文件中定义 URL 授权规则。 此外，IIS 7 包括其自己的 URL 授权引擎。 有关 ASP.NET 集成和 IIS 7 本机 URL 授权功能的详细信息，请参阅[了解 IIS7 URL 授权](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)。 如 ASP.NET 和 IIS 7 集成更深入了解，选取一份 Shahram Khosravi 簿*Professional IIS 7 和 ASP.NET 集成编程*(ISBN: 978 0470152539)。

简而言之，在 IIS 7 之前的版本，URL 授权规则仅适用于由 ASP.NET 运行时的资源。 但与 IIS 7 可以使用 IIS 的本机 URL 授权功能或集成 ASP。NET 的`UrlAuthorizationModule`到 IIS 的 HTTP 管道，从而扩展到的所有请求此功能。

> [!NOTE]
> 有一些细微但却很重要的差异，如何 ASP。NET 的`UrlAuthorizationModule`和 IIS 7 的 URL 授权功能处理授权规则。 本教程将不检查 IIS 7 的 URL 授权功能，或者它如何分析相比的授权规则中的差异`UrlAuthorizationModule`。 有关这些主题的详细信息，请参阅 MSDN 上或在 IIS 7 文档[www.iis.net](https://www.iis.net/)。


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>步骤 1： 定义中的 URL 授权规则`Web.config`

`UrlAuthorizationModule`确定是否授予或拒绝对请求的资源的特定标识根据在应用程序的配置中定义的 URL 授权规则访问。 授权规则中将逐一[`<authorization>`元素](https://msdn.microsoft.com/library/8d82143t.aspx)形式`<allow>`和`<deny>`子元素。 每个`<allow>`和`<deny>`子元素可以指定：

- 特定用户
- 以逗号分隔的用户列表
- 所有匿名用户，由问号 （？） 表示
- 所有用户，由一个星号表示 (\*)

以下标记演示如何使用 URL 授权规则可以允许用户 Tito 和 Scott 和拒绝所有其他用户：

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>`元素定义-Tito 和 Scott-允许哪些用户时`<deny>`元素指示*所有*用户被拒绝。

> [!NOTE]
> `<allow>`和`<deny>`元素还可以指定角色的授权规则。 我们将检查将来的教程中基于角色的授权。


以下设置对 Sam （包括匿名访问者） 以外的任何人授予访问权限：

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

若要仅允许经过身份验证的用户，请使用下面的配置，对所有匿名用户拒绝访问：

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

在中定义的授权规则`<system.web>`中的元素`Web.config`和适用于所有 web 应用程序中的 ASP.NET 资源。 通常，应用程序具有用于不同节的不同的授权规则。 例如，在电子商务网站，所有访问者可能细读产品，请参阅产品评论、 搜索的目录，等等。 但是，只有经过身份验证的用户可能会达到签出或要管理的传送历史记录的页面。 此外，可能存在的站点才可访问的选择的用户，例如站点管理员的部分。

ASP.NET，可以轻松可在站点定义不同的文件和文件夹的不同的授权规则。 指定的根文件夹中的授权规则`Web.config`文件将应用到站点中的所有 ASP.NET 资源。 但是，这些授权可以被重写默认设置为特定文件夹通过添加`Web.config`与`<authorization>`部分。

让我们更新我们的网站，以便仅经过身份验证的用户可以访问中的 ASP.NET 页`Membership`文件夹。 若要完成此我们需要添加`Web.config`文件为`Membership`文件夹并设置其授权设置要拒绝匿名用户。 右键单击`Membership`文件夹在解决方案资源管理器，从上下文菜单中，选择添加新项菜单上，并添加名为的新 Web 配置文件`Web.config`。


[![将 Web.config 文件添加到成员资格文件夹](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**图 3**： 添加`Web.config`文件为`Membership`文件夹 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image9.png))


此时你的项目应包含两个`Web.config`文件： 一个中的根目录，并且一种`Membership`文件夹。


[![你的应用程序现在应包含两个 Web.config 文件](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**图 4**： 你应用程序应现在包含两个`Web.config`文件 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image12.png))


更新中的配置文件`Membership`文件夹，以便它禁止对匿名用户访问。

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

这就是所有到它 ！

要测试此更改，请访问在浏览器主页，请确保你已注销。由于 ASP.NET 应用程序的默认行为是允许所有访问者，因为我们未对进行任何授权修改的根目录`Web.config`文件中，我们将能够访问作为匿名访问者的根目录中的文件。

单击左侧列中找到的创建用户帐户链接。 这将需要你`~/Membership/CreatingUserAccounts.aspx`。 由于`Web.config`文件中`Membership`文件夹定义授权规则，以禁止匿名访问，`UrlAuthorizationModule`中止请求并返回 HTTP 401 未授权的状态。 `FormsAuthenticationModule`修改这到 302 重定向状态中，向我们发送到登录页。 请注意，页面我们已尝试访问 (`CreatingUserAccounts.aspx`) 传递到登录页通过`ReturnUrl`查询字符串参数。


[![自 URL 授权规则禁止匿名访问，我们将重定向到登录页](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**图 5**： 由于 URL 授权规则禁止匿名访问，我们将重定向到登录页 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image15.png))


在成功登录，我们将重定向到`CreatingUserAccounts.aspx`页。 这一次`UrlAuthorizationModule`允许对页的访问，因为我们不再是匿名的。

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>应用到的特定位置的 URL 授权规则

中定义的授权设置`<system.web>`部分`Web.config`适用于所有该目录及其子目录中的 ASP.NET 资源 (直至否则由另一个重写`Web.config`文件)。 在某些情况下，不过，我们可能想在给定目录具有一个或两个特定页除外特定授权配置中的所有 ASP.NET 资源。 这可以通过添加`<location>`中的元素`Web.config`指向其授权规则不同，该文件，其中定义其唯一的授权规则。

若要演示如何使用`<location>`元素以重写特定资源的配置设置，让我们自定义授权设置，以便可以访问仅 Tito `CreatingUserAccounts.aspx`。 若要完成此操作，将添加`<location>`元素`Membership`文件夹的`Web.config`文件并更新其标记，以便其类似于下面所示：

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>`中的元素`<system.web>`定义 ASP.NET 中的资源的默认 URL 授权规则`Membership`文件夹及其子文件夹。 `<location>`元素使我们可以重写这些规则为特定资源。 在上面的标记中`<location>`元素引用`CreatingUserAccounts.aspx`页上，并指定其的授权规则，例如允许 Tito，但拒绝其他人。

若要测试此授权更改，启动通过访问为匿名用户的网站。 如果你尝试访问中的任意页`Membership`文件夹，如`UserBasedAuthorization.aspx`、`UrlAuthorizationModule`将拒绝该请求，将重定向到登录页。 作为说，Scott，登录后，你可以访问中的任意页`Membership`文件夹*除*为`CreatingUserAccounts.aspx`。 尝试访问`CreatingUserAccounts.aspx`登录其他人但 Tito 将导致未经授权的访问尝试，将你重定向回登录页。

> [!NOTE]
> `<location>`元素必须出现在配置的外部`<system.web>`元素。 你需要使用一个单独`<location>`你想要重写其授权设置的每个资源的元素。


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>如何查看`UrlAuthorizationModule`使用授权规则来允许或拒绝访问

`UrlAuthorizationModule`确定是否授权对特定 URL 的特定标识，通过分析 URL 授权规则一次，从第一个和下使用自己的方式。 只要找到匹配项，用户被授予或拒绝访问，根据如果中找到匹配`<allow>`或`<deny>`元素。 **如果不找到任何匹配项，则用户被授予访问权限。** 因此，如果你想要限制访问，则你使用命令性`<deny>`URL 授权配置中的最后一个元素的元素。 **如果省略 * * *`<deny>`* * * 元素中，所有用户将被都授予访问。**

若要更好地了解使用的过程`UrlAuthorizationModule`若要确定颁发机构，例如，我们看之前在此步骤中的 URL 授权规则。 第一个规则是`<allow>`可以访问 Tito 和 Scott 的元素。 第二个规则中是`<deny>`供所有人拒绝访问的元素。 如果匿名用户访问，`UrlAuthorizationModule`通过要求，启动是匿名 Scott 或 Tito？ 答案，显然，为否，以便它将继续进行第二条规则。 是匿名的每个人都集中？ 因为答案此处为是，`<deny>`实际上放规则，并在距访客将重定向到登录页。 同样，如果访问 Jisun，`UrlAuthorizationModule`启动通过要求，是 Jisun Scott 或 Tito？ 因为她不可以，`UrlAuthorizationModule`继续到第二个问题，集中的每个人都是 Jisun？ 她是，因此她，也被拒绝访问。 最后，如果 Tito 访问时，第一个问题所带来`UrlAuthorizationModule`是赞成答案，因此 Tito 被授予访问权限。

由于`UrlAuthorizationModule`授权规则从上到下，在任何匹配项，停止务必具有更多特定规则之前特定性更强的进程。 也就是说，若要定义禁止 Jisun 和匿名用户，但允许所有其他身份验证的用户的授权规则，则将开头最特定的规则的一个影响 Jisun-并继续到更具体的规则-允许所有其他的那些身份验证的用户，但拒绝所有匿名用户。 以下 URL 授权规则通过第一次拒绝 Jisun，，然后拒绝任何匿名用户来实现此策略。 Jisun 以外的任何经过身份验证的用户将被授予访问因为两种情况均`<deny>`语句将匹配。

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>步骤 2： 为未经授权的、 经过身份验证的用户修复工作流

如我们中的 URL 授权工作流部分查看本教程中前面所述，随时调查未授权的请求，`UrlAuthorizationModule`中止请求并返回 HTTP 401 未授权的状态。 通过修改此 401 状态`FormsAuthenticationModule`到 302 重定向将用户发送到登录页的状态。 即使用户进行身份验证，在任何未经授权请求时，会发生此工作流。

返回到登录页的身份验证的用户很可能将它们混淆，因为用户已登录到系统。 通过工作很少我们可以通过将重定向到说明它们试图访问受限制的页面的页面进行未经授权的请求经过身份验证的用户提高此工作流。

首先在名为 web 应用程序的根文件夹中创建新的 ASP.NET 页`UnauthorizedAccess.aspx`; 不要忘记将使用此页`Site.master`母版页。 在创建后此页，删除引用的内容控件`LoginContent`ContentPlaceHolder 以便主控页的默认内容将显示。 接下来，添加一条消息，即描述一种情况，用户尝试访问受保护的资源。 在添加此类消息后`UnauthorizedAccess.aspx`页面的声明性标记的外观应与以下类似：

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

现在，我们需要修改工作流，以便如果通过已验证的用户来执行未授权的请求发送到`UnauthorizedAccess.aspx`而不是登录页的页。 将未经授权的请求重定向到登录页的逻辑隐藏的私有方法内`FormsAuthenticationModule`类，因此我们不能自定义此行为。 我们可以做什么，但是，是将我们自己的逻辑添加到将用户重定向的登录页到`UnauthorizedAccess.aspx`在需要时。

当`FormsAuthenticationModule`未经授权的访问者将重定向到登录页将追加到具有名称查询字符串的请求，未经授权 URL `ReturnUrl`。 例如，如果未经授权的用户尝试访问`OnlyTito.aspx`、`FormsAuthenticationModule`会重定向到`Login.aspx?ReturnUrl=OnlyTito.aspx`。 因此，如果通过具有包括查询字符串的经过身份验证的用户访问登录页`ReturnUrl`参数，然后我们知道此未经身份验证的用户只是尝试访问她无权查看页面。 在这种情况下，我们想要重定向到她`UnauthorizedAccess.aspx`。

若要完成此操作，请将以下代码添加到登录页`Page_Load`事件处理程序：

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

上面的代码将重定向到经过身份验证，则未经授权的用户`UnauthorizedAccess.aspx`页。 若要查看此逻辑操作中的，访问作为匿名访问者站点并单击左侧列中的创建用户帐户链接。 这将需要你`~/Membership/CreatingUserAccounts.aspx`页面，在步骤 1 我们配置为仅允许 Tito 的访问。 匿名用户禁止的因为`FormsAuthenticationModule`将我们重定向回登录页。

此时我们是匿名的因此`Request.IsAuthenticated`返回`false`和我们不重定向到`UnauthorizedAccess.aspx`。 相反，将显示登录页。 Tito，如罗斯向她以外的用户登录。 输入适当的凭据后, 的登录页将重定向我们回到`~/Membership/CreatingUserAccounts.aspx`。 但是，由于此页才可以访问 Tito，我们无权查看它，将立即返回到登录页。 这一次，但是，`Request.IsAuthenticated`返回`true`(和`ReturnUrl`查询字符串参数存在)，因此我们将重定向到`UnauthorizedAccess.aspx`页。


[![通过身份验证，未经授权的用户将被重定向到 UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**图 6**： 经过身份验证，未经授权的用户将被重定向到`UnauthorizedAccess.aspx`([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image18.png))


此自定义工作流显示通过执行短路图 2 所示的周期更有意义且简单的用户体验。

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>步骤 3： 限制基于当前登录用户的功能

URL 授权更便于指定粗糙的授权规则。 正如我们看到在步骤 1 中，使用 URL 授权我们可以用于简单地状态允许哪些标识，并且拒绝哪些功能在查看文件夹中的特定页或所有页面。 在某些情况下，但是，我们可能想要允许所有用户访问页上，但限制基于用户访问它的页面的功能。

请考虑使经过身份验证的访问者可以查看他们的产品电子商务网站的大小写。 在匿名用户访问产品页时，它们会看到仅将产品信息并不能提供机会留下评论。 但是，身份验证的用户访问同一页上会看到审阅的接口。 如果经过身份验证的用户具有未检查此产品，接口将使其能够提交评论;否则，它将显示它们其以前提交的检查。 这种情况下过头来另外，产品页可能显示的其他信息，并且提供为在电子商务公司工作的这些用户的扩展的功能。 例如，产品页可能会列出库存量的清单，并包含用于编辑产品的价格和描述当员工访问选项。

可以以声明方式或以编程方式 （或两个的某种组合），可以实现这种细粒度授权规则。 下一节中，我们将了解如何以实现通过 LoginView 控件的细粒度授权。 接下来，我们将探讨编程技术。 我们可以看看应用细粒度授权规则之前，但是，我们首先需要创建其功能依赖于用户访问它的页。

让我们创建的页面列出一个 GridView 中特定目录中的文件。 GridView 将列出每个文件的名称、 大小和其他信息，以及包括 LinkButtons 两列： 一个，标题为视图和一个名为的删除。 如果单击视图 LinkButton 后，将显示所选文件的内容;如果单击删除 LinkButton 后，将删除该文件。 让我们最初创建此页，这样，其视图和 delete 功能可供所有用户如此。 在使用中我们将了解如何启用或禁用这些功能根据用户访问的页面的 LoginView 控件和以编程方式限制功能部分。

> [!NOTE]
> 我们将要生成 ASP.NET 页使用 GridView 控件显示的文件的列表。 自系列侧重于窗体身份验证、 授权、 用户帐户和角色本教程中，我不希望花费太多时间来讨论 GridView 控件的内部工作情况。 虽然本教程提供了有关设置此页的特定分步说明，它不会不深入探讨原因所做的某些选择，或影响特定属性对呈现输出的详细信息。 GridView 控件全面检查，请查阅我*[在 ASP.NET 2.0 中使用数据](../../data-access/index.md)*教程系列。


首先打开`UserBasedAuthorization.aspx`文件中`Membership`文件夹并将 GridView 控件添加到名为的页`FilesGrid`。 在 GridView 的智能标记，单击编辑列链接以启动字段对话框中。 从此处，取消选中自动生成字段中的复选框左下角。 接下来，添加选择按钮、 删除按钮和两个 BoundFields 从左上角 （选择和删除按钮可以找到的 CommandField 类型下方）。 设置选择按钮的`SelectText`属性来查看和第一个 BoundField`HeaderText`和`DataField`为名称的属性。 设置第二个 BoundField`HeaderText`属性大小 （字节），其`DataField`属性长度，其`DataFormatString`{0: n0} 的属性并将其`HtmlEncode`属性设置为 False。

配置后 GridView 的列，单击确定关闭字段对话框。 从属性窗口中，设置 GridView`DataKeyNames`属性`FullName`。 此时 GridView 的声明性标记应如下所示：

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

创建 GridView 的标记，我们就可以编写的代码会检索特定目录中的文件，并将其绑定到 GridView。 将以下代码添加到页面的`Page_Load`事件处理程序：

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

上述代码使用[`DirectoryInfo`类](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx)以获取应用程序的根文件夹中的文件的列表。 [ `GetFiles()`方法](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx)返回的所有文件目录中为一个数组[`FileInfo`对象](https://msdn.microsoft.com/library/system.io.fileinfo.aspx)，后者然后绑定到 GridView。 `FileInfo`对象具有多种类型的属性，如`Name`， `Length`，和`IsReadOnly`，等等。 正如您可以看到从其声明性的标记，只需显示的 GridView`Name`和`Length`属性。

> [!NOTE]
> `DirectoryInfo`和`FileInfo`类位于[`System.IO`命名空间](https://msdn.microsoft.com/library/system.io.aspx)。 因此，将任一需要开头这些类名称与命名空间名称或具有到的类文件导入的命名空间 (通过`using System.IO`)。


需要一段时间来访问此页面，通过浏览器。 它将显示的文件驻留在应用程序的根目录中的列表。 单击任何视图或删除 LinkButtons 将导致回发，但会发生任何操作，因为我们尚未为创建所需的事件处理程序。


[![GridView 列出了 Web 应用程序的根目录中的文件](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**图 7**: GridView 列出了 Web 应用程序的根目录中的文件 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image21.png))


我们需要一种方法以显示所选文件的内容。 返回到 Visual Studio 并添加名为文本框中`FileContents`上面 GridView。 设置其`TextMode`属性`MultiLine`及其`Columns`和`Rows`属性设置为 95%和 10，分别。

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

接下来，为 GridView 创建事件处理程序[`SelectedIndexChanged`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx)并添加以下代码：

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

此代码使用 GridView`SelectedValue`属性来确定所选文件的完整文件名。 在内部，`DataKeys`为了获取引用集合`SelectedValue`，因此它是命令性设置 GridView`DataKeyNames`为名称，如之前在此步骤中所述的属性。 [ `File`类](https://msdn.microsoft.com/library/system.io.file.aspx)中读取选定的文件的内容读入一个字符串，然后分配给`FileContents`文本框的`Text`属性，从而显示在页面上所选文件的内容。


[![选择文件的内容显示在文本框中](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**图 8**: 选择文件内容显示在文本框中 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> 如果你查看包含 HTML 标记中，文件的内容，然后尝试查看或删除文件，你将收到`HttpRequestValidationException`错误。 这是因为在回发时，文本框的内容会发送回 web 服务器。 默认情况下，ASP.NET 将引发`HttpRequestValidationException`错误，每当检测到潜在危险的回发内容，如 HTML 标记。 若要禁用的发生此错误，请关闭页请求验证通过添加`ValidateRequest="false"`到`@Page`指令。 有关详细信息的优势的请求验证在以及哪些预防措施，应执行时禁用它，读取[请求验证-防止脚本攻击](https://asp.net/learn/whitepapers/request-validation/)。


最后，将事件处理程序替换为以下代码添加为 GridView [ `RowDeleting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

代码将只显示要删除中的文件的完整名称`FileContents`文本框中*而无需*真正删除该文件。


[![单击删除按钮不会实际删除文件](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**图 9**： 单击删除按钮不会实际删除文件 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image27.png))


我们在步骤 1 中配置 URL 授权规则，以禁止在查看中的页的匿名用户`Membership`文件夹。 为了更好地表现出细粒度身份验证，让我们允许匿名用户访问`UserBasedAuthorization.aspx`页上，但使用有限的功能。 若要打开此页为可供所有用户，添加以下`<location>`元素`Web.config`文件中`Membership`文件夹：

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

添加这后`<location>`元素，通过从站点的日志记录来测试新的 URL 授权规则。 作为匿名用户你应该有权访问`UserBasedAuthorization.aspx`页。

任何经过身份验证或匿名用户可以访问当前，`UserBasedAuthorization.aspx`页上，查看或删除文件。 让我们使它，以便只有经过身份验证的用户可以查看文件的内容和仅 Tito 可以删除文件。 以声明方式，以编程方式，或通过组合这两种方法，可以应用此类细粒度授权规则。 让我们使用声明性方法来限制谁可以查看文件; 的内容我们将使用编程方法来限制可以删除文件。

### <a name="using-the-loginview-control"></a>使用 LoginView 控件

如我们所见过去教程中，但是 LoginView 控件用于显示不同的接口，对于经过身份验证和匿名用户，并提供隐藏不能由匿名用户访问的功能的简单办法。 由于匿名用户无法查看或删除文件，我们只需显示`FileContents`时由已经过身份验证的用户访问页的文本框。 若要实现此目的，将 LoginView 控件添加到页，将其命名为`LoginViewForFileContentsTextBox`，进而`FileContents`LoginView 控件到文本框中的声明性标记`LoggedInTemplate`。

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

LoginView 的模板中的 Web 控件将不再从代码隐藏类直接访问。 例如， `FilesGrid` GridView 的`SelectedIndexChanged`和`RowDeleting`事件处理程序当前引用`FileContents`带代码的 TextBox 控件类似：

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

但是，此代码将不再有效。 通过移动`FileContents`到文本框中`LoggedInTemplate`文本框中不能直接访问。 相反，我们必须使用`FindControl("controlId")`方法以编程方式引用控件。 更新`FilesGrid`引用文本框中的事件处理程序如下所示：

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

移到 LoginView 的文本框中之后`LoggedInTemplate`和页面的代码更新为引用文本框中使用`FindControl("controlId")`模式，请访问作为匿名用户页。 如图 10 显示，`FileContents`不显示文本框。 但是，仍显示视图 LinkButton。


[![LoginView 控件只呈现已验证用户的 FileContents 文本框中](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**图 10**: LoginView 控件只呈现`FileContents`文本框的经过验证的用户 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image30.png))


若要隐藏的匿名用户视图按钮的一种方法是将 GridView 字段转换为 TemplateField。 这将生成的视图 LinkButton 中包含的声明性的标记的模板。 我们然后可以将 LoginView 控件添加到 TemplateField 并放置在 LoginView LinkButton `LoggedInTemplate`，从而隐藏从匿名访问者视图按钮。 若要完成此操作，请单击从 GridView 的智能标记上以启动字段对话框中的编辑列链接。 接下来，从左下角中的列表中选择选择按钮，然后单击转换 TemplateField 链接到此字段。 这样将修改字段的声明性标记：

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 到: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

此时，我们可以向 TemplateField 添加 LoginView。 以下标记显示仅适用于经过身份验证的用户视图 LinkButton。

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

如图 11 显示，最终结果不会非常作为视图显示了列仍即使视图 LinkButtons 列中处于隐藏状态。 我们将在下一节中了解如何隐藏整个 GridView 列 （和而不仅仅是 LinkButton）。


[![LoginView 控件为匿名访客隐藏视图 LinkButtons](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**图 11**: LoginView 控件为匿名访客隐藏视图 LinkButtons ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>以编程方式限制功能

在某些情况下，声明性的技术是不足，无法限制到页上的功能。 例如，某些页面功能的可用性可能依赖于超出中访问的页面的用户是否是匿名还是已通过身份验证的条件。 在这种情况下，可以显示或隐藏通过编程方式的各种用户界面元素。

若要以编程方式限制功能，我们需要执行两个任务：

1. 确定访问的页面的用户是否可以访问该功能，并
2. 以编程方式修改基于用户是否有权访问问题的功能的用户界面。

为了演示这两个任务的应用程序，让我们只允许 Tito 从 GridView 删除文件。 然后，我们第一个任务是确定它是否是 Tito 中访问的页面。 一旦确定的我们需要 （或者） 可以隐藏显示 GridView 的删除列。 GridView 的列都可通过访问其`Columns`属性; 如果仅呈现列其`Visible`属性设置为`true`（默认值）。

以下代码添加到`Page_Load`之前将数据绑定到 GridView 的事件处理程序：

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

如我们所述[*概述窗体身份验证的*](../introduction/an-overview-of-forms-authentication-cs.md)教程中，`User.Identity.Name`返回标识的名称。 这对应于登录控件中输入的用户名。 如果它是 Tito 中访问的页面，GridView 的第二列的`Visible`属性设置为`true`; 否则为它设置为`false`。 最终结果是，当 Tito 以外的其他人访问页上，另一个身份验证的用户或匿名用户，删除列不呈现 （请参阅图 12）;但是，当 Tito 访问页时，删除列是存在 （请参阅图 13）。


[![删除列是不呈现时访问由人以外的其他 Tito （如罗斯向她）](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**图 12**: 删除列是不呈现时访问由人以外的其他 Tito （如罗斯向她） ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image36.png))


[![为 Tito 呈现删除列](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**图 13**: 删除列呈现为 Tito ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>步骤 4： 将授权规则应用于类和方法

在步骤 3 中我们将不允许匿名用户通过查看文件的内容，并禁止所有用户，但 Tito 删除文件。 这被通过通过声明性和编程技术访问者在未经授权访问隐藏的关联的用户界面元素。 对于我们的简单示例，正确隐藏用户界面元素已很简单，但呢更为复杂的网站其中可能有多种不同方式来执行相同的功能？ 在限制该功能未经授权的用户，会发生什么情况如果我们忘记隐藏或禁用所有合适的用户界面元素？

确保未经授权的用户不能访问的功能的特定部分的简单办法是修饰该类或方法替换[`PrincipalPermission`属性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)。 当.NET 运行时使用的类，或者执行其方法之一时，它会检查以确保当前安全上下文有权使用类或执行方法。 `PrincipalPermission`属性提供一种机制，通过该我们可以定义这些规则。

让我们演示如何使用`PrincipalPermission`属性 GridView`SelectedIndexChanged`和`RowDeleting`分别禁止执行的匿名用户和 Tito，以外的用户的事件处理程序。 我们需要做的只是添加之上打开每个函数定义适当的特性：

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

为属性`SelectedIndexChanged`仅身份验证的用户的事件处理程序决定可以执行的事件处理程序，而上的属性的`RowDeleting`事件处理程序限制到 Tito 执行。

如果由于某种原因，Tito 以外的用户尝试执行`RowDeleting`事件处理程序或未经身份验证的用户尝试执行`SelectedIndexChanged`事件处理程序的.NET 运行时将引发`SecurityException`。


[![如果安全上下文无权执行方法，则会引发安全异常](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**图 14**： 如果安全上下文未被授权执行方法，`SecurityException`引发 ([单击以查看实际尺寸的图像](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> 若要允许多个安全上下文之间进行访问的类或方法，修饰类或方法使用`PrincipalPermission`为每个安全上下文的属性。 也就是说，若要允许 Tito 和罗斯向她执行`RowDeleting`事件处理程序添加*两个*`PrincipalPermission`属性：


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

除了 ASP.NET 页中，许多应用程序还具有一个体系结构，包括各层，如业务逻辑和数据访问层。 这些层通常作为类库实现，并提供类和用于执行业务逻辑和数据相关的功能的方法。 `PrincipalPermission`属性可用于将授权规则应用于这些层。

有关详细信息使用`PrincipalPermission`属性以便定义类和方法的授权规则，请参阅[Scott Guthrie](https://weblogs.asp.net/scottgu/)的博客文章[业务和数据层使用添加授权规则`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>摘要

在本教程中我们介绍了如何将基于用户的授权规则应用。 与 ASP 一下，我们已开始。NET 的 URL 授权框架。 在每个请求时，ASP.NET 引擎的`UrlAuthorizationModule`检查在应用程序的配置，以确定是否标识有权访问请求的资源中定义的 URL 授权规则。 简单地说，URL 授权使易于指定特定目录中的特定页或所有页面的授权规则。

URL 授权框架针对授权规则按页。 使用 URL 授权请求标识有权访问特定资源，或不。 许多方案中，但是，调用多个细粒度授权规则。 而不是定义谁有权访问的页面，我们可能需要让所有人访问的页面，但若要显示不同的数据或提供根据不同的用户访问的页面的不同功能。 页级授权通常包括以下隐藏特定的用户界面元素，以防止未经授权的用户访问被禁止的功能。 此外，它是可以使用属性来限制对类和执行的某些用户其方法的访问。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [将授权规则添加到业务和使用的数据层`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET 授权](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Iis 6 和 IIS7 安全之间的更改](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [配置特定文件和子目录](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [限制基于用户的数据修改功能](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [LoginView 控件快速入门](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [了解 IIS7 URL 授权](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule`技术文档](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [在 ASP.NET 2.0 中使用数据](../../data-access/index.md)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](validating-user-credentials-against-the-membership-user-store-cs.md)
[下一页](storing-additional-user-information-cs.md)
