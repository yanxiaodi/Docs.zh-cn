---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: "成员身份 |Microsoft 文档"
author: microsoft
description: "ASP.NET 成员资格生成的窗体身份验证模型成功从 ASP.NET 1.x。 ASP.NET 窗体身份验证提供 incorp 到一种简便方式..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="membership"></a>成员身份
====================
by [Microsoft](https://github.com/microsoft)

> ASP.NET 成员资格生成的窗体身份验证模型成功从 ASP.NET 1.x。 ASP.NET 窗体身份验证可以方便地将一个登录窗体合并到 ASP.NET 应用程序并验证针对数据库或其他数据存储的用户。


ASP.NET 成员资格生成的窗体身份验证模型成功从 ASP.NET 1.x。 ASP.NET 窗体身份验证可以方便地将一个登录窗体合并到 ASP.NET 应用程序并验证针对数据库或其他数据存储的用户。 FormsAuthentication 类的成员，使能够处理身份验证的 cookie、 检查有效的登录名、 出等用户直接登录。但是，在 ASP.NET 1.x 应用程序中实现窗体身份验证可能需要大量的代码。

在 ASP.NET 2.0 中的成员身份是通过使用单独的窗体身份验证一次重大进步。 （成员身份是最强健结合了窗体身份验证，但使用 Forms 身份验证并不是必需）。正如您很快将看到，你可以使用 ASP.NET 成员资格和登录控件中 ASP.NET 2.0 来实现功能强大的成员身份系统而无需在所有编写大量的代码。

## <a name="implementing-membership-in-aspnet-20"></a>在 ASP.NET 2.0 中实现的成员身份

成员身份由以下四个步骤来实现。 请记住，有许多子步骤，以及可以以及实现的可选配置涉及。 这些步骤是为了演示的配置成员资格的大体形势。

1. 创建成员资格数据库 （如果使用的 SQL Server 作为成员资格存储。）
2. 在你的应用程序配置文件中指定的成员身份选项。 （成员资格启用默认情况下中）。
3. 确定你想要使用的成员身份存储的类型。 选项包括： 

    - Microsoft SQL Server （版本 7.0 或更高版本）
    - Active Directory Store
    - 自定义成员资格提供程序
4. 配置 ASP.NET 表单身份验证的应用程序。 同样，成员身份旨在充分利用 Forms 身份验证，但使用 Forms 身份验证并不是必需。
5. 定义成员身份的用户帐户和配置角色，如果需要。

## <a name="creating-the-membership-database"></a>创建成员资格数据库

如果您正在使用 SQL Server 7.0 或更高版本作为成员资格存储，可以使用 aspnet\_regsql 实用程序 （可非常轻松地从 Visual Studio.NET 2005年命令提示） 来配置你的数据库。 Aspnet\_作为命令提示符工具或通过 GUI 向导，可以使用 regsql 实用工具。 向导方法是配置你的数据库的最简单方法。 若要访问该向导，只需运行以下命令：

`aspnet_regsql W`

一旦你运行该命令，就将如下所示显示与 ASP.NET SQL Server 安装向导。


![](membership/_static/image1.jpg)

**图 1**


ASP.NET SQL Server 安装向导在向导中指定的实例中创建网站。 但是，ASP.NET 将使用连接字符串在 machine.config 文件中连接到你的数据库。 默认情况下，此连接字符串将指向的 SQL Server 2005 实例，因此如果你使用的 SQL Server 2000 或 SQL Server 7.0 实例，你将需要修改 machine.config 文件中的连接字符串。 该连接字符串可以位于此处：

[!code-xml[Main](membership/samples/sample1.xml)]

遗憾的是，如果你无需修改连接字符串，ASP.NET 将不为你提供描述性错误。 它将只需继续抱怨说你尚未创建数据库。 在上面的示例中，我已修改为指向我的本地 SQL Server 2000 实例的连接字符串。

## <a name="specifying-configuration-and-adding-users-and-roles"></a>指定配置和添加用户和角色

配置成员资格的下一步是将所需的信息添加到应用程序的 web.config 文件。 在 ASP.NET 1.x，修改 web.config 文件是有时很困难由于 lowerCamelCase 利用和缺少的 Intellisense 问题。 Visual Studio.NET 2005年使任务可以更加容易与 Intellisense 一起配置文件，但 ASP.NET 2.0 就更进一步提供进行编辑配置文件的 Web 界面。

你可以通过单击如下所示的解决方案资源管理器工具栏上的 ASP.NET 配置按钮启动 Web 接口。 你也可以启动通过插入登录控件时显示的弹出窗口的 Web 界面。


![](membership/_static/image2.jpg)

**图 2**


这将启动 ASP.NET 网站管理工具如下所示。 ASP.NET 网站管理是一个四个选项卡接口，可以轻松地管理应用程序设置。 提供了以下选项卡：

- **主页**
- **安全**配置用户、 角色和访问。
- **应用程序**配置应用程序设置。
- **提供程序**配置和测试应用程序成员资格提供程序。

网站管理工具，可以轻松地创建新用户，创建新的角色，以及管理用户和角色。 此功能在 Windows 界面中不可用。 Windows 界面，您可以轻松地定义授权设置和添加、 删除和管理提供程序，不在网站管理工具的功能。

若要启动 Windows 界面，打开 Internet Information Services 管理单元，右键单击你的应用程序，然后选择属性。 单击 ASP.NET 选项卡，然后单击编辑配置按钮。 （若要启用编辑配置按钮的 ASP.NET 2.0 下必须运行应用程序。 你可以配置 ASP.NET 版本在 ASP.NET 对话框。）ASP.NET 配置设置对话框将显示如下所示。


![](membership/_static/image3.jpg)

**图 3**


在常规选项卡上列出了连接字符串和应用程序设置。 在父配置文件 （machine.config 或在较高级别 web.config） 中定义将以斜体显示的任何设置，并不将以斜体显示的设置从应用程序配置文件。 如果添加设置，则已删除或编辑应用程序级别中，ASP.NET 将添加、 删除或修改在应用程序级别 web.config 而不是从它继承的配置文件中删除该设置的设置。

身份验证选项卡如下所示。 这是你将在其中配置成员身份设置。 窗体身份验证设置，成员资格提供程序和角色提供程序可以在此处配置。


![](membership/_static/image4.jpg)

**图 4**


## <a name="implementing-membership-in-your-application"></a>在你的应用程序中实现的成员身份

在你的应用程序中实现 ASP.NET 2.0 成员身份的最简单方法是使用提供的登录控件。 此方法可以实现 ASP.NET 2.0 成员资格的基础知识，而无需编写任何代码。

以下的登录控件是在 ASP.NET 2.0 中可用：

## <a name="login-control"></a>登录控件

该登录控件提供一个接口，使用户能够登录到成员资格系统。 它提供的用户名和密码 textboxt 和登录按钮。 许多其他常见功能，如将其注册为那些尚未完成使一个复选框，使用户可以自动在后续的访问的登录名的用户的链接、 密码提示，等等的链接。登录控件的所有功能都都通过控件的属性可自定义。

在 ASP.NET 中 1.x，开发人员必须编写大量代码以使用窗体身份验证时执行查找。 具有 ASP.NET 2.0 成员身份，你可以无需编写任何代码在所有验证用户。 ASP.NET 自动将为您完成用户的查找。 (如果使用 Login 控件，而不使用 ASP.NET 成员资格，则可以使用**OnAuthenticate**方法以验证此用户。)

## <a name="loginview-control"></a>LoginView 控件

LoginView 控件是一个模板化控件，默认情况下; 提供两个模板AnonymousTemplate 和 LoggedInTemplate。 显示的模板取决于将用户登录到成员资格系统。 此控件通常用于显示用户尚未登录时的登录控件和 LoginStatus 控制和/或其他登录控件时，用户已登录。 如果你正在使用 ASP.NET 应用程序中的角色管理，LoginView 控件可以显示特定的模板基于用户角色。 （更多关于 ASP.NET 角色管理将在后面说明。）

## <a name="passwordrecovery-control"></a>取回控件

取回控件允许用户收到一封电子邮件使用他或她的当前密码或重置他或她的密码。 明文形式和加密的密码可以恢复并且通过电子邮件发送给用户。 如果密码哈希处理，将无法恢复。 而是用户将需要以执行密码重置。

## <a name="loginstatus-control"></a>LoginStatus 控件

LoginStatus 控件用于向当前登录用户显示的用户未登录的登录名指示器和注销指示器。 Request.IsAuthenticated 属性用于确定哪些标记，以显示。 LoginStatus 控件显示的指示器可以是文本 (通过实现**LoginText**和**LogoutText**属性) 或图像 (通过实现**LoginImageUrl**和**LogoutImageUrl**属性。)

当用户注销通过 LoginStatus 控制时，他或她将重定向到指定的 URL **LogoutPageUrl**属性。 如果未设置该属性，则会刷新当前页。 站点可能受窗体身份验证，因为当前页的刷新将用户重定向到该网站的登录页。

## <a name="loginname-control"></a>LoginName 控件

LoginName 控件显示当前登录到网站的用户的用户名。

## <a name="createuserwizard-control"></a>通过

通过向用户提供一种简便方式注册你的成员身份系统。 你可以通过如下所示的界面添加步骤 （实现为一个 WizardSteps 的集合）。


![](membership/_static/image5.jpg)

**图 5**


CreateUserWizard 是一个模板化控件，从向导类派生并提供了以下模板：

- **HeaderTemplate**此模板控制的标头的向导的外观。
- **SidebarTemplate**此模板控制的侧栏向导的外观。
- **StartNavigationTemplate**在导航窗格的外观开始步骤在向导的此模板控制。
- **StepNavigationTemplate**此模板控制在不启动或完成步骤时的导航区域的外观。
- **FinishNavigationTemplate**此模板控制时在完成步骤的导航区域中的外观。

此外，对于为向导添加每个步骤，ASP.NET 将创建包含 ContentTemplate 和为该步骤 CustomNavigationTemplate 的自定义模板。 有关自定义 CreateUserWizard 的完整详细信息，请参阅 VS.NET 2005 文档：

## <a name="changepassword-control"></a>ChangePassword 控件

ChangePassword 控件允许用户更改其密码。 如果 DisplayUserName 属性为 true （它位于 false 默认情况下），用户可以在用户未登录时更改他或她的密码。 如果用户*是*已登录和 DisplayUserName 属性为 true 时，用户将能够更改不记录在提供他们知道该用户的用户 ID 的另一个用户的密码。

请记住，如果你希望用户能够更改密码，而无需登录，你将需要确保 ChangePassword 控件显示在其的页面允许匿名访问。 显然，用户将必须提供旧密码，以更改其密码。

## <a name="role-management"></a>角色管理

角色管理，可将用户分配到特定角色，然后限制对特定文件或文件夹基于该角色的访问。 角色管理还提供一个 API，以便你可以以编程方式确定 someones 角色或确定对特定角色中的所有用户并相应地做出响应。

角色管理不是 ASP.NET 成员身份要求也不是为了使用角色管理要求的成员身份。 但是，这两个很好地对相互进行补充，以及有可能，开发人员将使用这些与相互结合使用。

若要启用应用程序中的角色管理，请在 web.config 文件中进行以下更改：

[!code-xml[Main](membership/samples/sample2.xml)]

当**cacheRolesInCookie**属性设置为 true，ASP.NET 缓存客户端的 cookie 中的用户角色的成员身份。 这允许角色查找，以调入 RoleProvider 的情况下发生。 当使用此属性，开发人员建议确保**cookieProtection**属性将设为 All。 （这是默认设置）。这可确保 cookie 数据进行加密，并可帮助确保的 cookie 内容尚未被更改。 可以使用网站管理工具添加角色。 它允许你轻松地定义角色、 配置的访问权限的基于这些角色的站点部分并将用户分配到角色。


![](membership/_static/image6.jpg)

**图 6**


如上所示，可以通过只需输入角色的名称，然后单击添加角色添加新角色。 可以管理或通过单击相应的链接的现有角色列表中删除现有的角色。

当你管理角色时，你可以添加或删除用户，如下所示。


![](membership/_static/image7.jpg)

**图 7**


通过检查角色中用户是复选框，可以轻松地将用户添加到特定角色中。 ASP.NET 将使用适当的项自动更新成员资格数据库。 你还需要配置你的应用程序的访问规则。 ASP.NET 1.x 开发人员熟悉这样通过&lt;授权&gt;web.config 文件中和该选项中的元素是 ASP.NET 2.0 中仍然可用。 但是，轻松地管理访问权限的规则使用网站管理工具如图所示下面。


![](membership/_static/image8.jpg)

**图 8**


在这种情况下，突出显示的管理文件夹 （其难看到，因为该工具使其突出显示以浅灰色） 并且已被管理员角色授予访问权限。 将拒绝所有其他用户。 你可以单击头图标以选择一个规则，然后使用上移和下移按钮将规则调整。 ASP.NET 与&lt;授权&gt;元素，它们出现的顺序处理规则。 换而言之，如果在上面的截图中的规则的顺序被取消，没有人就可以访问管理文件夹因为 ASP.NET 将会遇到的第一个规则将拒绝所有人的文件夹的规则。

ASP.NET 2.0 将 web.config 文件添加到要为其指定访问规则的文件夹。 通过配置文件或网站管理工具，可以编辑访问规则。 换而言之，网站管理工具是只需通过其可以在用户友好的环境中编辑配置文件的接口。

## <a name="using-roles-in-code"></a>在代码中使用角色

用于角色管理的 API 不自从版本 1.x。 **IsInRole**方法用于确定用户是否对特定角色中。

[!code-csharp[Main](membership/samples/sample3.cs)]

为当前上下文的成员，ASP.NET 还创建 RolePrincipal 实例。 RolePrincipal 对象可以用于获取所有角色与用户所属，如下所示：

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>RoleGroups 使用 LoginView 控件

现在，你已了解角色管理和成员身份，允许简要讨论如何 LoginView 控件利用此功能在 ASP.NET 2.0。 如前面所述，LoginView 控件是一个模板化的控件，默认情况下; 包含两个模板AnonymousTemplate 和 LoggedInTemplate。 LoginView 任务对话框是 （如下所示） 的链接，您可以编辑 RoleGroups。


![](membership/_static/image9.jpg)

**图 9**


每个 RoleGroup 对象包含定义 RoleGroup 适用于哪些角色的字符串数组。 若要将新 RoleGroup 添加到 LoginView 控件，请单击编辑 RoleGroups 链接。 在上图中，你可以看到我已将添加为管理员新 RoleGroup。 通过选择该 RoleGroup （RoleGroup[0]) 从视图下拉列表中，可以配置将仅显示管理员角色的成员的模板。 在下图中，我添加了新 RoleGroup 适用于 Sales 角色和分发角色的成员。 这会向登录视图任务对话框中的视图下拉列表中添加第二个 RoleGroup 而不显示任何内容添加到该模板中的销售或分发的任何用户角色。


![](membership/_static/image10.jpg)

**图 10**


## <a name="overriding-the-existing-membership-provider"></a>重写现有成员资格提供程序

有几种方法，你可以扩展的 ASP.NET 成员资格功能。 首先，您可以通过从它继承并重写其方法显然更改 SqlMembershipProvider 类的现有功能。 例如，如果你想要实现您自己的功能，在创建用户时，你可以创建您自己的类继承自 SqlMembershipProvider，如下所示：

[!code-csharp[Main](membership/samples/sample5.cs)]

如果在另一方面，你想要创建自己的提供程序 （用于在访问数据库中，存储成员身份信息，例如所示），你可以创建自己的提供程序。

## <a name="creating-your-own-membership-provider"></a>创建你自己的成员资格提供程序

若要创建你自己的成员资格提供程序，首先需要创建从 MembershipProvider 类继承的类。 如果使用 VB.NET，Visual Studio 2005 将添加所有需要重写的方法存根。 如果你使用的 C#，其最多可以添加存根 （stub）。

你将需要重写以下：

- 应用程序名称属性
- ChangePassword 函数
- ChangePasswordQuestionAndAnswer 函数
- CreateUser 函数
- DeleteUser 函数
- EnablePasswordReset 属性
- EnablePasswordRetrieval 属性
- FindUsersByEmail 函数
- FindUsersByName 函数
- GetAllUsers 函数
- GetNumberOfUsersOnline 函数
- GetPassword 函数
- GetUser 函数
- GetUserNameByEmail 函数
- MaxInvalidPasswordAttempts 属性
- MinRequiredNonAlphanumericCharacters 属性
- MinRequiredPasswordLength 属性
- PasswordAttemptWindow 属性
- PasswordFormat 属性
- PasswordStrengthRegularExpression 属性
- RequiresQuestionAndAnswer 属性
- RequiresUniqueEmail 属性
- ResetPassword 函数
- 解锁用户函数
- UpdateUser 函数
- ValidateUser 函数

这是要作为 C# 开发人员实现相当多的列表。 你可能会发现更轻松地在 VB.NET 中创建类，而无需任何实现，然后使用.NET 反射器或类似的工具将代码转换为 C#。

连接字符串和其他属性应设置为各自的 Initialize 方法中的默认值。 （Initialize 方法时激发的提供程序是在运行时加载。）Initialize 方法的第二个参数是类型 System.Collections.Specialized.NameValueCollection 并且是指&lt;添加&gt;web.config 文件中的自定义提供程序与关联的元素。 该条目如下所示：

[!code-xml[Main](membership/samples/sample6.xml)]

下面是初始化方法的示例。

[!code-csharp[Main](membership/samples/sample7.cs)]

若要验证用户在提交登录窗体时，你将需要使用 ValidateUser 方法。 当用户单击 Login 控件中的登录按钮，将引发此方法。 请将你执行此方法中的用户查找的代码。

如你所见，编写您自己的成员资格提供程序并不困难，并允许你将扩展此功能强大的功能的 ASP.NET 2.0。
