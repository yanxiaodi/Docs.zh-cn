---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: "窗体身份验证 (VB) 的概述 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将转而从单纯讨论的实现;具体而言，我们将了解实现窗体身份验证。 Web 应用程序 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 90bcff91d0642e6af66f43fd807b253cc516d277
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-forms-authentication-vb"></a>窗体身份验证 (VB) 的概述
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip)或[下载 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> 在本教程中我们将转而从单纯讨论的实现;具体而言，我们将了解实现窗体身份验证。 我们开始学习本教程中构造的 web 应用程序将继续基于在后续教程中，当我们从简单的窗体身份验证移到成员资格和角色。
> 
> 有关本主题，请参阅有关详细信息此视频：[使用基本窗体中的身份验证 ASP.NET](# "using-basic-forms-authentication-in-aspnet")。


## <a name="introduction"></a>介绍

在[前面教程](security-basics-and-asp-net-support-vb.md)我们讨论由 ASP.NET 提供的各种身份验证、 授权和用户帐户选项。 在本教程中我们将转而从单纯讨论的实现;具体而言，我们将了解实现窗体身份验证。 我们开始学习本教程中构造的 web 应用程序将继续基于在后续教程中，当我们从简单的窗体身份验证移到成员资格和角色。

本教程开始使用该窗体身份验证工作流，我们在前面的教程时接触的主题深入探讨了如何。 接下来，我们将创建 ASP.NET 网站通过其进行演示窗体身份验证的概念。 接下来，我们将配置使用 forms 身份验证、 创建简单的登录页，并请参阅如何确定，在代码中，是否对用户身份验证，如果是这样，用户名它们登录时使用的站点。

了解身份验证工作流，使其在 web 应用程序，并创建登录和注销页是在生成的 ASP.NET 应用程序支持用户帐户，通过网页上的用户进行身份验证过程中的所有重要步骤的窗体。 由于此-，因为这些教程相互依赖另-我将鼓励您就可以完成本教程中完全之前前进到下一个即使已经具有有体验配置在过去的项目中窗体身份验证。

## <a name="understanding-the-forms-authentication-workflow"></a>了解窗体身份验证工作流

ASP.NET 运行时处理一个 ASP.NET 资源，如 ASP.NET 页或 ASP.NET Web 服务的请求时请求在其生命周期期间引发事件的数。 没有在请求的请求未经过身份验证并获得授权，在未处理的异常等的情况下引发的事件时引发的非常开始且非常结束时引发的事件。 若要查看事件的完整列表，请参阅[HttpApplication 对象的事件](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx)。

*HTTP 模块*是在响应特定事件在请求生命周期中执行其代码的托管的类。 ASP.NET 附带许多执行所需的任务在后台的 HTTP 模块。 与我们讨论尤其相关的两个内置的 HTTP 模块为：

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -对用户进行身份验证通过检查窗体身份验证票证，它通常包括在用户的 cookie 集合。 如果存在未窗体身份验证票证，则用户是匿名的。
- **[哪](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** -确定当前用户是否有权访问所请求的 URL。 此模块通过参考应用程序的配置文件中指定的授权规则确定该颁发机构。 ASP.NET 还包括[FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) ，它确定机构通过参考请求的文件 Acl。

FormsAuthenticationModule 尝试之前执行哪 （和 FileAuthorizationModule） 用户进行身份验证。 如果发出请求的用户无权访问请求的资源，授权模块终止请求，并返回[HTTP 401 未授权](http://www.checkupdown.com/status/E401.html)状态。 在 Windows 身份验证方案，HTTP 401 状态将返回到浏览器。 此状态代码会导致浏览器以提示用户输入其凭据通过模式对话框。 使用窗体身份验证，但是，HTTP 401 未授权状态是永远不会发送到浏览器由于 FormsAuthenticationModule 检测到此状态并会修改它以将用户重定向到登录页相反 (通过[HTTP 302 重定向](http://www.checkupdown.com/status/E302.html)状态)。

登录页的责任是确定是否用户的凭据是否有效并且，如果是这样，若要创建窗体身份验证票证和将用户重定向回页，他们已尝试访问。 在对 FormsAuthenticationModule 用于标识用户的网站上的页的后续请求中包含身份验证票证。


[![窗体身份验证工作流](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**图 01**: 窗体身份验证工作流 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>记住跨页访问的身份验证票证

登录后，窗体身份验证票证必须发送回上每个请求的 web 服务器以便浏览站点时，用户仍然登录。 这通常通过将身份验证票证放置在用户的 cookie 集合。 [Cookie](http://en.wikipedia.org/wiki/HTTP_cookie)小文本文件，驻留在用户的计算机上，在创建 cookie 的网站的每个请求的 HTTP 标头中传输。 因此，一旦窗体身份验证票证已创建并存储在浏览器的 cookie，每次后续访问到该站点将发送以及请求，从而确定用户的身份验证票证。

> [!NOTE]
> 每个教程中使用的演示 web 应用程序作为下载提供。 此可下载应用程序是使用针对.NET Framework 3.5 版的 Visual Web Developer 2008 创建的。 由于此应用针对.NET 3.5 中，其 Web.config 文件包括其他的 3.5 特定配置元素。 较长的篇幅简短，如果你尚未在您的计算机然后可下载的 web 应用程序上安装.NET 3.5 不会需要首先从 Web.config 删除 3.5 特定于标记。


Cookie 的一个方面是其过期时间，即的日期和时间浏览器将放弃该 cookie。 窗体身份验证 cookie 过期后，可以不再进行身份验证并因此成为匿名用户。 当用户从公共终端来访时，很有他们希望其身份验证票证过期时它们关闭其浏览器。 当来访从主页时，但是，相同的用户可能想要身份验证票证，以便它们不具有要记住跨浏览器重新启动到重新登录每次访问该站点。 此决定通常是由用户在窗体的一个记住我的复选框的登录页上做出的。 在步骤 3 中，我们将查看如何在登录页中实现一个记住我的复选框。 以下教程地址详细信息中的身份验证票证超时设置。

> [!NOTE]
> 很可能用于登录到网站的用户代理可能不支持 cookie。 在这种情况下，ASP.NET 还可以使用无 cookie 窗体身份验证票证。 在此模式下，身份验证票证编码到 URL 中。 我们将了解何时使用无 cookie 的身份验证票证和它们的创建和管理在下一步的教程。


### <a name="the-scope-of-forms-authentication"></a>窗体身份验证的范围

管理 FormsAuthenticationModule ASP.NET 运行时的一部分的代码。 之前的 Microsoft 的版本 7 [Internet 信息服务 (IIS)](https://www.iis.net/) web 服务器没有之间 IIS 的 HTTP 管道和 ASP.NET 运行时的管道的不同屏障。 简而言之，在 IIS 6 和更早版本，FormsAuthenticationModule 才会执行时请求从 IIS 委派给 ASP.NET 运行时。 默认情况下，当请求的.aspx、.asmx 或.ashx 扩展名页时，IIS 也会处理之类的静态内容本身的 HTML 页和 CSS 和图像文件-与仅将请求消息到 ASP.NET 运行时。

IIS 7，但是，可以集成的 IIS 和 ASP.NET 的管道。 使用其他一些配置设置，你可以设置 IIS 7 以调用为 FormsAuthenticationModule*所有*请求。 此外，IIS 7 可以定义任何类型的文件的 URL 授权规则。 有关详细信息，请参阅[更改之间 IIS6 和 IIS7 安全](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above)，[你的 Web 平台安全](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)，和[了解 IIS7 URL 授权](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)。

较长的篇幅简短，在 IIS 7 之前的版本，你可以只使用窗体身份验证保护由 ASP.NET 运行时的资源。 同样，URL 授权规则仅应用于由 ASP.NET 运行时的资源。 但与 IIS 7 很可能将 FormsAuthenticationModule 和哪集成到 IIS 的 HTTP 管道，从而扩展到的所有请求此功能。

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>步骤 1： 为本系列教程中创建 ASP.NET 网站

若要访问尽可能多的用户，我们将生成本系列中的 ASP.NET 网站将创建与 Microsoft 的免费版本的 Visual Studio 2008 起， [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)。 我们将实现中的 SqlMembershipProvider 用户存储区[Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx)数据库。 如果你使用 Visual Studio 2005 或其他版本的 Visual Studio 2008 或 SQL Server，别担心-步骤将几乎相同，并且将指出方面的任何非细微差异。

我们可以配置窗体身份验证之前，我们首先需要 ASP.NET 网站。 首先，创建新的文件系统基于 ASP.NET 网站。 若要完成此操作，启动 Visual Web Developer 然后转到文件菜单并选择新的网站，显示新建网站对话框。 选择 ASP.NET Web 站点模板、 到文件系统设置的位置下拉列表中，选择一个文件夹，以将 web 站点，和将语言设置为 VB. 这将使用 Default.aspx ASP.NET 页中，应用程序创建新的 web 站点\_数据文件夹和 Web.config 文件。

> [!NOTE]
> Visual Studio 支持两种项目管理模式： 网站项目和 Web 应用程序项目。 网站项目缺少项目文件中，而 Web 应用程序项目模拟项目体系结构在 Visual Studio.NET 2002年/2003年-它们包括项目文件并将项目的源代码编译到单个程序集，置于 /bin 文件夹。 Visual Studio 2005 最初仅支持的 Web 站点项目、 Web 应用程序项目模型已重新引入 Service Pack 1; 尽管Visual Studio 2008 提供这两种项目模型。 Visual Web Developer 2005 和 2008年版，但是，仅支持网站项目。 我将使用网站项目模型。 如果在使用非 Express edition 并且想要使用[Web 应用程序项目模型](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)相反，随意这样做，但请注意，可能会出现某些差异之间在你的屏幕和必须与执行的步骤上看到的内容屏幕快照所示，这些教程中提供的说明操作。


[![创建新的文件系统基于 Web 站点](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**图 02**： 创建 New File System-Based 网站 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>添加母版页

接下来，将一个新的主页面添加到名为 Site.master 的根目录中的站点。 [主页](https://msdn.microsoft.com/library/wtxbf3hh.aspx)启用页开发人员可以定义可应用于 ASP.NET 页的整个站点模板。 为母版页的主要优势是站点的整体外观可以定义在一个位置，从而使它轻松更新或调整站点的布局。


[![将主页面添加名为 Site.master 到网站](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**图 03**： 将 Master 页名为 Site.master 添加到网站 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image9.png))


定义在母版页中的站点范围页面布局此处。 你可以使用设计视图，然后添加需要任何布局或 Web 控件或您可以通过在源视图中手动手动添加标记。 我结构化我母版页的布局，以模拟中所使用的布局我*[在 ASP.NET 2.0 中使用数据](../../data-access/index.md)* （请参见图 4） 的系列教程。 主控页使用[级联样式表](http://www.w3schools.com/css/default.asp)用于定位和文件 Style.css （它包括在本教程的相关下载） 中定义的 CSS 设置的样式。 你不能从如下所示的标记，而定义的 CSS 规则，以便导航&lt;div&gt;的内容进行了绝对定位，以便它显示在左侧，具有固定的宽度的 200 像素。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

母版页定义静态页面布局和使用母版页的 ASP.NET 页可以编辑的区域。 指示由 ContentPlaceHolder 控件，可以在内容中看到这些内容的可编辑区域&lt;div&gt;。 我们母版页具有单个 ContentPlaceHolder （主内容），但母版页的可能具有多个 ContentPlaceHolders。

上面输入的标记，切换到设计视图显示主控页的布局。 使用此母版页任何 ASP.NET 页将具有此统一的布局，与指定的主内容区域的标记的功能。


[![Master 页上，通过设计视图查看](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**图 04**： 在母版页、 时查看通过设计视图 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>创建内容页

此时我们在我们的网站，具有 Default.aspx 页面，但它不使用我们刚刚创建的母版页。 虽然可能操作的 web 页后，可以使用母版页，声明性的标记，如果页不包含任何内容，但尚未很容易地只需删除页面，并将其重新添加到项目中，指定主页后，可以使用。 因此，从开始通过从项目中删除 Default.aspx。

接下来，右键单击解决方案资源管理器中的项目名称，然后选择添加新 Web 窗体名为 Default.aspx。 这次，检查选择母版页复选框，然后从列表中选择 Site.master 母版页。


[![添加新的 Default.aspx 页选择选择母版页](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**图 05**： 添加新 Default.aspx 页上选择选择母版页 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image15.png))


[![使用 Site.master 母版页](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**图 06**： 使用 Site.master 母版页 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> 如果你正在使用 Web 应用程序项目模型添加新项对话框中不包括选择母版页复选框。 相反，你需要添加 Web 内容窗体类型的项。 选择 Web 内容窗体选项并单击添加后，Visual Studio 将显示相同的 Select 主控形状图 6 所示的对话框。


新 Default.aspx 页面的声明性标记只包括@Page指令指定为主路径为母版页的主内容 ContentPlaceHolder 页面文件和内容控件。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

现在，将 Default.aspx 留空。 我们将稍后在本教程中添加内容返回到它。

> [!NOTE]
> 我们母版页包含为菜单或某些其他导航接口的部分。 在将来的教程，我们将创建此类接口。


## <a name="step-2-enabling-forms-authentication"></a>步骤 2： 启用窗体身份验证

创建的 ASP.NET 网站，使用我们的下一个任务是启用表单身份验证。 通过指定应用程序的身份验证配置[&lt;身份验证&gt;元素](https://msdn.microsoft.com/library/532aee0e.aspx)在 Web.config 中。&lt;身份验证&gt;元素包含单个属性名为指定应用程序使用的身份验证模型的模式。 此属性可以具有以下四个值之一：

- **Windows** -如在前面的教程中，所述，当应用程序使用 Windows 身份验证它是 web 服务器负责进行身份验证在距访客，，这通常通过基本、 摘要式或集成 Windows身份验证。
- **窗体**-用户进行身份验证通过在网页上的窗体。
- **Passport**-用户进行身份验证使用 Microsoft 的 Passport 网络。
- **无**-使用任何身份验证模型; 所有访问者是匿名的。

默认情况下，ASP.NET 应用程序使用 Windows 身份验证。 若要将身份验证类型更改为窗体身份验证，然后，我们需要修改&lt;身份验证&gt;向窗体元素的 mode 属性。

如果你的项目尚不包含 Web.config 文件，添加一个现在通过右键单击解决方案资源管理器中的项目名称，选择添加新项，然后添加 Web 配置文件。


[![如果你的项目尚未包含 Web.config，将其添加到](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**图 07**： 如果你项目不尚未包括 Web.config、 现在添加 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image21.png))


接下来，找到&lt;身份验证&gt;元素和更新它以使用窗体身份验证。 此更改之后 Web.config 文件的标记应类似于以下：

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> 由于 Web.config 是一个 XML 文件，大小写很重要。 请确保以大写 F.将模式属性设置为窗体，如果你使用不同的大小写，如窗体，你将收到配置错误，访问通过浏览器网站时。


&lt;身份验证&gt;元素可以根据需要包含&lt;窗体&gt;包含窗体身份验证特定设置的子元素。 现在，让我们只需使用的默认窗体身份验证设置。 我们将探讨&lt;窗体&gt;中下一教程中的更多详细信息的子元素。

## <a name="step-3-building-the-login-page"></a>步骤 3： 生成的登录页

为了支持窗体身份验证我们的网站需要登录页。 中的窗体身份验证工作流部分，FormsAuthenticationModule 会自动重定向了解所述用户到登录页如果用户尝试访问的页，它们不是有权查看。 此外，还有将显示一个链接到匿名用户到登录页的 ASP.NET Web 控件。 这回避询问，登录页的 URL 是什么？

默认情况下，窗体身份验证系统需要登录页后，可以命名为 Login.aspx 并放置在 web 应用程序的根目录。 如果你想要使用不同的登录页 URL，你可以做到这一点在 Web.config 中指定。我们将了解如何执行此操作在后续教程。

登录页将显示三个职责。

1. 提供允许在距访客输入其凭据的接口。
2. 确定提交的凭据是否有效。
3. 通过创建窗体身份验证票证登录用户。

### <a name="creating-the-login-pages-user-interface"></a>创建登录页的用户界面

让我们开始使用第一个任务。 将新的 ASP.NET 页添加到名为 Login.aspx 的站点的根目录，并将其与 Site.master 母版页。


[![添加新的 ASP.NET 页名为 Login.aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**图 08**： 添加新 ASP.NET 页名为 Login.aspx ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image24.png))


典型的登录页界面由两个文本框中的一个用于用户的名称，另一个用于其密码-和按钮以提交窗体组成。 网站通常包括一个记住我的复选框，如果选中，仍然生成的身份验证票证存在跨浏览器重新启动。

添加到 Login.aspx 的两个文本框中，并分别设置为用户名和密码，其 ID 属性。 此外将密码的 TextMode 属性设置为密码。 接下来，添加一个复选框控件，将其 ID 属性设置为记住我，其文本属性设为记住 me. 接下来，添加一个名为的 LoginButton 其文本属性设置为登录按钮。 最后，添加标签 Web 控件并将其 ID 属性设置为 InvalidCredentialsMessage，其文本属性到你的用户名或密码无效。 请重试。，为红色，其前景色属性和其可见属性设置为 False。

此时你的屏幕应类似于的屏幕快照图 9 中和声明性语法，你的页应如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![登录页包含两个文本框中，一个复选框、 一个按钮和标签](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**图 09**: 登录页包含两个文本框、 一个复选框、 按钮和标签 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image27.png))


最后，创建一个事件处理程序，为 LoginButton 的 Click 事件。 从设计器中，只需双击该按钮控件，若要创建此事件处理程序。

### <a name="determining-if-the-supplied-credentials-are-valid"></a>确定提供的凭据是否有效

现在，我们需要实现任务 2 中按钮的 Click 事件处理程序-确定提供的凭据是否有效。 若要这样做需要有包含所有用户的凭据，以便我们能够确定是否与任何已知的凭据提供的凭据相匹配的用户存储区。

在 ASP.NET 2.0 中之前, 开发人员负责实施自己两个用户存储区，并编写代码以验证针对存储提供的凭据。 大多数开发人员将实现用户存储在数据库中，创建表包含如用户名、 密码、 电子邮件、 LastLoginDate 和等的列的命名用户。 然后，此表中，将包含每个用户帐户的一条记录。 验证的用户提供的凭据将涉及查询数据库中的匹配的用户名，然后确保数据库中的密码对应于提供的密码。

使用 ASP.NET 2.0，开发人员应使用成员资格提供程序之一来管理用户存储区。 在本系列教程中我们将使用 SqlMembershipProvider，为用户存储使用 SQL Server 数据库。 使用 SqlMembershipProvider 时，我们需要实现特定的数据库架构，包括表、 视图和提供程序所需的存储的过程。 我们将研究如何实现此架构中的*[在 SQL Server 中创建成员身份架构](../membership/creating-the-membership-schema-in-sql-server-vb.md)*教程。 与就地成员身份提供程序中，验证用户的凭据非常简单，只调用[成员资格类](https://msdn.microsoft.com/library/system.web.security.membership.aspx)的[ValidateUser (*用户名*，*密码*)方法](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)，这将返回一个布尔值，该值指示是否的有效性*用户名*和*密码*组合。 如看到我们未实现 SqlMembershipProvider 的用户存储区，我们无法在此时使用成员资格类 ValidateUser 方法。

而不是花时间来生成我们自己自定义用户数据库表 （它将是已过时，我们实现 SqlMembershipProvider 后），让我们改为硬编码在该登录名的有效凭据页本身。 在 LoginButton 的单击事件处理程序，添加以下代码：

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

如你所见，有三个有效的用户帐户-Scott、 Jisun 和 Sam-和所有三个具有相同的密码 （密码）。 该代码循环访问用户和密码数组查找有效的用户名和密码匹配。 如果用户名和密码都有效，我们需要登录用户，然后再将它们重定向到相应的页面。 如果凭据无效，则我们显示 InvalidCredentialsMessage 标签。

当用户输入有效的凭据时，我所述，它们然后重定向到合适的页面。 尽管是合适的页面，什么？ 回想一下，当用户访问它们未被授权查看的页面，FormsAuthenticationModule 自动将他们重定向到登录页。 在此情况下，它包括通过 ReturnUrl 参数查询字符串中的请求的 URL。 也就是说，如果用户试图访问 ProtectedPage.aspx，并且它们无权这样做，FormsAuthenticationModule 将重定向到：

Login.aspx?ReturnUrl=ProtectedPage.aspx

在成功登录，用户应重定向回 ProtectedPage.aspx。 或者，用户可能在其自己 volition 访问登录页。 在这种情况下，在用户登录后它们应将发送到根文件夹的 Default.aspx 页上。

### <a name="logging-in-the-user"></a>在用户的日志记录

假定提供的凭据有效，我们需要创建窗体身份验证票证，从而用户访问该站点中的日志记录。 [FormsAuthentication 类](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx)中[System.Web.Security 命名空间](https://msdn.microsoft.com/library/system.web.security.aspx)提供身份验证系统的日志记录并注销通过窗体的用户的各种的方法。 尽管 FormsAuthentication 类中有几种方法，我们感兴趣此时的三个：

- [GetAuthCookie (*用户名*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -创建提供的名称的窗体身份验证票证*用户名*。 接下来，此方法创建并返回一个包含身份验证票证的内容的 HttpCookie 对象。 如果*persistCookie*为 True 时，创建持久性 cookie。
- [SetAuthCookie (*用户名*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -调用 GetAuthCookie (*用户名*， *persistCookie*)若要生成的窗体身份验证 cookie 的方法。 然后，此方法将添加到 （假设基于 cookie 的窗体身份验证正在使用; 否则为此方法调用处理无 cookie 票证逻辑的内部类） 的 Cookie 集合由 GetAuthCookie 返回的 cookie。
- [RedirectFromLoginPage (*用户名*， *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -此方法调用 SetAuthCookie (*用户名*， *persistCookie*)，然后将用户重定向到合适的页面。

当你需要修改，然后写出到 Cookie 集合的 cookie 再身份验证票证时，GetAuthCookie 会很方便。 如果你想要创建窗体身份验证票证，并将其添加到 Cookie 集合中，但不是希望将用户重定向到相应的页面，SetAuthCookie 非常有用。 可能是你想要保留它们的登录页上或将其发送给某些备用页。

由于我们想要在用户登录并将它们重定向到相应的页面，让我们使用 RedirectFromLoginPage。 更新 LoginButton 的单击事件处理程序，将两个注释的 TODO 行替换为以下代码行：

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

创建窗体身份验证票证时我们用于用户名文本框的 Text 属性的窗体身份验证票证*用户名*参数，并为记住我复选框的选中的状态*persistCookie*参数。

若要测试的登录页，请在浏览器中访问它。 首先输入凭据无效，如 Nope 的用户名和密码的错误。 单击登录按钮时将发生回发，并且将显示 InvalidCredentialsMessage 标签。


[![InvalidCredentialsMessage 标签是显示时输入凭据无效](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**图 10**: InvalidCredentialsMessage 标签是显示时输入无效凭据 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image30.png))


接下来，输入有效的凭据，然后单击登录按钮。 创建回发发生窗体身份验证票证时此时间，你会自动重定向回 Default.aspx。 此时您已登录到网站，尽管有没有视觉提示，以指示当前登录。 在步骤 4，我们将了解如何以编程方式确定用户是否将记录中或不以及如何识别用户访问的页面。

步骤 5 检查日志记录将用户从网站的技术。

### <a name="securing-the-login-page"></a>保护登录页

当用户输入其凭据，并提交登录页窗体时，将-包括她的密码-凭据传输到中的 web 服务器在 Internet 上*纯文本*。 这意味着探查的网络流量任何黑客可以查看的用户名和密码。 若要防止此情况，请务必使用加密的网络流量[安全套接字层 (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)。 这将确保凭据 （以及整个页面的 HTML 标记） 进行加密的那一刻他们离开浏览器，直到其被接收由 web 服务器。

除非你的网站包含敏感信息，将仅需要使用 SSL 的登录页上和其他页上，用户的密码将否则通过发送网络在纯文本中。 不需要担心保护窗体身份验证票证，因为默认情况下，则会同时加密和数字签名 （以防止篡改）。 有关窗体身份验证票证安全的更全面讨论呈现在以下教程中。

> [!NOTE]
> 很多财务和医疗网站中配置为在上使用 SSL*所有*页访问身份验证的用户。 如果你在构建网站可以配置窗体身份验证系统，以便通过安全连接，窗体身份验证票证才会传输。 下一步的教程中，我们将各种窗体身份验证配置选项查看*[窗体身份验证配置和高级主题](../membership/creating-the-membership-schema-in-sql-server-vb.md)*。


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>步骤 4： 检测经过身份验证的访问者，并确定其标识

此时我们具有启用表单身份验证并创建基本的登录页，但我们尚无检查如何我们可以确定用户是否已经过身份验证或匿名。 在某些情况下我们可能想要显示不同的数据或信息取决于是否为经过身份验证或匿名用户访问的页面。 此外，我们通常需要了解的已验证用户身份。

让我们增加现有 Default.aspx 页后，可以演示了这些技术。 在 Default.aspx 中将添加两个面板控件、 一个命名的 AuthenticatedMessagePanel 和另一个命名的 AnonymousMessagePanel。 添加一个名为 WelcomeBackMessage 第一个面板中的标签控件。 在第二个面板添加超链接控件中，将其文本属性设置为 Log In，其 NavigateUrl 属性设为 ~ / Login.aspx。 此时 Default.aspx 的声明性标记应类似以下：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

您已经可能猜到目前为止，本指南旨在向经过身份验证的访问者和仅对匿名访问者 AnonymousMessagePanel 显示仅 AuthenticatedMessagePanel。 若要实现此目的，我们需要设置这些面板的可见属性，具体取决于是否用户登录或未。

[Request.IsAuthenticated 属性](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx)返回一个布尔值，该值指示是否已验证请求。 下列代码输入到页面\_加载事件处理程序代码：

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

使用此代码中的位置，通过浏览器中访问 Default.aspx。 假定你尚未登录，你将看到一个链接到登录页 （请参阅图 11）。 单击此链接，登录到站点。 正如我们看到在步骤 3 中，输入你的凭据后你将返回到 Default.aspx 中，但这次该页面显示欢迎回来 ！ 消息 （请参阅图 12）。


[![匿名访问，日志中链接显示时](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**图 11**： 已显示时访问以匿名方式、 日志中链接 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image33.png))


[![经过身份验证的用户显示欢迎回来 ！Message](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**图 12**： 身份验证的用户显示欢迎回来 ！ 消息 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image36.png))


我们可以确定通过当前登录的用户的身份[HttpContext 对象](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)的[用户属性](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)。 HttpContext 对象表示有关当前请求，并且内的其他一些常见的 ASP.NET 对象，如响应、 请求和会话中的主页。 用户属性表示的当前 HTTP 请求和实现的安全上下文[IPrincipal 接口](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx)。

用户属性由 FormsAuthenticationModule 设置。 具体而言，FormsAuthenticationModule 查找传入请求中的窗体身份验证票证，它将创建一个新的 GenericPrincipal 对象并将其分配给用户属性。

主体对象 （如 GenericPrincipal) 提供对用户的标识和它们所属的角色的信息。 IPrincipal 接口定义两个成员：

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -返回一个布尔值，该值指示是否主体属于指定角色的方法。
- [标识](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx)-返回实现的对象的属性[IIdentity 接口](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx)。 IIdentity 接口定义三个属性： [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx)， [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)，和[名称](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx)。

我们可以确定当前使用下面的代码的访问者的名称：

Dim currentUsersName 作为字符串 = User.Identity.Name

当使用窗体身份验证， [FormsIdentity 对象](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx)创建 GenericPrincipal 的标识属性。 FormsIdentity 类始终返回其 AuthenticationType 属性和 True 的字符串窗体，其 IsAuthenticated 属性。 Name 属性返回在创建窗体身份验证票证时指定的用户名。 这三个属性，除了 FormsIdentity 包含对通过基础的身份验证票证的访问其[票证属性](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx)。 票证属性返回类型的对象[FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)，它具有属性，例如过期、 IsPersistent、 IssueDate、 名称和等等。

重要的一点要清楚的一点此处在于*用户名*FormsAuthentication.GetAuthCookie 中指定的参数 (*用户名*， *persistCookie*)，FormsAuthentication.SetAuthCookie (*用户名*， *persistCookie*)，和 FormsAuthentication.RedirectFromLoginPage (*用户名*， *persistCookie*) 方法是通过 User.Identity.Name 返回的相同值。 此外，这些方法创建的身份验证票证已提供着强制 User.Identity 转换为 FormsIdentity 对象，然后访问票证属性：

Dim ident 作为 FormsIdentity = CType （User.Identity，FormsIdentity）

Dim authTicket As FormsAuthenticationTicket = ident.Ticket

让我们提供 Default.aspx 中的更加个性化的消息。 更新页面\_加载事件处理程序，以便 WelcomeBackMessage 标签的文本属性分配字符串欢迎回来，*用户名*！

WelcomeBackMessage.Text = "Welcome back, " &amp; User.Identity.Name &amp; "!"

图 13 显示此修改的影响 （时作为用户 Scott，日志记录）。


[![在欢迎消息包括当前已登录用户的名称中](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**图 13**： 欢迎消息包括当前记录中用户的名称 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>使用 LoginView 和 LoginName 控件

向经过身份验证和匿名用户显示不同的内容是常见的要求;因此显示当前登录用户的名称。 因此，ASP.NET 包括提供相同的功能显示在图 13 中，但无需编写一行代码的两个 Web 控件。

[LoginView 控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx)是一个基于模板的 Web 控件，可以轻松地向经过身份验证和匿名用户显示不同的数据。 LoginView 包括两个预定义的模板：

- 对匿名的访问者仅显示 AnonymousTemplate-添加到此模板中的任何标记。
- LoggedInTemplate-仅向经过身份验证的用户，会显示此模板的标记。

让我们将 LoginView 控件添加到我们的站点的主页上，Site.master。 而不是添加不仅仅是 LoginView 控件，不过，让我们添加这两个新 ContentPlaceHolder 控件，然后将在该新 ContentPlaceHolder LoginView 控件。 此决策的理由将变得明显很快。

> [!NOTE]
> 除了 AnonymousTemplate 和 LoggedInTemplate，LoginView 控件可以包含特定于角色的模板。 特定于角色的模板仅向这些属于指定角色的用户显示标记。 在将来的教程中，我们将检查 LoginView 控件的基于角色的功能。


首先，通过添加名为 LoginContent 转换为母版页中导航 ContentPlaceHolder &lt;div&gt;元素。 只需将 ContentPlaceHolder 控件从工具箱中拖动到源视图中，放置 TODO 的正上方的生成标记： 菜单将转到此处文本。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

接下来，添加内 LoginContent ContentPlaceHolder LoginView 控件。 放入母版页的 ContentPlaceHolder 控件的内容被视为*默认内容*ContentPlaceHolder 有关。 即，使用此母版页的 ASP.NET 页可以为每个 ContentPlaceHolder 指定他们自己的内容，或使用母版页的默认内容。

LoginView 和其他登录名相关的控件都位于工具箱的登录选项卡。


[![在工具箱 LoginView 控件](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**图 14**: 工具箱中的 LoginView 控件 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image42.png))


接下来，添加两个&lt;巴西 /&gt;紧跟 LoginView 控件，但仍在 ContentPlaceHolder 内的元素。 在这种情况下，导航&lt;div&gt;元素的标记应如下所示：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

LoginView 的模板可以定义从设计器或声明性的标记。 从 Visual Studio 设计器中，展开 LoginView 的智能标记，其中列出了在下拉列表中配置的模板。 输入文本 Hello，陌生人到 AnonymousTemplate;接下来，添加超链接控件并将其文本和 NavigateUrl 属性设置为登录和 ~ / Login.aspx，分别。

后配置 AnonymousTemplate，切换到 LoggedInTemplate 并输入文本，"欢迎使用后，"。 然后将从工具箱的 LoginName 控件拖入 LoggedInTemplate，将其放在"欢迎回来，"文本后立即中。 [LoginName 控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx)，作为其名称表示，显示当前登录用户的名称。 LoginName 控件内部，只需将输出 User.Identity.Name 属性

进行这些添 LoginView 的模板后, 标记应类似于以下：

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

添加到 Site.master 母版页此元素后，我们的网站中每个页面将显示一个不同的消息，具体取决于是否对用户进行身份验证。 图 15 显示通过浏览器访问由用户 Jisun 后的 Default.aspx 页。 欢迎回来，Jisun 消息重复两次： 一次 （通过我们刚添加的 LoginView 控件） 左侧的主控页导航部分中，并且一次是在 Default.aspx 的内容 （通过 Panel 控件和编程逻辑） 的区域。


[![LoginView 控件显示欢迎使用后，Jisun。](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**图 15**: 的 LoginView 控件显示欢迎回来，Jisun。 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image45.png))


因为我们添加 LoginView 到母版页，它可以出现在我们的站点上的每一页。 但是，可能有网页，我们不希望显示此消息。 一个这样的页是登录页上，因为到登录页的链接似乎不在存在适当位置。 由于我们 LoginView 控件置于母版页中 ContentPlaceHolder，我们可以在我们的内容页面中重写此默认标记。 打开 Login.aspx 并转到设计器。 由于我们没有显式定义的内容控件在母版页中 LoginContent ContentPlaceHolder 的 Login.aspx，登录页将为此 ContentPlaceHolder 演示主页面的默认标记。 你可以看到这通过设计器-LoginContent ContentPlaceHolder 显示的默认标记 （LoginView 控件）。


[![登录页显示的默认内容为母版页的 LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**图 16**： 登录页显示的默认内容母版页的 LoginContent ContentPlaceHolder ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image48.png))


若要覆盖默认标记为 LoginContent ContentPlaceHolder，只需右键单击设计器中的区域，然后从上下文菜单中选择创建自定义内容选项。 (当选中时，当使用 Visual Studio 2008 ContentPlaceHolder 包含智能标记，提供的相同选项。)这将添加一个新的内容控件到页面的标记，从而使我们可以定义自定义内容的此页。 无法添加自定义消息在这里，如请登录，但让我们只需将其留空。

> [!NOTE]
> 在 Visual Studio 2005 中，创建自定义内容创建一个空内容 ASP.NET 页中的控件。 在 Visual Studio 2008 中，但是，创建自定义内容主控页的默认将内容复制到新创建的内容控件。 如果使用 Visual Studio 2008，然后，在创建新的内容控件后请确保清除复制通过从母版页的内容。


图 17 显示 Login.aspx 页面时进行此更改后从浏览器访问。 请注意，没有任何 Hello，陌生人或欢迎回来，*用户名*左侧导航区域中的消息&lt;div&gt;因为没有访问 Default.aspx 时。


[![登录页隐藏默认 LoginContent ContentPlaceHolder 标记](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**图 17**： 登录页隐藏默认 LoginContent ContentPlaceHolder 的标记 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>步骤 5： 注销

在步骤 3 中我们讨论过生成登录页，以使用户登录到站点，但我们尚无若要了解如何注销用户。日志记录中的用户的方法，除了 FormsAuthentication 类还提供[SignOut 方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx)。 注销方法只需销毁窗体身份验证票证，从而日志记录将用户从该站点。

该 ASP.NET 产品注销链接此类的常见功能包括一个专门设计用于注销用户控件。[LoginStatus 控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx)显示登录 LinkButton 或注销 LinkButton，具体取决于用户的身份验证状态。 向经过身份验证的用户显示注销 LinkButton 而不为匿名用户呈现登录 LinkButton。 可以通过 LoginStatus 的配置的文本的登录和注销 LinkButtons LoginText 和 LogoutText 属性。

单击登录 LinkButton 导致回发，从其重定向颁发给登录页。 单击注销 LinkButton 导致 LoginStatus 控件调用 FormsAuthentication.SignOff 方法，然后将用户重定向到页。 登录页上关闭用户已重定向至取决于 LogoutAction 属性，可以分配给三个以下值之一：

- 刷新的默认设置;将用户重定向到他们只需访问的页面。 如果他们只需访问的页面不允许匿名用户，然后 FormsAuthenticationModule 将自动将用户重定向到登录页。

你可能想有关为什么重定向这里未执行。 如果用户想要保留在同一页上，为什么需要显式重定向？ 原因是因为在用户单击后注销 LinkButton，cookie 集合中的窗体身份验证票证仍有。 因此，在回发请求是已经过身份验证的请求。 LoginStatus 控件调用注销方法，但 FormsAuthenticationModule 具有用户进行身份验证后将发生这种情况。 因此，显式重定向导致浏览器来重新请求该页。 通过浏览器重新请求页时，窗体身份验证票证已删除，因此传入请求是匿名。

- 重定向的用户将重定向到 LoginStatus LogoutPageUrl 属性指定的 URL。
- RedirectToLoginPage-用户将重定向到登录页。

让我们 LoginStatus 控件添加到母版页和将其配置为使用重定向选项将用户发送到显示一条消息，确认，它们已注销的页。首先创建名为 Logout.aspx 的根目录中的页。 不要忘记将此页与母版页 Site.master 相关联。 接下来，向它们已被注销用户解释的页面的标记中输入一条消息。

接下来，返回到 Site.master 主控页并按 LoginContent ContentPlaceHolder 添加下 LoginView LoginStatus 控件。 将重定向到的 LoginStatus 控件的 LogoutAction 属性和其 LogoutPageUrl 属性设置为 ~/Logout.aspx。

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

由于 LoginStatus 是在 LoginView 控件外，它将显示对于匿名和经过身份验证的用户，但这是确定，因为登录或注销 LinkButton LoginStatus 将正确显示。 LoginStatus 控件添加后，日志中的超链接 AnonymousTemplate 多余，因此将其删除。

图 18 显示 Default.aspx 时 Jisun 访问。 请注意左侧的列显示的消息，欢迎回来，并提供一个链接 Jisun 以注销。单击注销 LinkButton 导致回发，Jisun 注销系统，然后将她重定向到 Logout.aspx。 如图 19 所示，通过时间 Jisun 达到的 Logout.aspx 她已注销，并因此是匿名的时间。 因此，左侧的列显示欢迎，陌生人并链接到登录页的文本。


[![Default.aspx 显示欢迎回来，以及注销 LinkButton Jisun](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**图 18**: Default.aspx 显示欢迎回来，Jisun 沿与注销 LinkButton ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx 显示欢迎，以及登录 LinkButton 陌生人](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**图 19**: Logout.aspx 显示欢迎，以及登录 LinkButton 陌生人 ([单击以查看实际尺寸的图像](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> 我建议你自定义 Logout.aspx 页后，可以隐藏母版页的 LoginContent ContentPlaceHolder （例如，我们为 Login.aspx 在步骤 4 中所做的那样）。 原因是因为由 LoginStatus 控件呈现登录 LinkButton (Hello 下, 一个陌生人) 将用户发送到 ReturnUrl 查询字符串参数中传递的当前 URL 的登录页。 简单地说，如果已注销的用户单击此 LoginStatus 登录 LinkButton，然后登录，则他们将被重定向回 Logout.aspx，无法将用户轻松地混淆。


## <a name="summary"></a>摘要

在本教程中我们开始使用窗体身份验证工作流的检查，然后打开到 ASP.NET 应用程序中实现窗体身份验证。 窗体身份验证采用 FormsAuthenticationModule，它具有以下两项职责： 标识用户根据其窗体身份验证票证，并将未经授权的用户重定向到登录页。

.NET Framework 的 FormsAuthentication 类包括用于创建、 检查和删除窗体身份验证票证的方法。 Request.IsAuthenticated 属性和用户对象提供用于确定是否对请求进行身份验证和有关用户的标识信息的其他编程支持。 也有 LoginView、 LoginStatus 和 LoginName Web 控件，为开发人员提供了一种快速、 无需编码的方法执行许多常见登录相关的任务。 在将来的教程中，我们将检查这些和其他登录名相关的 Web 控件中更多详细信息。

本教程提供窗体身份验证的粗略概述。 我们未检查的各种的配置选项，查看如何无 cookie 窗体身份验证票证工作，或浏览 ASP.NET 保护窗体身份验证票证的内容的方式。 我们将讨论这些主题以及详细信息中[下一教程](forms-authentication-configuration-and-advanced-topics-vb.md)。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [Iis 6 和 IIS7 安全之间的更改](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [登录 ASP.NET 控件](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [专业 ASP.NET 2.0 安全、 成员资格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [&lt;身份验证&gt;元素](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;窗体&gt;元素&lt;身份验证&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教程中包含的主题的视频培训

- [在 ASP.NET 中使用基本 Forms 身份验证](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者包括 Alicja Maziarz、 John Suru 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)。

>[!div class="step-by-step"]
[上一页](security-basics-and-asp-net-support-vb.md)
[下一页](forms-authentication-configuration-and-advanced-topics-vb.md)
