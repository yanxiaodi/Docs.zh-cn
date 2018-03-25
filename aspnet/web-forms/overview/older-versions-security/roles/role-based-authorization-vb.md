---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: "基于角色的授权 (VB) |Microsoft 文档"
author: rick-anderson
description: "本教程开头一下如何角色 framework 将用户的角色与他的安全上下文相关联。 然后，它会检查如何应用基于角色的 URL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 331282dfa3c05dd4bd6fef19dcfe7e5c0adad84d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="role-based-authorization-vb"></a>基于角色的授权 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip)或[下载 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> 本教程开头一下如何角色 framework 将用户的角色与他的安全上下文相关联。 然后，它检查如何应用基于角色的 URL 授权规则。 以下，我们将着眼于更改显示的数据和由 ASP.NET 页面提供的功能的使用声明性和编程的方式。


## <a name="introduction"></a>介绍

在<a id="_msoanchor_1"> </a> [*基于用户的授权*](../membership/user-based-authorization-vb.md)教程，我们已了解如何使用 URL 授权来指定哪些用户可以访问一组特定的页。 与只需要一点中标记`Web.config`，我们可以指示 ASP.NET 以仅允许经过身份验证的用户以访问一个页面。 或者，我们无法规定，允许用户 Tito 和 Bob，或指示 Sam 除外的所有经过身份验证的用户已允许。

除了 URL 授权，我们还了解了用于控制显示的数据和提供基于用户访问的页面的功能的声明性和编程技术。 具体而言，我们创建一个页，列出当前目录的内容。 任何人都无法访问此页上，但仅经过身份验证的用户无法查看的文件的内容以及仅 Tito 无法删除的文件。

应用基于用户的用户的授权规则可以增长簿记非常困难。 更易于维护的方法是使用基于角色的授权。 好消息是，在我们的应用授权规则的处理方法的工具工作。 同样好与角色，因为它们的用户帐户。 URL 授权规则可以指定而不是用户角色。 LoginView 控件，来呈现的经过身份验证和匿名用户不同的输出，可以配置为显示不同内容基于在已登录的用户的角色。 和角色 API 包括用于确定在已登录的用户的角色的方法。

本教程开头一下如何角色 framework 将用户的角色与他的安全上下文相关联。 然后，它检查如何应用基于角色的 URL 授权规则。 以下，我们将着眼于更改显示的数据和由 ASP.NET 页面提供的功能的使用声明性和编程的方式。 让我们进入正题！

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>了解如何角色关联的用户的安全上下文

当请求进入 ASP.NET 管道它时关联的安全上下文，包括标识请求者的信息。 当使用窗体身份验证，身份验证票证用作一个标识令牌。 如我们所述<a id="_msoanchor_2"> </a> [*概述窗体身份验证的*](../introduction/an-overview-of-forms-authentication-vb.md)和<a id="_msoanchor_3"> </a> [*窗体身份验证配置和高级主题*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)教程，`FormsAuthenticationModule`负责确定请求者，它的作用期间的身份[`AuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

如果找到有效、 未过期的身份验证票证，则`FormsAuthenticationModule`进行解码，以确定请求者的身份。 它创建一个新`GenericPrincipal`对象并将分配到`HttpContext.User`对象。 用途的主体，如`GenericPrincipal`，是确定已经过身份验证的用户的名称和什么她所属的角色。 此目的较为明显，因为所有的主体对象具有`Identity`属性和`IsInRole(roleName)`方法。 `FormsAuthenticationModule`，但是，不感兴趣录制角色信息和`GenericPrincipal`它创建的对象并不指定任何角色。

如果启用了角色 framework， [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) HTTP 模块中的步骤后`FormsAuthenticationModule`和标识期间的经过身份验证的用户的角色[`PostAuthenticateRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)，后者之后，将引发`AuthenticateRequest`事件。 如果该请求是否来自经过身份验证的用户，`RoleManagerModule`覆盖`GenericPrincipal`创建对象`FormsAuthenticationModule`，并替换与[`RolePrincipal`对象](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)。 `RolePrincipal`类使用角色 API 来确定哪些用户所属的角色。

图 1 描绘了 ASP.NET 管道工作流时使用窗体身份验证和角色 framework。 `FormsAuthenticationModule`首先执行，标识用户通过其身份验证票证，并创建一个新`GenericPrincipal`对象。 接下来，`RoleManagerModule`中的步骤，并覆盖`GenericPrincipal`对象`RolePrincipal`对象。

如果匿名用户访问该网站，既不`FormsAuthenticationModule`也不`RoleManagerModule`创建主体对象。


[![身份验证的用户时使用窗体身份验证和角色 Framework ASP.NET 管道事件](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**图 1**: ASP.NET 管道事件以进行身份验证用户时使用 Forms 身份验证和角色 Framework ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>缓存在 Cookie 中的角色信息

`RolePrincipal`对象的`IsInRole(roleName)`方法调用`Roles`。`GetRolesForUser` 若要获取用户的角色，以便确定用户是否属于*roleName*。 使用时`SqlRoleProvider`，这会导致查询到角色存储数据库。 使用基于角色的 URL 授权规则时`RolePrincipal`的`IsInRole`将对每个受基于角色的 URL 授权规则页请求调用方法。 而不需要查找数据库中对每个请求的角色信息`Roles`framework 包括缓存的 cookie 中的用户的角色的选项。

如果角色 framework 配置为缓存在 cookie 中，用户的角色`RoleManagerModule`在 ASP.NET 管道期间创建 cookie [ `EndRequest`事件](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)。 在中的后续请求中使用此 cookie `PostAuthenticateRequest`，即当`RolePrincipal`创建对象。 如果 cookie 有效，并且未过期，并将数据在 cookie 中的分析用于填充用户的角色，从而保存`RolePrincipal`无需调用`Roles`类以确定用户的角色。 图 2 描绘了此工作流。


[![用户的角色信息可以存储在一个 Cookie 以提高性能](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**图 2**： 的用户角色可以存储信息提高性能的 Cookie 中 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image6.png))


默认情况下，角色缓存 cookie 机制处于禁用状态。 可以通过启用`<roleManager>`; 中的配置标记`Web.config`。 我们讨论了使用[`<roleManager>`元素](https://msdn.microsoft.com/library/ms164660.aspx)指定中的角色提供程序<a id="_msoanchor_4"> </a> [*创建和管理角色*](creating-and-managing-roles-vb.md)教程中，因此，你应该已在你的应用程序中此元素`Web.config`文件。 角色缓存 cookie 设置中指定的属性`<roleManager>`; 表 1 中的元素，并汇总。

> [!NOTE]
> 表 1 中列出的配置设置指定生成的角色缓存 cookie 的属性。 在 cookie、 它们的工作原理和它们的各种属性的详细信息，请阅读[此 Cookie 教程](http://www.quirksmode.org/js/cookies.html)。


| **Property** | **说明** |
| --- | --- |
| `cacheRolesInCookie` | 一个布尔值，该值指示是否使用 cookie 缓存。 默认为 `false`。 |
| `cookieName` | 角色缓存 cookie 的名称。 默认值是"。ASPXROLES"。 |
| `cookiePath` | 角色名称 cookie 的路径。 Path 属性使开发人员可以限制到特定的目录层次结构的 cookie 的作用域。 默认值是"/"，它通知浏览器将身份验证票证 cookie 发送到对域进行任何请求。 |
| `cookieProtection` | 指示哪些技术用来保护角色缓存 cookie。 允许的值包括： `All` （默认值）;`Encryption`;`None`; 和`Validation`。 步骤 3 中将回指<a id="_anchor_5"> </a> [*窗体身份验证配置和高级主题*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)教程，以有关这些保护级别的详细信息。 |
| `cookieRequireSSL` | 一个布尔值，该值指示是否需要 SSL 连接来传输身份验证 cookie。 默认值为 `false`。 |
| `cookieSlidingExpiration` | 一个布尔值，该值指示用户是否每次重置的 cookie 超时在单个会话期间访问该站点。 默认值为 `false`。 此值才相关`createPersistentCookie`设置为`true`。 |
| `cookieTimeout` | 指定时间，以分钟为单位，身份验证票证 cookie 过期。 默认值为 `30`。 此值才相关`createPersistentCookie`设置为`true`。 |
| `createPersistentCookie` | 一个布尔值，指定的角色缓存 cookie 的会话 cookie 或持久性 cookie。 如果`false`（默认值），使用会话 cookie，关闭浏览器时，其删除。 如果`true`，使用持久性 cookie; 它过期`cookieTimeout`数分钟后已创建或前一次访问，具体取决于的值后`cookieSlidingExpiration`。 |
| `domain` | 指定 cookie 的域值。 默认值为空字符串，这会导致浏览器使用从该情况下，它已签发 （如 www.yourdomain.com) 的域。 在这种情况下，cookie 将**不**时进行请求定向到子域，例如 admin.yourdomain.com 发送。如果你想要传递给所有子域的 cookie 需要自定义`domain`属性，将其设置为"yourdomain.com"。 |
| `maxCachedResults` | 在 cookie 中指定缓存的角色名称的最大的数量。 默认值为 25。 `RoleManagerModule`不会创建 cookie 的用户，属于多个`maxCachedResults`角色。 因此，`RolePrincipal`对象的`IsInRole`方法将使用`Roles`类以确定用户的角色。 原因`maxCachedResults`存在是因为许多用户代理不允许 cookie 大于 4096 字节。 因此，此线帽旨在降低超过此大小限制的可能性。 如果你有极长的角色名称，你可能想要考虑指定较小`maxCachedResults`值; contrariwise，如果你有极短的角色名称，您可以可能增大此值。 |

**表 1**： 角色缓存 Cookie 配置选项

让我们配置应用程序以使用非持久的角色缓存 cookie。 若要实现此目的，更新`<roleManager>`中的元素`Web.config`包括以下与 cookie 相关的属性：

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

我已更新`<roleManager>`; 通过添加三个特性的元素： `cacheRolesInCookie`， `createPersistentCookie`，和`cookieProtection`。 通过设置`cacheRolesInCookie`到`true`、`RoleManagerModule`现在将自动缓存在 cookie 中的用户的角色，而不是无需查找在每次请求的用户的角色信息。 我显式设置`createPersistentCookie`和`cookieProtection`特性以`false`和`All`分别。 从技术上讲，我不需要指定值对于这些特性，因为它们仅分配到其默认值，但我将它们放在此处以使其显式清除我现在没有使用持久性 cookie 和 cookie 既加密和验证。

这就是所有到它 ！ 自此以后，角色 framework 将缓存在 cookie 中的用户的角色。 如果用户的浏览器不支持 cookie，或者如果其 cookie 被删除或丢失，由于某种原因，它是没有了不起 –`RolePrincipal`对象时将只使用`Roles`任何 cookie （或一个无效或已过期） 是可用的情况下的类。

> [!NOTE]
> Microsoft 的模式&amp;实践组不鼓励使用持久性角色缓存 cookie。 如果黑客可以以某种方式获得访问权限的有效用户 cookie，因为拥有的角色缓存 cookie 不足以证明角色成员身份，他可以模拟该用户。 如果在用户的浏览器上保存 cookie，也会增大此类情况发生的可能性。 有关此安全建议，以及其他安全问题的详细信息，请参阅[适用于 ASP.NET 2.0 的安全问题列表](https://msdn.microsoft.com/library/ms998375.aspx)。


## <a name="step-1-defining-role-based-url-authorization-rules"></a>步骤 1： 定义基于角色的 URL 授权规则

中所述<a id="_msoanchor_6"> </a> [*基于用户的授权*](../membership/user-based-authorization-vb.md)教程中，URL 授权提供一种限制对一组页面上的用户的用户或角色的角色的访问的方法基数。 URL 授权规则中将逐一`Web.config`使用[`<authorization>`元素](https://msdn.microsoft.com/library/8d82143t.aspx)与`<allow>`和`<deny>`子元素。 除了前面的教程中所述的与用户相关的授权规则每个`<allow>`和`<deny>`子元素还可以包括：

- 特定角色
- 角色的以逗号分隔列表

例如，URL 授权规则授予在管理员和主管角色中，这些用户访问权限，但拒绝向所有其他用户的访问：

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>`上面的标记中的元素状态，允许管理员和主管角色; `<deny>`; 元素指示*所有*用户被拒绝。

让我们配置我们的应用程序，以便`ManageRoles.aspx`， `UsersAndRoles.aspx`，和`CreateUserWizardWithRoles.aspx`页才可供在管理员角色中，这些用户访问时`RoleBasedAuthorization.aspx`仍然可以对所有访问者访问该页。

若要完成此操作，首先要添加`Web.config`文件为`Roles`文件夹。


[![将 Web.config 文件添加到角色目录](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**图 3**： 添加`Web.config`文件为`Roles`目录 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image9.png))


接下来，添加到以下配置标记`Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>`中的元素`<system.web>`部分指示，只有管理员角色中的用户可能访问中的 ASP.NET 资源`Roles`目录。 `<location>`元素定义一组备用 URL 授权规则`RoleBasedAuthorization.aspx`页上，允许所有用户访问页。

保存到所做的更改后`Web.config`、 不是管理员角色的用户身份登录，然后尝试访问某个受保护页。 `UrlAuthorizationModule`将检测您没有权限访问所请求的资源; 因此，`FormsAuthenticationModule`你将重定向到登录页。 登录页将然后将你重定向到`UnauthorizedAccess.aspx`页面 （请参见图 4）。 此最终的重定向到登录页从`UnauthorizedAccess.aspx`由于我们将添加到在步骤 2 中的登录页的代码发生<a id="_msoanchor_7"> </a> [*基于用户的授权*](../membership/user-based-authorization-vb.md)教程。 具体而言，登录页自动重定向到任何经过身份验证的用户`UnauthorizedAccess.aspx`如果查询字符串包含`ReturnUrl`参数，作为此参数指示，用户转到登录页面后尝试查看他不是的页被授权查看。


[![只有管理员角色中的用户可以查看受保护的页](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**图 4**： 只有中管理员角色的用户可以查看受保护页 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image12.png))


注销，然后作为管理员角色的用户登录。 现在，你应能够查看三个受保护的页。


[![可以访问 Tito UsersAndRoles.aspx 页因为他不能管理员角色](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**图 5**: Tito 可以访问`UsersAndRoles.aspx`页因为他不能管理员角色 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> 指定的角色或用户 – – URL 授权规则时务必要记住的规则是经过分析一次一个地，从顶部向下。 只要找到匹配项，用户被授予或拒绝访问，根据如果中找到匹配`<allow>`或`<deny>`元素。 **如果不找到任何匹配项，则用户被授予访问权限。** 因此，如果你想要限制对一个或多个用户帐户的访问，则你使用命令性`<deny>`URL 授权配置中的最后一个元素的元素。 **如果你的 URL 授权规则不包括**`<deny>`**元素中，所有用户将被都授予访问。** 有关如何分析 URL 授权规则的更全面讨论，回头参考"一看`UrlAuthorizationModule`使用授权规则来允许或拒绝访问"部分<a id="_msoanchor_8"> </a> [ *基于用户的授权*](../membership/user-based-authorization-vb.md)教程。


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>步骤 2： 限制基于当前登录的用户的角色的功能

轻松地指定粗糙的授权规则该状态哪些标识的 URL 授权使允许和拒绝哪些功能在查看特定页 （或一个文件夹及其子文件夹中的所有页）。 但是，在某些情况下我们可能想要允许所有用户访问页上，但限制基于正在访问用户的角色的页面的功能。 这可能需要显示或隐藏数据基于用户的角色，或为属于特定角色的用户提供其他功能。

可以以声明方式或以编程方式 （或两个的某种组合），可以实现这种细粒度的基于角色的授权规则。 在下一部分中，我们将了解如何实施 LoginView 控件通过声明性的细粒度授权。 接下来，我们将探讨编程技术。 我们可以看看应用细粒度授权规则之前，但是，我们首先需要创建其功能取决于访问它的用户角色的页。

让我们创建 GridView 在系统中列出的所有用户帐户的页面。 GridView 将包括每个用户的用户名、 电子邮件地址、 上次登录的日期和有关用户的注释。 除了显示每个用户的信息，GridView 将包括编辑和删除功能。 最初，我们将创建具有编辑此页，或删除功能可供所有用户。 "使用 LoginView 控件"和"以编程方式限制的功能"部分中我们将了解如何启用或禁用这些功能基于正在访问用户的角色。

> [!NOTE]
> 我们将要生成 ASP.NET 页使用 GridView 控件来显示用户帐户。 自系列侧重于窗体身份验证、 授权、 用户帐户和角色本教程中，我不希望花费太多时间来讨论 GridView 控件的内部工作情况。 虽然本教程提供了有关设置此页的特定分步说明，它不会不深入探讨原因所做的某些选择，或影响特定属性对呈现输出的详细信息。 GridView 控件全面检查，请查看我*[在 ASP.NET 2.0 中使用数据](../../data-access/index.md)*教程系列。


首先打开`RoleBasedAuthorization.aspx`页面`Roles`文件夹。 从拖到设计器和组页上拖动一个 GridView 其`ID`到`UserGrid`。 在稍后我们将编写调用的代码`Membership`。`GetAllUsers` 方法，并将绑定生成`MembershipUserCollection`对象到 GridView。 `MembershipUserCollection`包含`MembershipUser`系统; 中的每个用户帐户的对象`MembershipUser`对象具有属性，例如`UserName`，`Email`，`LastLoginDate` ，依此类推。

我们编写代码，以便将用户帐户绑定到网格之前，让我们首先定义 GridView 的字段。 在 GridView 的智能标记上，单击"编辑列"链接以启动字段对话框框中 （请参阅图 6）。 从此处，取消选中中左下角的"自动生成字段"复选框。 由于我们希望此 GridView 编辑和删除功能，包括添加 CommandField 并设置其`ShowEditButton`和`ShowDeleteButton`属性为 True。 接下来，添加用于显示的四个字段`UserName`， `Email`， `LastLoginDate`，和`Comment`属性。 BoundField 用于两个只读属性 (`UserName`和`LastLoginDate`) 和两个可编辑字段的 TemplateFields (`Email`和`Comment`)。

使第一个 BoundField 显示`UserName`属性; 设置其`HeaderText`和`DataField`为"UserName"的属性。 此字段将不可编辑，因此设置其`ReadOnly`属性为 True。 配置`LastLoginDate`BoundField 通过设置其`HeaderText`到"最后一个登录名"并将其`DataField`到"LastLoginDate"。 以便只是日期显示 （而不是日期和时间），让我们设置此 BoundField 输出格式。 若要完成此操作，将设置此 BoundField`HtmlEncode`属性设置为 False 并将其`DataFormatString`属性设置为"{0: d}"。 此外设置`ReadOnly`属性为 True。

设置`HeaderText`"电子邮件"和"注释"两个 TemplateFields 的属性。


[![可通过字段对话框中配置 GridView 的字段](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**图 6**: GridView 字段可以是配置通过字段对话框 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image18.png))


现在，我们需要定义`ItemTemplate`和`EditItemTemplate`"Email"和"注释"TemplateFields。 将标签 Web 控件添加到各个`ItemTemplates`并将绑定其`Text`属性设置为`Email`和`Comment`属性，分别。

对于"Email"TemplateField 中，添加名为文本框中`Email`到其`EditItemTemplate`并将绑定其`Text`属性`Email`使用双向数据绑定的属性。 添加 RequiredFieldValidator 和到 RegularExpressionValidator`EditItemTemplate`以确保访问者编辑电子邮件属性输入一个有效的电子邮件地址。 对于"注释"TemplateField 中，添加名为多行文本框中`Comment`到其`EditItemTemplate`。 设置文本框的`Columns`和`Rows`属性设置为 40 和 4，分别，然后将绑定其`Text`属性`Comment`使用双向数据绑定的属性。

后配置这些 TemplateFields，其声明性标记应看起来类似于以下：

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

当编辑或删除用户帐户，我们将需要了解该用户的`UserName`属性值。 设置 GridView`DataKeyNames`为"UserName"的属性，以便此信息可通过 GridView`DataKeys`集合。

最后，向页面添加 ValidationSummary 控件并设置其`ShowMessageBox`属性为 True 并且其`ShowSummary`属性设置为 False。 使用这些设置，ValidationSummary 将显示客户端警报，如果用户尝试编辑具有缺失或无效的电子邮件地址的用户帐户。

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

我们现在已经完成了此页面的声明性标记。 我们的下一个任务是将用户帐户的组绑定到 GridView。 添加一个名为`BindUserGrid`到`RoleBasedAuthorization.aspx`将绑定的页的代码隐藏类`MembershipUserCollection`返回`Membership.GetAllUsers`到`UserGrid`GridView。 调用此方法从`Page_Load`上第一次的页访问的事件处理程序。

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

使用此代码中的位置，请访问通过浏览器页面。 如图 7 所示，你应看到列出有关每个用户帐户的信息系统中一个 GridView。


[![UserGrid GridView 列出系统中有关每个用户的信息](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**图 7**: `UserGrid` GridView 列出信息有关每个用户在系统中 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView 列出所有非分页接口中的用户。 此简单网格接口不适合方案有多个几十个或多个用户。 一个选项是配置 GridView 来启用分页。 `Membership.GetAllUsers`方法有两个重载： 另一个用于不接受任何输入的参数和返回的所有用户，另一个中的页索引和页大小的整数值采用并都返回仅指定用户的子集。 第二个重载可以使用到更有效地通过用户的页面，因为它返回的用户帐户所精确子集而非*所有*其中。 如果你具有数以千计的用户帐户，你可能想要考虑基于筛选器的接口，一个只显示其用户名选字符开头，例如这些用户。 [ `Membership.FindUsersByName`方法](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)是用于构建基于筛选器的用户界面的理想选择。 我们将了解在将来的教程中生成此类接口。


GridView 控件提供内置的编辑和删除支持时该控件绑定到正确配置的数据源控件，如 SqlDataSource 或对象数据源。 `UserGrid` GridView，但是，具有其数据以编程方式绑定; 因此，我们必须编写代码来执行这两个任务。 具体而言，我们将需要创建的事件处理程序 GridView `RowEditing`， `RowCancelingEdit`， `RowUpdating`，和`RowDeleting`在访问者单击 GridView 时触发的事件编辑，取消，更新，或删除按钮。

首先为 GridView 的创建的事件处理程序`RowEditing`， `RowCancelingEdit`，和`RowUpdating`事件，然后添加以下代码：

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing`和`RowCancelingEdit`事件处理程序只需设置 GridView`EditIndex`属性，然后重新绑定的列表的用户帐户添加到网格。 有趣的内容发生在`RowUpdating`事件处理程序。 此事件处理程序启动通过确保验证数据是否有效，且然后捕捉`UserName`值中的已编辑的用户帐户`DataKeys`集合。 `Email`和`Comment`中两个 TemplateFields 的文本框中`EditItemTemplate`s 然后以编程方式引用。 其`Text`属性包含编辑电子邮件地址和注释。

若要更新通过成员资格 API 的用户帐户，我们需要先获取用户的信息，我们执行此操作通过调用`Membership.GetUser(userName)`。 返回`MembershipUser`对象的`Email`和`Comment`编辑接口从在两个文本框中输入的值与然后更新属性。 最后，通过调用保存这些修改[ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)。 `RowUpdating`事件处理程序完成通过还原其预编辑接口 GridView。

接下来，创建`RowDeleting`RowDeleting 事件处理程序，然后添加以下代码：

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

上面的事件处理程序启动通过抓取`UserName`GridView 的中值`DataKeys`集合; 此`UserName`值然后传递到成员资格类[`DeleteUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)。 `DeleteUser`方法系统，包括相关的成员身份数据 （如哪些角色此用户所属的） 中删除用户帐户。 删除用户，网格的后`EditIndex`（以防另一个行处于编辑模式时，用户单击删除） 设置为-1 和`BindUserGrid`调用方法。

> [!NOTE]
> 删除按钮不需要任何种类的用户才能删除用户帐户的确认。 我建议你添加某种形式的用户确认，以减小被意外删除的帐户的可能性。 确认某项操作的最简单方法之一是通过客户端确认对话框。 有关此技术的详细信息，请参阅[删除时添加客户端确认](https://asp.net/learn/data-access/tutorial-42-vb.aspx)。


验证此页按预期方式运行。 你应能够编辑任何用户的电子邮件地址和注释，以及删除任何用户帐户。 由于`RoleBasedAuthorization.aspx`页面所有用户访问，任何用户 – 甚至匿名访问者 – 可以访问此页面并编辑和删除用户帐户 ！ 让我们更新此页，以便只有主管和管理员角色中的用户可以编辑用户的电子邮件地址和注释，并且只有管理员才能删除用户帐户。

"使用 LoginView 控件"部分介绍使用 LoginView 控件来显示说明特定于用户的角色。 如果管理员角色的人员在访问此页时，我们将演示如何编辑和删除用户的说明。 如果主管角色中的用户达到此页，我们将显示在编辑用户的说明。 而如果在距访客为匿名或不在的主管或管理员角色，我们将显示一条消息说明不能编辑或删除用户帐户信息。 在"以编程方式限制的功能"部分中，我们将编写代码，以编程方式显示或隐藏基于用户的角色的编辑和删除按钮。

### <a name="using-the-loginview-control"></a>使用 LoginView 控件

如我们所见过去教程中，LoginView 控件可用于显示不同的接口，对于经过身份验证和匿名用户，但 LoginView 控件还可以用于显示不同的标记，基于用户的角色。 让我们使用 LoginView 控件显示基于正在访问用户的角色的不同指令。

通过添加上述 LoginView `UserGrid` GridView。 LoginView 控件如下已前面所述，有两个内置模板：`AnonymousTemplate`和`LoggedInTemplate`。 在这两个不能编辑或删除任何用户信息通知用户这些模板输入简要的消息。

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

除了`AnonymousTemplate`和`LoggedInTemplate`，LoginView 控件可以包含*RoleGroups*，这是特定于角色的模板。 每个 RoleGroup 包含单个属性， `Roles`，它指定 RoleGroup 适用于哪些角色。 `Roles`可以设置属性，为单个角色 （如"管理员"） 或逗号分隔列表的角色 （如"管理员，主管"）。

若要管理 RoleGroups，请单击从控件的智能标记上以打开 RoleGroup 集合编辑器的"编辑 RoleGroups"链接。 添加两个新 RoleGroups。 设置第一个 RoleGroup`Roles`属性设置为"管理员"和"主管"到秒。


[![管理 LoginView 的特定于角色的模板通过 RoleGroup 集合编辑器](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**图 8**： 管理 LoginView 的特定于角色的模板通过利用 RoleGroup 集合编辑器 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image24.png))


单击确定以关闭 RoleGroup 集合编辑器;这将更新 LoginView 的声明性标记，以包括`<RoleGroups>`部分当中`<asp:RoleGroup>`每个 RoleGroup 的子元素定义在 RoleGroup 集合编辑器中。 此外，"视图"下拉列表中 LoginView 的智能标记的最初列出仅`AnonymousTemplate`和`LoggedInTemplate`– 现在包括添加的 RoleGroups 以及。

以便主管角色中的用户显示的有关如何编辑用户帐户，而管理员角色中的用户将显示用于编辑和删除说明，请编辑 RoleGroups。 进行这些更改后，你 LoginView 声明性标记应类似于以下。

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

进行这些更改之后, 保存页面，然后通过浏览器中访问它。 首先为匿名用户访问页。 你应显示消息，"你尚未登录到系统。 因此你无法编辑或删除任何用户信息。" 然后以经过身份验证的用户，但其中一个主管或管理员角色中既不是登录。 此时应看到消息"你不是主管或管理员角色的成员。 因此你无法编辑或删除任何用户信息。"

接下来，登录的用户是主管角色的成员身份。 此时你应该会看到两个监控器特定于角色的消息 （请参阅图 9）。 如果你的管理员角色，你应看到有一些管理员特定于角色的消息 （请参阅图 10） 中的用户身份登录。


[![罗斯向她显示主管特定于角色的消息](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**图 9**： 罗斯向她显示主管特定于角色的消息 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image27.png))


[![Tito 显示管理员特定于角色的消息](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**图 10**: Tito 显示管理员特定于角色的消息 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image30.png))


图 9 中的屏幕快照和 10 显示的 LoginView 只呈现一个模板，即使多个模板应用。 罗斯向她和 Tito 都记录在用户，但 LoginView 呈现仅匹配 RoleGroup 而不`LoggedInTemplate`。 此外，Tito 所属的管理员和主管角色，但 LoginView 控件呈现而不是两个监控器管理员角色特定的模板之一。

图 11 阐释 LoginView 控件用于确定要呈现哪个模板的工作流。 请注意，是否有多个 RoleGroup 指定，将呈现 LoginView 模板*第一个*RoleGroup 相匹配。 换而言之，如果我们有放置作为第一个 RoleGroup 主管 RoleGroup 和为第二个管理员，然后 Tito 访问此页时他会看到主管消息。


[![LoginView 控件的工作流用于确定呈现到模板](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**图 11**： 确定什么模板呈现 LoginView 控件的工作流 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>以编程方式限制功能

当 LoginView 控件显示基于访问的页面的用户角色的不同指令时，则编辑和取消按钮保持对所有可见。 我们需要以编程方式隐藏匿名访问者和用户主管和管理员都不角色中的编辑和删除按钮。 我们需要为每个用户不是管理员隐藏删除按钮。 为了实现此目的，我们将编写的代码以编程方式引用 CommandField 的编辑和删除 LinkButtons 并设置其`Visible`属性设置为`False`，如果有必要。

以编程方式引用 CommandField 中的控件的最简单方法是先将其转换为模板。 若要完成此操作，单击从 GridView 的智能标记的"编辑列"链接，从当前字段的列表中选择 CommandField，然后单击"将此字段转换为 TemplateField"链接。 这将转换为 TemplateField CommandField`ItemTemplate`和`EditItemTemplate`。 `ItemTemplate`包含的编辑和删除时的 LinkButtons`EditItemTemplate`保存的更新和取消 LinkButtons。


[![将 CommandField 转换转换为 TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**图 12**： 转换 TemplateField CommandField 到 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image36.png))


更新的编辑和删除 LinkButtons 中`ItemTemplate`，设置其`ID`属性的值`EditButton`和`DeleteButton`分别。

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

每当数据绑定到 GridView，GridView 枚举中的记录其`DataSource`属性，并生成相应`GridViewRow`对象。 为每个`GridViewRow`创建对象时，`RowCreated`激发事件。 若要隐藏未经授权的用户的编辑和删除按钮，我们需要创建此事件的事件处理并以编程方式引用的编辑和删除 LinkButtons，设置其`Visible`属性相应地。

创建一个事件处理程序`RowCreated`事件，然后添加以下代码：

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

请记住，`RowCreated`个都会激发事件*所有*的 GridView 行，包括的页眉、 页脚、 页导航界面和等。 我们只想要以编程方式引用的编辑和删除 LinkButtons 如果我们处理的数据行未处于编辑模式 （因为处于编辑模式的行而不是编辑和删除的更新和取消按钮）。 此检查由`If`语句。

如果我们处理的数据行不处于编辑模式下，引用的编辑和删除 LinkButtons 及其`Visible`属性根据返回的布尔值设置`User`对象的`IsInRole(roleName)`方法。 `User`对象引用由主体`RoleManagerModule`; 因此，`IsInRole(roleName)`方法使用角色 API 来确定是否当前的访问者属于*roleName*。

> [!NOTE]
> 我们本来也可以使用角色类直接替换调用`User.IsInRole(roleName)`通过调用[`Roles.IsUserInRole(roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)。 我决定要使用的主体对象`IsInRole(roleName)`在此示例中的方法因为它是比直接使用角色 API 更加有效。 本教程中前面我们配置了要缓存的 cookie 中的用户的角色的角色管理器。 此缓存的 cookie 数据仅当利用该主体的`IsInRole(roleName)`调用方法; 对角色 API 的直接调用总会涉及到角色存储到的行程。 即使角色不会缓存在 cookie 中，调用的主体对象`IsInRole(roleName)`方法是通常更高效，因为为它缓存结果的第一次请求过程的调用时。 角色 API，另一方面，不执行任何缓存。 因为`RowCreated`中 GridView，每行一次激发事件使用`User.IsInRole(roleName)`而涉及到角色存储只在一个行程`Roles.IsUserInRole(roleName)`需要*N*行程，其中*N*是网格中显示的用户帐户数。


编辑按钮的`Visible`属性设置为`True`如果用户访问此页中的管理员或主管角色; 否则它设置为`False`。 删除按钮的`Visible`属性设置为`True`仅当用户处于管理员角色。

通过浏览器的此页进行测试。 作为匿名访问者或既不主管，也不管理员的用户访问页，CommandField 为空;它仍然存在，但作为而无需编辑或删除精简银按钮。

> [!NOTE]
> 可以隐藏 CommandField 完全当非管理员和非管理员为访问的页面。 我将此作为练习为读取器。


[![编辑和删除按钮隐藏非主管和非管理员](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**图 13**: 编辑和删除按钮隐藏非主管和非管理员 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image39.png))


如果用户属于主管角色 （但不是属于管理员角色） 访问时，他会看到仅编辑按钮。


[![编辑按钮时可供主管，隐藏删除按钮](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**图 14**： 时的编辑按钮可用于主管，则删除按钮处于隐藏状态 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image42.png))


如果管理员访问时，她有权编辑和删除按钮。


[![编辑和删除按钮才可用管理员](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**图 15**: 编辑和删除按钮才可用管理员 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>步骤 3： 将基于角色的授权规则应用于类和方法

在步骤 2 中，我们仅限于编辑为主管和管理员角色的功能和删除只向管理员功能。 这被通过隐藏未经授权的用户，通过编程技术的相关联的用户界面元素。 此类度量值不能保证未经授权的用户将不能执行特权的操作。 可能有更高版本添加的用户界面元素或我们忘了要隐藏的未经授权的用户。 或黑客可能会发现某些其他方法可以实现 ASP.NET 页后，可以执行所需的方法。

确保未经授权的用户不能访问的功能的特定部分的简单办法是修饰该类或方法替换[`PrincipalPermission`属性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)。 当.NET 运行时使用的类，或者执行其方法之一时，它会检查以确保当前安全上下文具有权限。 `PrincipalPermission`属性提供一种机制，通过该我们可以定义这些规则。

我们看使用`PrincipalPermission`属性返回<a id="_msoanchor_9"> </a> [*基于用户的授权*](../membership/user-based-authorization-vb.md)教程。 具体而言，我们已了解如何修饰 GridView`SelectedIndexChanged`和`RowDeleting`事件处理程序，以便它们无法由执行已经过身份验证的用户和 Tito，分别。 `PrincipalPermission`属性同样适用的角色。

让我们演示如何使用`PrincipalPermission`属性 GridView`RowUpdating`和`RowDeleting`要禁止未经授权的用户执行的事件处理程序。 我们需要做的只是添加之上打开每个函数定义适当的特性：

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

为属性`RowUpdating`事件处理程序指示只有管理员或主管角色的用户可以执行而的上的属性的事件处理程序，`RowDeleting`事件处理程序限制到管理员中的用户执行角色。


> [!NOTE]
> `PrincipalPermission`属性表示为一个类中`System.Security.Permissions`命名空间。 请务必添加`Imports System.Security.Permissions`语句你的代码隐藏类文件导入此命名空间的顶部。


如果由于某种原因，非管理员尝试执行`RowDeleting`事件处理程序或如果非监督程序或非管理员是否有尝试执行`RowUpdating`事件处理程序的.NET 运行时将引发`SecurityException`。


[![如果安全上下文无权执行方法，则会引发安全异常](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**图 16**： 如果安全上下文未被授权执行方法，`SecurityException`引发 ([单击以查看实际尺寸的图像](role-based-authorization-vb/_static/image48.png))


除了 ASP.NET 页中，许多应用程序还具有一个体系结构，包括各层，如业务逻辑和数据访问层。 这些层通常作为类库实现，并提供类和用于执行业务逻辑和数据相关的功能的方法。 `PrincipalPermission`属性是将授权规则应用于这些层也很有用。

有关详细信息使用`PrincipalPermission`属性以便定义类和方法的授权规则，请参阅[Scott Guthrie](https://weblogs.asp.net/scottgu/)的博客文章[业务和数据层使用添加授权规则`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>摘要

在本教程中我们介绍了如何指定粗陋和细粒度授权规则基于用户的角色。 ASP。NET 的 URL 授权功能允许页开发人员指定哪些标识允许或拒绝访问哪些页。 正如我们所看到的进来<a id="_msoanchor_10"> </a> [*基于用户的授权*](../membership/user-based-authorization-vb.md)教程，则 URL 授权规则可以应用基于用户的用户。 它们还可以将应用基于角色的角色，如我们在本教程的步骤 1 中看到。

以声明方式或以编程方式，则可能应用细粒度授权规则。 在步骤 2 中我们讨论在使用 LoginView 控件 RoleGroups 功能呈现基于正在访问用户的角色不同的输出。 我们还了解了如何以编程方式确定用户是否属于特定角色以及如何相应地调整页面的功能。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [将授权规则添加到业务和使用的数据层`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [检查 ASP.NET 2.0 的成员资格、 角色和配置文件： 使用角色](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [ASP.NET 2.0 的安全问题列表](https://msdn.microsoft.com/library/ms998375.aspx)
- [技术文档`<roleManager>`元素](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢...

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者包括 Suchi Banerjee 和 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一篇](assigning-roles-to-users-vb.md)
