---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: "创建和管理角色 (VB) |Microsoft 文档"
author: rick-anderson
description: "本教程可检查为配置角色框架所必需的步骤。 接下来，我们将生成的 web 页面以创建和删除角色。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: 0b1132c6d782cd85edb8cbee98c8ab95a15171ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-and-managing-roles-vb"></a>创建和管理角色 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip)或[下载 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> 本教程可检查为配置角色框架所必需的步骤。 接下来，我们将生成的 web 页面以创建和删除角色。


## <a name="introduction"></a>介绍

在<a id="_msoanchor_1"> </a> [*基于用户的授权*](../membership/user-based-authorization-vb.md)教程，我们了解了使用 URL 授权以限制某些用户从一组页面，并介绍了声明性和有关如何调整基于正在访问的用户的 ASP.NET 页的功能的编程技术。 授予访问页或基于用户的用户的功能的权限，但是，可以更大量方案中的维护任务在有多个用户帐户，或当用户的权限通常更改。 任何时候用户获得或失去授权，以执行特定任务，管理员需要更新的相应 URL 授权规则，声明性标记和代码。

它通常有助于将归入组的用户或*角色*，然后将权限应用基于角色的角色。 例如，大多数 web 应用程序具有某组页面或仅为管理用户保留的任务。 在中使用的技术学习到*基于用户的授权*教程中，我们将添加相应的 URL 授权规则、 声明性标记和代码，以允许指定的用户帐户来执行管理任务。 但如果添加到新的管理员或现有的管理员需要能够吊销其管理权限，我们必须返回并更新配置文件和 web 页。 与角色，但是，我们可以创建名为管理员角色并将这些受信任的用户分配到管理员角色。 接下来，我们将添加相应的 URL 授权规则、 声明性标记和代码，以允许管理员角色来执行各种管理任务。 与此基础结构，向站点添加新的管理员或删除现有类型是简单，只包括或从管理员角色中删除用户。 没有配置、 声明性标记或代码更改是必需的。

ASP.NET 提供一个用于定义角色和将其与用户帐户关联的角色框架。 使用角色 framework 我们可以创建和删除角色，将用户添加或从角色中删除用户、 确定属于特定角色，并告知用户是否属于特定角色的用户组。 一旦配置了角色框架，我们可以限制对页 URL 授权规则通过基于角色的角色的访问以及显示或隐藏的其他信息或基于当前登录的用户的角色页面上的功能。

本教程可检查为配置角色框架所必需的步骤。 接下来，我们将生成的 web 页面以创建和删除角色。 在<a id="_msoanchor_2"> </a> [*向用户分配角色*](assigning-roles-to-users-vb.md)教程我们将了解如何添加和从角色中删除用户。 然后在<a id="_msoanchor_3"> </a> [*基于角色的授权*](role-based-authorization-vb.md)教程，我们将了解如何限制对页以及如何调整页功能具体取决于基于角色的角色的访问正在访问用户的角色。 让我们进入正题！

## <a name="step-1-adding-new-aspnet-pages"></a>步骤 1： 添加新的 ASP.NET 页

我们将在本教程和后面的两个检查各种与角色相关的函数和功能。 我们将需要一系列的 ASP.NET 网页来实现检查整个这些教程的主题。 让我们创建这些页面和更新站点映射。

首先创建一个新文件夹中名为的项目`Roles`。 接下来，添加到四个新的 ASP.NET 页面`Roles`文件夹中，链接与每个页面`Site.master`母版页。 命名页：

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

此时你的项目的解决方案资源管理器应类似于的屏幕快照中图 1 所示。


[![四个新页面已添加到角色文件夹](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**图 1**： 四个新页已添加到`Roles`文件夹 ([单击以查看实际尺寸的图像](creating-and-managing-roles-vb/_static/image3.png))


每一页后，应该在此情况下，有两个内容控件，每个母版页的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

回想一下， `LoginContent` ContentPlaceHolder 的默认标记显示在登录或注销的站点，具体取决于是否对用户进行身份验证的链接。 是否存在`Content2`内容控件在 ASP.NET 页中，但是，重写主页面的默认标记。 如我们所述<a id="_msoanchor_4"> </a> [*概述窗体身份验证的*](../introduction/an-overview-of-forms-authentication-vb.md)教程中，重写的默认标记是有用我们不希望在何处显示登录相关的页面中左侧列中的选项。

对于这些四个页，但是，我们希望能够显示主控页的默认标记`LoginContent`ContentPlaceHolder。 因此，删除的声明性标记`Content2`内容控件。 这么做，四个页面的标记的每个应包含几个内容控件。

最后，让我们更新站点图 (`Web.sitemap`) 以包括这些新的 web 页面。 添加以下 XML 后的`<siteMapNode>`我们添加有关成员身份教程。

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

使用站点映射更新，请访问通过浏览器的站点。 如图 2 所示，在左侧的导航现在将包括以为角色教程的项。


[![四个新页面已添加到角色文件夹](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**图 2**： 四个新页已添加到`Roles`文件夹 ([单击以查看实际尺寸的图像](creating-and-managing-roles-vb/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>步骤 2： 指定和配置的角色 Framework 提供程序

成员资格 framework 中，如角色 framework 是在提供程序模型之上构建的。 中所述<a id="_msoanchor_5"> </a> [*安全性基础知识和 ASP.NET 支持*](../introduction/security-basics-and-asp-net-support-vb.md)教程中，.NET Framework 附带有三个内置角色提供程序： [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx)[ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)，和[ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)。 本系列教程侧重于`SqlRoleProvider`，它使用 Microsoft SQL Server 数据库作为角色存储。

实际上角色 framework 和`SqlRoleProvider`工作一样成员资格 framework 和`SqlMembershipProvider`。 .NET Framework 包含`Roles`用作角色 framework API 的类。 `Roles`类已共享等方法`CreateRole`， `DeleteRole`， `GetAllRoles`， `AddUserToRole`， `IsUserInRole`，依次类推。 当调用这些方法之一时，`Roles`类将调用委托给配置的提供程序。 `SqlRoleProvider`适用于特定于角色的表 (`aspnet_Roles`和`aspnet_UsersInRoles`) 在响应中。

若要使用`SqlRoleProvider`我们的应用程序中的提供程序，我们需要指定什么数据库以用作存储区。 `SqlRoleProvider`需要指定的角色存储具有某些数据库表、 视图和存储的过程。 可以使用添加这些必备项的数据库对象[`aspnet_regsql.exe`工具](https://msdn.microsoft.com/library/ms229862.aspx)。 现在我们已有具有所需的架构的数据库`SqlRoleProvider`。 返回<a id="_msoanchor_6"> </a> [*在 SQL Server 中创建成员身份架构*](../membership/creating-the-membership-schema-in-sql-server-vb.md)我们创建一个名为数据库的教程`SecurityTutorials.mdf`和使用`aspnet_regsql.exe`以添加该应用程序服务，它包括所需的数据库对象`SqlRoleProvider`。 因此我们只需通知角色框架以支持角色并使用`SqlRoleProvider`与`SecurityTutorials.mdf`作为角色存储的数据库。

通过配置角色 framework`<roleManager>`中应用程序的元素`Web.config`文件。 默认情况下，角色支持处于禁用状态。 若要启用它，必须设置[ `<roleManager>` ](https://msdn.microsoft.com/library/ms164660.aspx)元素的`enabled`属性设为`true`如下所示：

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

默认情况下，所有 web 应用程序都具有名为角色提供程序`AspNetSqlRoleProvider`类型的`SqlRoleProvider`。 此默认的提供程序注册中`machine.config`(位于`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

提供程序的`connectionStringName`属性指定使用角色存储。 `AspNetSqlRoleProvider`提供程序将此属性设置为`LocalSqlServer`，也会在定义`machine.config`和点，默认情况下，到中的 SQL Server 2005 Express Edition 数据库`App_Data`文件夹名为`aspnet.mdf`。

因此，如果我们只需为不在我们的应用程序中指定任何提供程序信息的情况下启用角色 framework`Web.config`文件，应用程序使用注册的默认角色提供程序， `AspNetSqlRoleProvider`。 如果`~/App_Data/aspnet.mdf`数据库不存在，ASP.NET 运行时将自动创建它并添加应用程序服务架构。 但是，我们不想使用`aspnet.mdf`数据库; 相反，我们想要使用`SecurityTutorials.mdf`我们已经创建并添加到应用程序服务架构的数据库。 此修改，可以处于两种方式之一实现：

- **指定的值 * * *`LocalSqlServer`* * * 中的连接字符串名称 * * *`Web.config`* * *。** 通过覆盖`LocalSqlServer`连接字符串名称值中的`Web.config`，我们可以使用注册的默认角色提供程序 (`AspNetSqlRoleProvider`)，并将其正确使用`SecurityTutorials.mdf`数据库。 有关此技术的详细信息，请参阅[Scott Guthrie](https://weblogs.asp.net/scottgu/)的博客文章[配置 ASP.NET 2.0 应用程序服务添加到使用 SQL Server 2000 或 SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)。
- **添加新的已注册提供程序的类型 * * *`SqlRoleProvider`* * * 并配置其 * * *`connectionStringName`* * * 设置为指向 * * *`SecurityTutorials.mdf`* * * 数据库。** 这是我建议并在中使用的方法<a id="_msoanchor_7"> </a> [*在 SQL Server 中创建成员身份架构*](../membership/creating-the-membership-schema-in-sql-server-vb.md)教程中，也是如此 I will use 以及本教程中使用的方法。

添加到以下的角色配置标记`Web.config`文件。 此标记注册新的提供程序名为`SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

上面的标记定义`SecurityTutorialsSqlRoleProvider`作为默认提供程序 (通过`defaultProvider`属性中`<roleManager>`元素)。 它还将设置`SecurityTutorialsSqlRoleProvider`的`applicationName`将设置为`SecurityTutorials`，即相同`applicationName`使用成员资格提供程序的设置 (`SecurityTutorialsSqlMembershipProvider`)。 而不显示在这里， [ `<add>`元素](https://msdn.microsoft.com/library/ms164662.aspx)为`SqlRoleProvider`还可能包含`commandTimeout`特性来指定数据库超时持续时间，以秒为单位。 默认值为 30。

此就地配置标记中，我们现在可以开始使用我们的应用程序中的角色功能。

> [!NOTE]
> 上面的配置代码演示如何使用`<roleManager>`元素的`enabled`和`defaultProvider`属性。 有多个其他影响角色 framework 如何将基于用户的用户的角色信息相关联的特性。 我们将检查这些设置位于<a id="_msoanchor_8"> </a> [*基于角色的授权*](role-based-authorization-vb.md)教程。


## <a name="step-3-examining-the-roles-api"></a>第 3 步： 检查角色 API

角色框架的功能公开通过[`Roles`类](https://msdn.microsoft.com/library/system.web.security.roles.aspx)，其中包含十三个共享的方法，用于执行基于角色的操作。 当我们看一下创建和删除在步骤 4 中的角色和 6 我们将使用[ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx)和[ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx)方法，它们将添加或从系统中删除角色。

若要获取系统中的所有角色的列表，请使用[`GetAllRoles`方法](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)（请参阅步骤 5）。 [ `RoleExists`方法](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx)返回一个布尔值，该值指示是否存在指定的角色。

在下一教程中，我们将检查如何将用户与角色关联。 `Roles`类的[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)， [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx)， [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)，和[ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx)方法将一个或多个用户添加到一个或多个角色。 若要从角色中删除用户，使用[ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)， [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx)， [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)，或[ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx)方法。

在<a id="_msoanchor_9"> </a> [*基于角色的授权*](role-based-authorization-vb.md)我们将了解如何以编程方式显示或隐藏基于当前登录的用户的角色的功能的教程。 若要实现此目的，我们可以使用角色类的[ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx)， [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)， [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)，或[ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)方法。

> [!NOTE]
> 请记住，则调用的任何时间下列方法之一，`Roles`类将调用委托给配置的提供程序。 在本例中，这意味着发送到调用`SqlRoleProvider`。 `SqlRoleProvider`然后执行相应的数据库操作基于调用的方法。 例如，代码`Roles.CreateRole("Administrators")`导致`SqlRoleProvider`执行`aspnet_Roles_CreateRole`存储过程，它插入一条新记录到`aspnet_Roles`表名为管理员。


本教程的其余部分考察使用`Roles`类的`CreateRole`， `GetAllRoles`，和`DeleteRole`方法来管理系统中的角色。

## <a name="step-4-creating-new-roles"></a>步骤 4： 创建新的角色

角色提供了一个方法向任意组用户，并且通常情况下此分组适用于更加方便地将授权规则。 但若要使用角色作为一种授权机制我们首先需要定义应用程序中有哪些角色。 遗憾的是，ASP.NET 不包括 CreateRoleWizard 控件。 为了添加新角色，我们需要创建一个合适的用户界面和自行调用角色 API。 好消息是，这是非常轻松地完成。

> [!NOTE]
> 尽管没有 CreateRoleWizard Web 控件，但还有[ASP.NET 网站管理工具](https://msdn.microsoft.com/library/ms228053.aspx)，这是本地 ASP.NET 应用程序，旨在帮助查看和管理 web 应用程序的配置。 但是，我不是以下两个原因非常热衷于 ASP.NET 网站管理工具。 首先，有点错误并且用户体验离开大量需要改进。 其次，ASP.NET 网站管理工具旨在仅在本地运行，这意味着，您将需要生成你自己的角色管理网页，如果你需要远程管理实时网站上的角色。 有关这两个原因，本教程和下一步将专注于构建在网页中的管理工具的必要角色，而不是依赖于 ASP.NET 网站管理工具。


打开`ManageRoles.aspx`页面`Roles`文件夹并向页面添加一个文本框和按钮 Web 控件。 设置文本框中控件的`ID`属性`RoleName`和按钮的`ID`和`Text`属性设置为`CreateRoleButton`和创建角色，分别。 此时，页面的声明性标记应类似以下：

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

接下来，双击`CreateRoleButton`按钮在设计器中创建的控件`Click`事件处理程序，然后添加以下代码：

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

通过分配中输入的裁剪后的角色名称来启动上面的代码`RoleName`到文本框中`newRoleName`变量。 接下来，`Roles`类的`RoleExists`调用方法来确定如果角色`newRoleName`系统中已存在。 如果该角色不存在，则创建它通过调用`CreateRole`方法。 如果`CreateRole`方法传递在系统中，已存在的角色名称`ProviderException`引发异常。 这就是为什么代码将首先检查，以确保该角色已中不存在的系统，然后调用`CreateRole`。 `Click`事件处理程序结束清除`RoleName`文本框的`Text`属性。

> [!NOTE]
> 你可能想知道将发生如果用户不输入任何值转换为`RoleName`文本框。 如果值传递给`CreateRole`方法是`Nothing`或空字符串，异常引发。 同样，如果角色名称包含逗号被引发异常。 因此，页面应包含验证控件，以确保用户输入角色，并且它不包含任何逗号。 我将保留为练习为读取器。


让我们创建一个名为管理员角色。 请访问`ManageRoles.aspx`通过浏览器页上，在文本框中键入管理员中 （请参见图 3），然后单击创建角色按钮。


[![创建管理员角色](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**图 3**： 创建管理员角色 ([单击以查看实际尺寸的图像](creating-and-managing-roles-vb/_static/image9.png))


发生什么情况？ 产生的回发，但没有任何视觉提示，角色实际上已被添加到系统。 我们将更新此页面，在步骤 5 中包括的视觉反馈。 现在，但是，您可以验证该角色的创建通过转到`SecurityTutorials.mdf`数据库和显示从数据`aspnet_Roles`表。 如图 4 所示，`aspnet_Roles`表包含刚添加管理员角色的记录。


[![Aspnet_Roles 表对于管理员，有一个行](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**图 4**:`aspnet_Roles`表具有一个行的管理员 ([单击以查看实际尺寸的图像](creating-and-managing-roles-vb/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>步骤 5： 在系统中显示的角色

让我们增加`ManageRoles.aspx`页以包含在系统中的当前角色的列表。 若要完成此操作，将 GridView 控件添加到页并设置其`ID`属性`RoleList`。 接下来，将方法添加到页面的代码隐藏类名为`DisplayRolesInGrid`使用下面的代码：

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

`Roles`类的`GetAllRoles`方法的所有角色系统中作为返回的字符串数组。 此字符串数组，然后绑定到 GridView。 若要将角色列表绑定到 GridView，首次加载页时，我们需要调用`DisplayRolesInGrid`方法从该页面的`Page_Load`事件处理程序。 下面的代码调用此方法时首次访问页，但不是能在后续回发。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

使用此代码中的位置，请访问通过浏览器页面。 如图 5 所示，你应看到与标记为项的单个列的网格。 网格包括我们在步骤 4 中添加的管理员角色的行。


[![GridView 单个列中显示的角色](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**图 5**: GridView 单个列中显示的角色 ([单击以查看实际尺寸的图像](creating-and-managing-roles-vb/_static/image15.png))


GridView 显示一个标记为项，因为的唯一列的 GridView`AutoGenerateColumns`属性设置为 True （默认值），这会导致 GridView 自动创建列中每个属性及其`DataSource`。 数组具有一个属性，表示在 GridView 的单个列的数组，因此中的元素。

显示与一个 GridView 的数据时，我愿意显式定义列，而不能将其隐式生成的 GridView。 通过显式定义列就可以更轻松地设置数据格式、 重新排列列，以及执行其他常见任务。 因此，让我们更新 GridView 的声明性标记，以便其列显式定义如此。

通过设置 GridView 的启动`AutoGenerateColumns`属性设置为 False。 接下来，将为 TemplateField 添加到网格中，设置其`HeaderText`属性角色，并配置其`ItemTemplate`，使其显示数组的内容。 若要完成此操作，将添加一个名为的标签 Web 控件`RoleNameLabel`到`ItemTemplate`并将绑定其`Text`属性`Container.DataItem.`

这些属性和`ItemTemplate`的内容可以设置以声明方式或通过 GridView 的字段对话框和编辑模板接口。 若要访问字段对话框中，单击 GridView 的智能标记中的编辑列链接。 接下来，取消选中自动生成字段复选框以设置`AutoGenerateColumns`属性设置为 False，并将为 TemplateField 添加到 GridView，设置其`HeaderText`到角色的属性。 若要定义`ItemTemplate`的内容，选择编辑模板选项从 GridView 的智能标记。 将标签 Web 控件拖到`ItemTemplate`，将其`ID`属性`RoleNameLabel`，并配置其数据绑定设置，以便其`Text`属性绑定到`Container.DataItem`。

无论你使用何种方法，GridView 的生成的声明性标记应类似于以下完成后。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> 数组的内容显示使用的数据绑定语法`<%# Container.DataItem %>`。 当数组的内容显示绑定到 GridView 为什么使用此语法的全面说明是超出了本教程的范围。 有关此问题的详细信息，请参阅[标量数组绑定到数据 Web 控件](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)。


目前， `RoleList` GridView 只能绑定到的角色列表，在首次访问页时。 我们需要刷新该网格，无论何时添加新的角色。 若要实现此目的，更新`CreateRoleButton`按钮的`Click`事件处理程序，使它调用`DisplayRolesInGrid`方法如果创建新的角色。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

现在当用户添加新角色`RoleList`GridView 回发时，显示刚才添加的角色提供角色已成功创建的视觉反馈。 若要说明这一点，请访问`ManageRoles.aspx`通过浏览器页上，添加一个名为主管角色。 如果单击创建角色按钮，回发将随之发生和网格将会更新以包含管理员，以及新角色，主管。


[![主管角色具有已添加](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**图 6**: 主管角色具有已添加 ([单击以查看实际尺寸的图像](creating-and-managing-roles-vb/_static/image18.png))


## <a name="step-6-deleting-roles"></a>步骤 6： 删除角色

用户可以创建新的角色并查看中的所有现有角色此时`ManageRoles.aspx`页。 让我们允许用户还删除角色。 `Roles.DeleteRole`方法有两个重载：

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx)-删除角色*roleName*。 如果该角色包含一个或多个成员，将引发异常。
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx)-删除角色*roleName*。 如果*throwOnPopulateRole*是`True`，则如果角色包含一个或多个成员引发异常。 如果*throwOnPopulateRole*是`False`，然后是否它包含任何成员，或不删除此角色。 在内部，`DeleteRole(roleName)`方法调用`DeleteRole(roleName, True)`。

`DeleteRole`方法将还会引发异常，如果*roleName*是`Nothing`或空字符串或如果*roleName*包含逗号。 如果*roleName*不在系统中，存在`DeleteRole`以无提示方式，失败，且不引发异常。

让我们增加在 GridView`ManageRoles.aspx`以包括删除按钮，单击时，删除所选的角色。 通过将删除按钮添加到 GridView，通过转到字段对话框中，并添加位于 CommandField 选项下的删除按钮开始。 请删除按钮最左侧的列并设置其`DeleteText`到删除的角色的属性。


[![将删除按钮添加到 RoleList GridView](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**图 7**： 添加到一个删除按钮`RoleList`GridView ([单击以查看实际尺寸的图像](creating-and-managing-roles-vb/_static/image21.png))


添加删除按钮后, GridView 的声明性标记应看起来类似于以下：

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

接下来，为 GridView 创建事件处理程序`RowDeleting`事件。 这是单击删除角色按钮时，在回发时引发的事件。 将以下代码添加到该事件处理程序中。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

通过以编程方式引用的代码启动`RoleNameLabel`Web 控件在其删除角色按钮被单击的行。 `Roles.DeleteRole`然后调用方法，传入`Text`的`RoleNameLabel`和`False`，从而删除的角色，而不管是否有任何用户与角色关联。 最后， `RoleList` GridView 刷新，以便只删除角色不会再出现在网格中。

> [!NOTE]
> 删除角色按钮不需要任何种类的用户从在删除角色之前的确认。 确认某项操作的最简单方法之一是通过客户端确认对话框。 有关此技术的详细信息，请参阅[删除时添加客户端确认](https://asp.net/learn/data-access/tutorial-42-vb.aspx)。


## <a name="summary"></a>摘要

许多 web 应用程序具有特定的授权规则或仅适用于特定类别的用户正在的页级功能。 例如，可能有一套只有管理员才能访问的网页。 而不是基于用户的用户定义这些授权规则，通常很多有用定义基于角色的规则。 即，而不是显式允许用户访问管理的网页，Scott 并 Jisun，更易于维护的方法是允许的管理员角色的成员才能访问这些页面中，然后为用户属于表示 Scott 和 Jisun管理员角色。

角色框架，可以轻松地创建和管理角色。 在本教程中，我们将探讨如何配置要使用的角色 framework `SqlRoleProvider`，它使用 Microsoft SQL Server 数据库作为角色存储。 我们还创建一个网页，列出系统中的现有角色，并允许新角色，以创建和现有的要删除。 在后续教程中，我们将看到如何将用户分配到角色以及如何将应用基于角色的授权。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [检查 ASP.NET 2.0 的成员资格、 角色和配置文件](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [如何： 在 ASP.NET 2.0 中使用角色管理器](https://msdn.microsoft.com/library/ms998314.aspx)
- [角色提供程序](https://msdn.microsoft.com/library/aa478950.aspx)
- [滚动你自己的网站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [技术文档`<roleManager>`元素](https://msdn.microsoft.com/library/ms164660.aspx)
- [使用成员资格和角色管理器 Api](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者包括 Alicja Maziarz、 Suchi Banerjee 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](role-based-authorization-cs.md)
[下一页](assigning-roles-to-users-vb.md)
