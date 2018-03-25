---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: "创建用户帐户 (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将探讨使用成员资格框架 （通过 SqlMembershipProvider) 来创建新用户帐户。 我们将了解如何创建新我们..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 61621ffaae98ac74c16b2ff014ba9d85c2c10b3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-user-accounts-vb"></a>创建用户帐户 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip)或[下载 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> 在本教程中我们将探讨使用成员资格框架 （通过 SqlMembershipProvider) 来创建新用户帐户。 我们将了解如何以编程方式和通过 ASP 创建新用户。NET 的内置通过。


## <a name="introduction"></a>介绍

在<a id="_msoanchor_1"> </a>[前面教程](creating-the-membership-schema-in-sql-server-vb.md)我们在数据库中，也不能添加表、 视图、 存储过程所需的安装应用程序服务架构`SqlMembershipProvider`和`SqlRoleProvider`。 这将创建我们将需要这一系列中的教程的其余部分的基础结构。 在本教程中我们将探讨使用成员资格框架 (通过`SqlMembershipProvider`) 来创建新用户帐户。 我们将了解如何以编程方式和通过 ASP 创建新用户。NET 的内置通过。

除了学习如何创建新的用户帐户，我们将还需要更新我们首先创建中的演示网站 *<a id="_msoanchor_2"> </a>[概述窗体身份验证的](../introduction/an-overview-of-forms-authentication-vb.md)*教程，然后中增强 *<a id="_msoanchor_3"> </a>[窗体身份验证配置和高级主题](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*教程。 我们演示 web 应用程序登录页，用于验证针对硬编码的用户名/密码对用户的凭据。 此外，`Global.asax`包括创建自定义代码`IPrincipal`和`IIdentity`为经过身份验证的用户的对象。 我们将更新登录页后，可以验证用户的凭据针对成员资格框架和删除的自定义的主体和标识逻辑。

让我们进入正题！

## <a name="the-forms-authentication-and-membership-checklist"></a>窗体身份验证和成员身份清单

我们开始使用成员资格 framework 之前，让我们花些时间回顾我们采取了来达到此点的重要步骤。 使用的成员资格 framework 时`SqlMembershipProvider`需要在基于窗体的身份验证方案中，在 web 应用程序中实现成员资格功能之前执行以下步骤：

1. **启用基于窗体的身份验证。** 如我们所述 *<a id="_msoanchor_4"> </a>[概述窗体身份验证的](../introduction/an-overview-of-forms-authentication-vb.md)*，窗体身份验证通过编辑`Web.config`和设置`<authentication>`元素的`mode`属性设为`Forms`。 通过启用 forms 身份验证，每个传入请求检查，以确定*窗体身份验证票证*，它，如果存在，标识请求者。
2. **将应用程序服务架构添加到相应的数据库。** 使用时`SqlMembershipProvider`我们需要先安装到数据库的应用程序服务架构。 通常将此架构添加到同一个数据库包含应用程序的数据模型。 *<a id="_msoanchor_5"> </a>[在 SQL Server 中创建成员身份架构](creating-the-membership-schema-in-sql-server-vb.md)*教程了解了使用`aspnet_regsql.exe`工具来实现此目的。
3. **自定义 Web 应用程序的设置以从步骤 2 中引用该数据库。** *在 SQL Server 中创建成员身份架构*教程介绍了两种方法可以配置 web 应用程序，以便`SqlMembershipProvider`将使用在步骤 2 中所选的数据库： 通过修改`LocalSqlServer`连接字符串名称;或通过将新的已注册提供程序添加到成员资格 framework 提供程序的列表和自定义该新的提供程序，用于将数据库从步骤 2。

当生成 web 应用程序，该使用`SqlMembershipProvider`和基于窗体的身份验证，你将需要使用之前执行以下三个步骤`Membership`类或 ASP.NET 登录 Web 控件。 由于我们已在前面的教程中执行这些步骤，我们已准备好开始使用成员资格 framework ！

## <a name="step-1-adding-new-aspnet-pages"></a>步骤 1： 添加新的 ASP.NET 页

我们将在本教程和下一步的三个检查各种与成员资格相关的函数和功能。 我们将需要一系列的 ASP.NET 网页来实现检查整个这些教程的主题。 让我们来创建这些页面，然后站点映射文件`(Web.sitemap)`。

首先创建一个新文件夹中名为的项目`Membership`。 接下来，添加到五个新的 ASP.NET 页`Membership`文件夹中，链接与每个页面`Site.master`母版页。 命名页：

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

此时你的项目的解决方案资源管理器应类似于的屏幕快照中图 1 所示。


[![新的 5 个页面已添加到成员资格文件夹](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**图 1**： 五个新页已添加到`Membership`文件夹 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image3.png))


每一页后，应该在此情况下，有两个内容控件，每个母版页的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

回想一下， `LoginContent` ContentPlaceHolder 的默认标记显示在登录或注销的站点，具体取决于是否对用户进行身份验证的链接。 是否存在`Content2`内容控件中，但是，将覆盖主页面的默认标记。 如我们所述 *<a id="_msoanchor_6"> </a>[概述窗体身份验证的](../introduction/an-overview-of-forms-authentication-vb.md)*教程中，这可在页中，我们不希望左侧列中显示与登录相关的选项。

有关这些的 5 个页面，但是，我们希望能够显示主控页的默认标记`LoginContent`ContentPlaceHolder。 因此，删除的声明性标记`Content2`内容控件。 这么做，每个五个页面的标记应包含几个内容控件。

## <a name="step-2-creating-the-site-map"></a>步骤 2： 创建站点图

除了最不重要的网站的所有需要实施某种形式的导航用户界面。 导航用户界面可能指向站点的各个部分的简单列表。 或者，这些链接可能安排到菜单或树视图。 页开发人员创建导航用户界面是仅有一半情景。 我们还需要一些方法来定义可维护性和可更新的方式的站点的逻辑结构。 在新页面添加或删除现有的页时，我们想要能够更新单个源-站点图-并这些修改反映跨站点的导航用户界面。

这两个任务的定义站点图和实现导航用户界面基于站点代码图-可轻松地完成站点图 framework 感谢和导航 Web 控件中添加的 ASP.NET 2.0 版。 站点图框架允许开发人员用来定义站点图，然后通过编程 API 访问它 ( [ `SiteMap`类](https://msdn.microsoft.com/library/system.web.sitemap.aspx))。 导航 Web 控件的内置包括[菜单控件](https://msdn.microsoft.com/library/bz09dy46.aspx)、 [TreeView 控件](https://msdn.microsoft.com/library/3eafky27.aspx)，和[SiteMapPath 控件](https://msdn.microsoft.com/library/3eafky27.aspx)。

成员资格和角色的框架，如站点图 framework 生成之上[提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)。 站点图提供程序类的作业是生成使用的内存中结构`SiteMap`从永久性数据存储，如 XML 文件或数据库表中的类。 .NET Framework 附带的默认站点图提供程序从 XML 文件读取站点地图数据 ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx))，并且这是我们将在本教程中使用的提供程序。 对于某些备用的站点图提供程序实现，请参阅进一步读数部分在本教程末尾。

默认站点图提供程序所需的格式正确的 XML 文件，名为`Web.sitemap`存在的根目录。 因为我们要使用此默认的提供程序，我们需要添加此类文件和定义站点图结构以正确的 XML 格式。 若要添加文件时，右键单击解决方案资源管理器中的项目名称，并选择添加新项。 从对话框中，选择要添加的类型名为的站点图文件`Web.sitemap`。


[![添加名为 Web.sitemap 到项目的根目录的文件](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**图 2**： 添加文件命名为`Web.sitemap`到项目的根目录 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image6.png))


XML 映射文件定义为层次结构的网站的结构。 此层次结构关系建模在 XML 文件中的祖先通过`<siteMapNode>`元素。 `Web.sitemap`必须以开头`<siteMap>`精确有父节点`<siteMapNode>`子。 此顶层`<siteMapNode>`元素表示的根的层次结构，并可能包含任意数目的子代节点。 每个`<siteMapNode>`元素必须包含`title`属性，并可以根据需要包含`url`和`description`属性，以及其他; 每个非空`url`必须是唯一的属性。

输入以下 XML`Web.sitemap`文件：

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

上面的站点地图标记定义图 3 所示的层次结构。


[![站点图表示分层导航结构](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**图 3**： 站点图表示分层导航结构 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>步骤 3： 更新 Master 页后，可以包括导航用户界面

ASP.NET 包括大量导航相关器设计用户界面的 Web 控件。 其中包括菜单、 树视图和 SiteMapPath 控件。 菜单和 TreeView 控件呈现菜单或树中，在站点地图结构分别而 SiteMapPath 显示显示当前正在访问以及其祖先节点痕迹导航。 站点地图数据可以绑定到 Web 控件使用 SiteMapDataSource 其他数据，可以通过以编程方式访问`SiteMap`类。

由于站点图 framework 和导航控件的全面讨论超出了本系列教程的范围，而不是花时间来让我们创建我们自己导航用户界面改为借用在中使用我 *[在 ASP.NET 2.0 中使用数据](../../data-access/index.md)*教程系列，使用转发器控件来显示导航链接的两个深层项目符号列表中图 4 所示。

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>在左侧列中添加两个级别的链接列表

若要创建此接口，添加到以下声明性标记`Site.master`主控页的左列其中文本 TODO： 菜单将转到此处...当前位于。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

上面的标记将名为转发器控件绑定`menu`到 SiteMapDataSource，它返回站点映射层次结构中定义`Web.sitemap`。 由于 SiteMapDataSource 控件的[`ShowStartingNode`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)设置为 False 启动返回开头的主节点的子代的站点图的层次结构。 转发器显示中每个这些节点 （当前仅成员资格）`<li>`元素。 另一个内部中继器中将显示当前节点的子级嵌套未经排序的列表。

图 4 显示上述标记的呈现的输出与我们在步骤 2 中创建站点地图结构。 转发器呈现香草未经排序的列表标记;在中定义的级联样式表规则`Styles.css`负责的审美情趣布局。 有关上述标记的工作原理的更多详细说明，请参阅[母版页和网站的导航](https://asp.net/learn/data-access/tutorial-03-vb.aspx)教程。


[![导航用户界面是呈现使用嵌套无序列表](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**图 4**: 导航用户界面是呈现使用嵌套无序列出 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>添加痕迹导航

除了左侧列中的链接列表，让我们还具有每个页面显示[痕迹导航](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29)。 痕迹导航是快速向用户显示其站点层次结构中的当前位置的导航用户界面元素。 说明如何使用站点图 framework 来确定站点图中的当前页的位置，然后显示痕迹基于此信息。

具体而言，添加`<span>`到母版页的标头元素`<div>`元素，并设置新`<span>`元素的`class`痕迹导航属性。 (`Styles.css`类包含用于痕迹导航类的规则。)接下来，添加到此新的 SiteMapPath`<span>`元素。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

图 5 显示的 SiteMapPath 输出时访问`~/Membership/CreatingUserAccounts.aspx`。


[![痕迹导航显示当前页，并在站点中的其上级映射](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**图 5**： 痕迹导航站点地图中显示当前页和其祖先 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>步骤 4： 删除的自定义主体和标识逻辑

在 *<a id="_msoanchor_7"> </a>[窗体身份验证配置和高级主题](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*我们已了解如何将向经过身份验证的用户的自定义主体和标识对象关联的教程。 我们实现了这一点通过创建事件处理程序中的`Global.asax`为应用程序的`PostAuthenticateRequest`后触发的事件`FormsAuthenticationModule`已经身份验证的用户。 在此事件处理程序中，我们替换`GenericPrincipal`和`FormsIdentity`通过添加对象`FormsAuthenticationModule`与`CustomPrincipal`和`CustomIdentity`对象我们在该教程中创建。

而自定义的主体和标识对象是在某些情况下，在大多数情况下有用`GenericPrincipal`和`FormsIdentity`对象已经足够。 因此，我认为很值得以返回到默认行为。 通过删除或注释掉进行此更改`PostAuthenticateRequest`事件处理程序或通过删除`Global.asax`完全文件。

> [!NOTE]
> 一旦已注释或删除中的代码`Global.asax`，你将需要注释掉中的代码`Default.aspx's`将强制转换的代码隐藏类`User.Identity`属性`CustomIdentity`实例。


## <a name="step-5-programmatically-creating-a-new-user"></a>步骤 5： 以编程方式创建新用户

若要创建新的用户帐户通过使用的成员资格 framework`Membership`类的[`CreateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)。 此方法的输入的用户名、 密码和其他与用户相关的字段的参数。 在调用，它将新的用户帐户创建委托给配置成员资格提供程序，然后返回[`MembershipUser`对象](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)表示刚创建的用户帐户。

`CreateUser`方法有四个重载，每个文本框接受不同数量的输入参数：

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

在收集信息的量上不同这些四个重载。 第一个重载，例如，需要只是用户名和密码对于新的用户帐户，而第二个还要求用户的电子邮件地址。

这些重载存在，因为创建新的用户帐户所需的信息取决于成员资格提供程序的配置设置。 在 *<a id="_msoanchor_8"> </a>[在 SQL Server 中创建成员身份架构](creating-the-membership-schema-in-sql-server-vb.md)*教程，我们探讨了中的指定成员资格提供程序配置设置`Web.config`。 表 2 包含的配置设置的完整列表。

一个此类成员资格提供程序配置设置会影响什么`CreateUser`可能使用重载是`requiresQuestionAndAnswer`设置。 如果`requiresQuestionAndAnswer`设置为`true`（默认值），然后创建新的用户帐户时我们必须指定一个安全提示问题和答案。 如果用户需要进行重置或更改其密码，更高版本使用此信息。 具体而言，此时，它们显示安全问题，并且他们必须输入正确答案若要重置或更改其密码。 因此，如果`requiresQuestionAndAnswer`设置为`true`然后调用前两个任一`CreateUser`重载会引发异常，因为安全问题和答案缺失。 由于我们的应用程序当前配置为需要安全问题和答案，我们将需要时以编程方式创建用户的使用后一种两个重载之一。

若要演示如何使用`CreateUser`方法，让我们创建我们其中提示用户输入其名称、 密码、 电子邮件、 和到预定义的安全问题的答案的用户界面。 打开`CreatingUserAccounts.aspx`页面`Membership`文件夹并将以下 Web 控件添加到内容控件：

- 名为文本框中`Username`
- 文本框中，名为`Password`、 其`TextMode`属性设置为`Password`
- 名为文本框中`Email`
- 一个名为标签`SecurityQuestion`与其`Text`属性已清除
- 名为文本框中`SecurityAnswer`
- 名为的按钮`CreateAccountButton`其`Text`属性设置为创建用户帐户
- 一个名为的标签控件`CreateAccountResults`与其`Text`属性已清除

此时你的屏幕应类似于的屏幕快照中图 6 所示。


[![将各种 Web 控件添加到 CreatingUserAccounts.aspx 页](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**图 6**: 各种 Web 将控件添加到`CreatingUserAccounts.aspx Page`([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image18.png))


`SecurityQuestion`标签和`SecurityAnswer`文本框中旨在显示预定义的安全问题并收集用户的答案。 请注意，安全问题和答案存储基于用户的用户，因此很可能允许每个用户定义其自己的安全问题。 但是，对于我已决定使用一个通用安全问题，即此示例： 什么是您喜爱的颜色？

若要实现此预定义的安全问题，请将常量添加到页面的代码隐藏类名为`passwordQuestion`，将其分配安全问题。 然后，在`Page_Load`事件处理程序中，分配到此常量`SecurityQuestion`标签的`Text`属性：

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

接下来，创建的事件处理程序`CreateAccountButton'`s`Click`事件并添加以下代码：

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click`事件处理程序启动通过定义一个名为变量`createStatus`类型的[ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx)。 `MembershipCreateStatus`是一个枚举，指示的状态`CreateUser`操作。 例如，如果用户成功创建帐户后，生成`MembershipCreateStatus`实例将设置为值`Success;`另一方面，如果操作失败，因为已存在具有相同的用户名的用户，它将设置为值为`DuplicateUserName`. 在`CreateUser`我们使用的重载，我们需要将`MembershipCreateStatus`到方法的实例。 此参数设置为适当的值中`CreateUser`方法，并且我们可以在方法调用，以确定是否已成功创建用户帐户后检查其值。

在调用`CreateUser`，并传入`createStatus`、`Select Case`语句用于输出赋予的值根据相应的消息`createStatus`。 已成功创建新用户时，图 7 显示输出。 未能创建用户帐户时，数字 8 和 9 显示的输出。 在图 8 中，在距访客输入了不满足密码强度要求拼在成员资格提供程序的配置设置中的五个字母密码。 在图 9 中，在距访客正在尝试使用现有的用户名 （图 7 中创建一个） 创建用户帐户。


[![新的用户帐户是已成功创建](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**图 7**: 新用户帐户是已成功创建 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image21.png))


[![创建不用户帐户，因为提供的密码为太弱](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**图 8**: 提供的密码强度太弱因为未创建用户帐户 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image24.png))


[![用户帐户是不创建因为用户名已在使用中](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**图 9**: 用户帐户是不创建因为用户名已在使用 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> 你可能想知道如何确定成功或失败时使用前两个之一`CreateUser`方法重载，两者的它具有的类型参数`MembershipCreateStatus`。 这些前两个重载会引发[`MembershipCreateUserException`异常](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx)在发生故障，其中包括[`StatusCode`属性](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx)类型的`MembershipCreateStatus`。


在创建后的几个用户帐户，验证是否已通过列出的内容创建了帐户`aspnet_Users`和`aspnet_Membership`中的表`SecurityTutorials.mdf`数据库。 如图 10 显示，但我添加了两个用户通过`CreatingUserAccounts.aspx`页： Tito 和罗斯向她。


[![在成员资格用户存储区中有两个用户： Tito 和罗斯向她](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**图 10**： 在成员资格用户存储区中有两个用户： Tito 和罗斯向她 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image30.png))


虽然成员资格用户存储现在包含罗斯向她和 Tito 的帐户的信息，我们还实现允许进行罗斯向她或 Tito 登录到站点的功能。 目前，`Login.aspx`验证用户的凭据针对一组硬编码用户名/密码对-与其*不*验证针对成员资格框架提供的凭据。 现在出现在新的用户帐户的`aspnet_Users`和`aspnet_Membership`表将需要满足要求。 在下一步的教程中，  *<a id="_msoanchor_9"> </a>[验证用户凭据对成员资格用户存储](validating-user-credentials-against-the-membership-user-store-vb.md)*，我们将更新成员资格存储针对验证的登录页。

> [!NOTE]
> 如果你看不到内的任何用户你`SecurityTutorials.mdf`数据库，则可能是因为 web 应用程序正在使用默认成员资格提供程序， `AspNetSqlMembershipProvider`，它使用`ASPNETDB.mdf`作为其用户存储区的数据库。 若要确定如果出现此问题，请单击解决方案资源管理器中的刷新按钮。 如果一个名为数据库`ASPNETDB.mdf`已添加到`App_Data`文件夹中，出现此问题。 返回到的第 4 步 *<a id="_msoanchor_10"> </a>[在 SQL Server 中创建成员身份架构](creating-the-membership-schema-in-sql-server-vb.md)*教程有关如何正确配置成员资格提供程序的说明。


在大多数创建用户帐户方案、 在距访客出现某些接口输入他们的用户名、 密码、 电子邮件和其他重要信息，此时创建新的帐户。 在此步骤中，我们看手动生成此类接口，然后了解了如何使用`Membership.CreateUser`方法以编程方式添加新的用户帐户基于用户的输入。 但是，我们的代码，只需创建新的用户帐户。 它没有执行任何跟进操作，如用户访问刚创建的用户帐户，该站点中的日志记录或向用户发送确认电子邮件。 这些附加步骤将需要附加代码按钮的`Click`事件处理程序。

ASP.NET 附带了用于处理从呈现用户界面，用于创建新的用户帐户，在成员资格框架中创建帐户并执行后的帐户的用户帐户创建过程，通过创建任务，例如发送确认电子邮件和刚创建的用户登录到网站。 使用通过只需从工具箱拖到页面上，拖动通过，然后设置的几个属性。 在大多数情况下，你不需要编写一行代码。 我们将探讨此极好的控件处于第 6 步中的详细信息。

如果通过典型的创建帐户网页仅创建新的用户帐户，它不太曾经需要编写代码，使用`CreateUser`方法，通过为可能满足您的需求。 但是，`CreateUser`需要提供高度自定义创建帐户的用户体验，或者当您需要以编程方式创建新的用户帐户，通过替代接口，方法是在方案中非常方便。 例如，你可能必须允许用户上载 XML 文件，其中包含从某个其他应用程序的用户信息的页。 页可能会的分析已上载的 XML 的内容文件，并在 XML 中创建表示每个用户的新帐户，通过调用`CreateUser`方法。

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>步骤 6: 通过使用创建新用户

ASP.NET 附带了大量的登录 Web 控件。 这些控件有助于许多常见用户帐户和登录名相关方案。 [通过](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)一个此类旨在提供用于将新的用户帐户添加到成员资格 framework 的用户界面的控件。

很多其他登录名相关的 Web 控件，如 CreateUserWizard 可用而无需编写一行代码。 它直观地提供用户界面基于成员资格提供程序的配置设置和内部调用`Membership`类的`CreateUser`方法后用户输入所需的信息并单击创建用户按钮。 通过可极自定义。 有一个主机为帐户创建过程的各个阶段激发的事件。 根据需要帐户创建工作流中插入自定义逻辑，我们可以创建事件处理程序。 此外，CreateUserWizard 的外观会非常灵活。 有多个定义的默认接口; 外观的属性如有必要，该控件可以转换为模板，或可能添加其他用户注册步骤。

让我们开始看一下使用通过默认接口和行为。 然后，我们将探讨如何自定义控件的属性和事件通过外观。

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>检查 CreateUserWizard 的默认接口和行为

返回到`CreatingUserAccounts.aspx`页面`Membership`文件夹中，切换到设计或拆分模式中，并将通过添加到页面顶部。 通过在工具箱的登录控件部分下归档。 在添加控件后, 设置其`ID`属性`RegisterUser`。 图 11 显示中的屏幕截图，如 CreateUserWizard 呈现为新用户的用户名、 密码、 电子邮件地址和安全问题和答案的文本框中的接口。


[![通过呈现泛型创建用户界面](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**图 11**: 通过呈现泛型的创建用户界面 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image33.png))


让我们花些时间比较由通过与我们在步骤 5 中创建的接口生成的默认用户界面。 对于初学者，通过允许访问者来指定安全问题和答案，而我们手动创建接口使用预定义的安全问题。 通过接口还包括验证控件，而我们不得不尚未在我们接口的窗体字段上实现验证。 并 CreateUserWizard 控件接口包含 （以及以确保输入密码的文本和比较密码文本框中相等 CompareValidator) 确认密码文本框。

有趣的是通过向成员资格提供程序的配置设置，在其用户界面进行呈现时。 例如，安全问题和答案文本框中仅显示如果`requiresQuestionAndAnswer`设置为 True。 同样，CreateUserWizard 会自动添加 RegularExpressionValidator 控件，以确保密码强度要求满足，并设置其`ErrorMessage`和`ValidationExpression`属性基于`minRequiredPasswordLength`， `minRequiredNonalphanumericCharacters`，和`passwordStrengthRegularExpression`配置设置。

通过，正如其名，派生自[向导控件](https://msdn.microsoft.com/library/s2etd1ek.aspx)。 向导控件被设计用于提供用于完成多步骤任务的接口。 向导控件可能包含任意数目的`WizardSteps`，其中每个是一个模板，定义 HTML 和 Web 控件为该步骤。 向导控件最初显示第一个`WizardStep`，以及允许用户可从某一步前进到下一行，或返回到前面的步骤的导航控件。

如图 11 中的声明性标记所示，通过默认接口包括两个`WizardStep`s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? 呈现的界面，以收集用于创建新的用户帐户的信息。 这是图 11 中所示的步骤。
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? 呈现，该值指示该帐户已成功创建一条消息。

可以修改 CreateUserWizard 的外观和行为，通过将上述任一步骤转换为模板，或添加自己`WizardStep`s。 我们将考察添加`WizardStep`到中的注册界面*存储的其他用户信息*教程。

我们来看通过在操作中。 请访问`CreatingUserAccounts.aspx`通过浏览器的页。 首先到 CreateUserWizard 的界面中输入一些无效的值。 请尝试输入与密码强度要求，不符合密码或将用户名文本框中保留为空。 CreateUserWizard 将显示相应的错误消息。 图 12 显示输出时尝试创建具有不足强密码的用户。


[![CreateUserWizard 自动插入验证控件](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**图 12**: CreateUserWizard 自动插入验证控件 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image36.png))


接下来，输入 CreateUserWizard 适当的值，然后单击创建用户按钮。 假设已输入必填的字段，并且密码的强度就足够了，CreateUserWizard 将创建新的用户帐户通过成员资格框架，然后显示`CompleteWizardStep`的接口 （请参阅图 13）。 在后台，CreateUserWizard 调用`Membership.CreateUser`方法，就像我们未在步骤 5 中一样。


[![新的用户帐户已成功创建](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**图 13**: 新的用户帐户已成功创建 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> 如图 13 所示，`CompleteWizardStep`的接口包含一个继续按钮。 但是，此时只需单击以执行回发时，将在距访客保留同一页上。 在自定义 CreateUserWizard 的外观和行为通过及其属性部分我们着眼于如何可以发送到在距访客此按钮`Default.aspx`（或某些其他页）。


创建新的用户帐户之后, 返回到 Visual Studio 和检查`aspnet_Users`和`aspnet_Membership`表像图 10，若要验证是否已成功创建帐户。

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>自定义 CreateUserWizard 的行为和外观通过其属性

中有许多种情况下，通过属性，可以自定义 CreateUserWizard `WizardStep` s 和事件处理程序。 在本部分中我们将探讨如何自定义控件的外观，通过其属性：下一节查看扩展通过事件处理程序的控件的行为。

几乎所有通过的默认用户界面中显示的文本可以通过其各种属性自定义。 例如，文本框中的左侧显示的用户名称、 密码、 确认密码、 电子邮件、 安全问题和安全答案标签也可以通过自定义[ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx)， [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx)， [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx)， [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx)， [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)，和[ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)属性，分别。 同样，没有用于指定中的创建用户并继续按钮的文本属性`CreateUserWizardStep`和`CompleteWizardStep`，也就像这些按钮将呈现为按钮、 LinkButtons 或 ImageButtons。

颜色、 边框、 字体和其他可视元素是可通过样式属性的主机配置。 通过本身具有常见 Web 控件的样式属性- `BackColor`， `BorderStyle`， `CssClass`， `Font`，依次类推中-并且有多个用于定义的特定部分的外观的样式属性CreateUserWizard 的接口。 [ `TextBoxStyle`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx)，例如，定义在文本框中的样式`CreateUserWizardStep`，虽然[`TitleTextStyle`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx)定义 （注册为你新标题的样式帐户）。

除了与外观相关的属性，有多个属性可以影响通过的行为。 [ `DisplayCancelButton`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx)，如果设置为 True，将显示取消按钮旁边的创建用户按钮 （默认值为 False）。 如果显示取消按钮，请务必也将设置[`CancelDestinationPageUrl`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)，其中指定后单击取消将用户发送到的页。 在上一节中的继续按钮中所述`CompleteWizardStep`的接口，导致回发，但将在距访客留在同一页上。 若要发送到其他页面上访问者，单击继续按钮后，只需指定中的 URL [ `ContinueDestinationPageUrl`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)。

让我们更新`RegisterUser`通过显示取消按钮并将发送到在距访客`Default.aspx`时单击取消或继续按钮。 若要完成此操作，将设置`DisplayCancelButton`属性设置为 True 且两`CancelDestinationPageUrl`和`ContinueDestinationPageUrl`属性设置为 ~ / Default.aspx。 图 14 显示更新的 CreateUserWizard 通过浏览器查看时。


[![CreateUserWizardStep 包括取消按钮](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**图 14**:`CreateUserWizardStep`包括取消按钮 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image42.png))


当访问者输入用户名、 密码、 电子邮件地址和安全问题和答案，并单击创建的用户时，创建一个新的用户帐户和访问者在中记录为该新创建的用户。 假定可访问的页面的人员为自己创建新帐户，这可能是所需的行为。 但是，你可能想要允许管理员添加新用户帐户。 在此情况下，将创建用户帐户，但管理员将保持在已登录，以管理员身份 （而不是新创建的帐户）。 此行为可以修改通过布尔[`LoginCreatedUser`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)。

成员资格 framework 中的用户帐户包含一个已批准的标志;未获批准的用户将无法登录到网站。 默认情况下，新创建的帐户是标记为已获得批准，允许用户立即登录到网站。 它有可能，但是，具有标记为未批准的新用户帐户。 您可能需要管理员手动批准的新用户之前用户可以登录;或者，你可能想要验证允许用户登录之前在注册输入的电子邮件地址有效。 这种情况可能存在的内容，你可以通过设置通过标记为未批准的新创建的用户帐户[`DisableCreatedUser`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)为 True （默认值为 False）。

注意的其他行为相关的属性包括`AutoGeneratePassword`和`MailDefinition`。 如果[`AutoGeneratePassword`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx)设置为 True，`CreateUserWizardStep`不显示密码和确认密码文本框中; 相反，新创建用户的密码自动生成使用`Membership`类的[`GeneratePassword`方法](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)。 `GeneratePassword`方法构造具有指定长度的和使用足够数量的非字母数字字符，以满足配置的密码强度要求的密码。

[ `MailDefinition`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)如果你想要在帐户创建过程中指定的电子邮件地址发送一封电子邮件将非常有用。 `MailDefinition`属性包含一系列的子属性，用于定义有关构造电子邮件消息的信息。 这些子属性包括诸如以下选项`Subject`， `Priority`， `IsBodyHtml`， `From`， `CC`，和`BodyFileName`。 [ `BodyFileName`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)指向文本或 HTML 文件，其中包含电子邮件的正文。 正文支持两个预定义的占位符：`<%UserName%>`和`<%Password%>`。 这些占位符，如果存在于`BodyFileName`文件中，将替换为刚创建用户的名称和密码。

> [!NOTE]
> `CreateUserWizard`控件的`MailDefinition`属性只指定当创建新帐户发送电子邮件消息的详细信息。 它不包含任何实际发送电子邮件方式的详细信息 （即，是否使用 SMTP 服务器或邮件投递目录、 任何身份验证信息和等等）。 需要在中定义这些低级别的详细信息`<system.net>`主题中`Web.config`。 有关这些配置设置和一般情况下从 ASP.NET 2.0 发送电子邮件的详细信息，请参阅[在 SystemNetMail.com 常见问题](http://www.systemnetmail.com/)和我文章[ASP.NET 2.0 中发送电子邮件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)。


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>扩展 CreateUserWizard 的行为使用事件处理程序

通过其工作流期间引发事件的数。 例如，访问者输入他们的用户名、 密码和其他相关信息并单击创建用户按钮后，通过引发其[`CreatingUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)。 如果在创建过程中，问题[`CreateUserError`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)激发; 但是，如果已成功创建用户，则[`CreatedUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)引发。 获取引发的其他 CreateUserWizard 控件事件，但这些是三个最 germane 的。

在某些情况下我们可能想要点击流入 CreateUserWizard 流中，我们可以通过创建相应的事件的事件处理程序执行操作。 为了说明这一点，让我们增强`RegisterUser`通过以包含一些自定义验证的用户名和密码。 具体而言，让我们增强我们 CreateUserWizard，以便用户名不能包含前导空格或尾随空格和用户名不能在密码中任意位置出现如此。 简单地说，我们想要防止某人创建的用户名与"Scott"，或都有 Scott 等 Scott.1234 用户名/密码组合。

若要完成此我们将创建的事件处理程序`CreatingUser`事件以执行我们的额外验证检查。 如果提供的数据不是有效我们需要取消创建过程。 我们还需要将标签 Web 控件添加到页后，可以显示一条消息说明的用户名或密码无效。 首先，通过添加标签控件下通过，设置其`ID`属性`InvalidUserNameOrPasswordMessage`及其`ForeColor`属性`Red`。 清除其`Text`属性并设置其`EnableViewState`和`Visible`属性为 False。

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

接下来，创建的事件处理程序通过`CreatingUser`事件。 若要创建一个事件处理程序，在设计器中选择控件，然后转到属性窗口。 从这里，单击闪电形图标，然后双击相应的事件来创建事件处理程序。

将以下代码添加到 `CreatingUser` 事件处理程序中：

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

请注意，用户名和密码输入到通过是可通过其[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)和[`Password`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)分别。 我们可以使用在上面的事件处理程序中的这些属性以确定提供的用户名是否包含前导空格或尾随空格，并且密码中找到的用户名。 如果满足这些条件之一，则一条错误消息显示在`InvalidUserNameOrPasswordMessage`标签和事件处理程序的`e.Cancel`属性设置为`True`。 如果`e.Cancel`设置为`True`，CreateUserWizard 会使短路其工作流，有效地取消用户帐户创建过程。

图 15 所示的屏幕截图`CreatingUserAccounts.aspx`当用户输入的用户名与前导空格。


[![不允许以前导或尾随空格的用户名](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**图 15**： 不允许以前导或尾随空格的用户名 ([单击以查看实际尺寸的图像](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> 我们将看到举例说明使用通过`CreatedUser`中的事件 *<a id="_msoanchor_11"> </a>[存储的其他用户信息](storing-additional-user-information-vb.md)*教程。


## <a name="summary"></a>摘要

`Membership`类的`CreateUser`方法中的成员资格 framework 创建新的用户帐户。 它会通过委派对配置成员资格提供程序的调用。 情况下`SqlMembershipProvider`、`CreateUser`方法将添加到记录`aspnet_Users`和`aspnet_Membership`数据库表。

虽然可以创建新的用户帐户，以编程方式 （如我们看到在步骤 5 中），更快且更容易的方法是使用通过。 此控件呈现用于收集用户信息并在成员资格框架中创建新用户的多步骤用户界面。 下方底层，此控件使用相同`Membership.CreateUser`方法检查在步骤 5 中，但该控件中作为创建用户界面，验证控件，并对用户帐户创建错误而无需编写缺乏对代码做出响应。

此时我们的位置，以创建新用户帐户具有的功能。 但是，登录页仍验证针对进来第二个教程，我们指定了这些硬编码的凭据。 在<a id="_msoanchor_12"> </a>[下一教程](validating-user-credentials-against-the-membership-user-store-vb.md)我们将更新`Login.aspx`以验证此用户的提供针对成员资格框架的凭据。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [`CreateUser`技术文档](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [通过概述](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [创建文件系统基于站点映射提供程序](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [使用 ASP.NET 2.0 向导控件创建分步的用户界面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [检查 ASP.NET 2.0 的网站的导航](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [母版页和网站的导航](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [SQL 站点地图提供商提供你所期待的](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

>[!div class="step-by-step"]
[上一页](creating-the-membership-schema-in-sql-server-vb.md)
[下一页](validating-user-credentials-against-the-membership-user-store-vb.md)
