---
uid: web-pages/overview/security/16-adding-security-and-membership
title: "将安全和成员身份添加到 ASP.NET Web 页 (Razor) 站点 |Microsoft 文档"
author: tfitzmac
description: "本章展示如何保护你的网站，以便仅供登录的人员的某些页。 （你还将了解如何创建页 tha..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: af2eeb128cff554e7ae3d903e2117861087344e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>将安全和成员身份添加到 ASP.NET 网站页 (Razor)
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何保护 ASP.NET Web 页 (Razor) 网站，以便仅供登录的人员的某些页。 （你将另请参阅如何创建任何人都可以访问的页面。）
> 
> **你将学习：** 
> 
> - 如何创建了一个注册页，并且登录页，因此，对于某些页中，你可以限制对仅成员的访问的网站。
> - 如何创建公共和仅限成员的页。
> - 如何定义角色，是在你的站点具有不同的安全权限的组，以及如何将用户分配到角色。
> - 如何使用 CAPTCHA 以防止自动的程序 （机器人） 创建帐户的成员。
>   
> 
> 这些是文章中引入的 ASP.NET 功能：
> 
> - WebMatrix**入门站点**模板。
> - `WebSecurity`帮助器和`Roles`类。
> - `ReCaptcha`帮助器。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web 帮助程序库


你可以设置你的网站，以便用户可以登录到 （&） #8212;也就是说，以便站点支持*成员资格*。 这可以是有用出于许多原因。 例如，你的站点可能具有可仅对成员的页。 在某些情况下，你可能会要求用户进行登录，才能向你发送反馈或发表评论。

即使你的网站支持成员资格，则用户不一定需要登录，然后在站点上使用的某些页。 没有登录的用户被称为*匿名用户*。

用户可在网站上注册，并可以然后登录到站点。 网站需要用户名 （电子邮件地址） 和密码以确认用户所声称的身份。 登录并确认用户的标识的此过程被称为*身份验证*。

你可以设置安全和成员身份以不同的方式：

- 如果你使用的 WebMatrix，一种简便方法是创建基于新站点作为**入门站点**模板。 此模板已配置为安全和成员身份，并已注册页、 登录页上，依次类推。

    由模板创建的站点也有一个选项以让用户使用如 Facebook、 Google、 外部站点登录或 Twitter。
- 如果你想要将安全添加到现有站点，或如果你不想要使用**入门站点**模板，您可以创建你自己的注册页、 登录页上，依次类推。

本文重点介绍在第一个选项&mdash;如何通过使用添加安全**入门站点**模板。 它还提供了有关如何实现你自己的安全一些基本信息，然后提供指向有关如何执行该操作的详细信息。 此外，还有若要了解如何启用外部登录名，在单独的文章中详细的介绍。

## <a name="creating-website-security-using-the-starter-site-template"></a>创建使用初学者站点模板的网站安全

在 WebMatrix 中，你可以使用**入门站点**模板来创建包含以下网站：

- 用于存储用户名和密码，为了使成员数据库。
- （新） 的匿名用户可以在其中注册注册页。
- 登录和注销页。
- 密码恢复和重置页。

以下过程描述如何创建站点，并对其进行配置。

1. 启动 WebMatrix 和在**快速启动**页上，选择**站点从模板**。
2. 选择**入门站点**模板，然后单击**确定**。 WebMatrix 创建新的站点。
3. 在左窗格中，单击**文件**工作区中选择器。
4. 在你的网站的根文件夹，打开 *\_AppStart.cshtml*文件，它是一个特殊文件，用于包含全局设置。 它包含已被注释掉使用的一些语句`//`字符：

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    这些语句配置`WebMail`帮助器，可用于发送电子邮件。 成员资格系统可以使用电子邮件发送确认消息，当用户注册时或当他们想要更改其密码。 （例如，用户注册后，它们收到的电子邮件，其中包含一个链接，则可以单击为了完成注册过程。）

    发送电子邮件都需要访问 SMTP 服务器时中, 所述[添加到 ASP.NET Web Pages 站点的电子邮件](https://go.microsoft.com/fwlink/?LinkId=202899)。 将此 central 中存储电子邮件设置 *\_AppStart.cshtml*文件，以便无需到可以发送电子邮件的每一页重复代码。 （不需要配置 SMTP 设置，以将注册数据库设置; 如果你想要验证用户从其电子邮件别名，并让用户重置忘记了的密码，则会仅需要 SMTP 设置。）
5. 取消语句注释通过删除`//`每个前面。

    如果您不想要设置电子邮件确认，则可以跳过此步骤和下一步。 如果未设置 SMTP 值，则新的帐户是立即可用，而无需确认电子邮件。
6. 修改代码中电子邮件相关的以下设置：

    - 设置`WebMail.SmtpServer`到可以访问 SMTP 服务器的名称。
    - 保留`WebMail.EnableSsl`设置为`true`。 此设置能保护对它们进行加密发送到 SMTP 服务器的凭据。
    - 设置`WebMail.UserName`到您的 SMTP 服务器帐户的用户名。
    - 设置`WebMail.Password`到您的 SMTP 服务器帐户的密码。
    - 设置`WebMail.From`为您自己的电子邮件地址。 这是从发送消息的电子邮件地址。

    > [!NOTE] 
    > 
    > **提示**有关这些属性的值的其他信息，请参阅[配置电子邮件设置](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings)中[自定义站点范围的行为的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906)。
7. 保存并关闭 *\_AppStart.cshtml*。
8. 运行*Default.cshtml*在浏览器中的页。

    ![security-membership-2](16-adding-security-and-membership/_static/image1.png)

    > [!NOTE]
    > 如果你看到错误，告知你属性必须为实例`ExtendedMembershipProvider`，不可能将站点配置为使用 ASP.NET Web Pages 成员资格系统 (SimpleMembership)。 如果宿主提供程序的服务器配置不同于你的本地服务器，有时会出现此问题。 若要解决此问题，请将下面的元素添加到站点的*Web.config*文件：
    > 
    > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
    > 
    > 添加此元素的子级作为`<configuration>`元素的对等方的以及`<system.web>`元素。
9. 在页面右上角，单击**注册**链接。 *Register.cshtml*显示页。
10. 输入用户名和密码，然后单击**注册**。

    ![security-membership-3](16-adding-security-and-membership/_static/image2.png)

    当创建对网站从**入门站点**模板、 一个名为数据库*StarterSite.sdf*在站点中创建*应用\_数据*文件夹。 在注册期间，你的用户信息添加到数据库。 如果设置 SMTP 值时，一条消息是发送到电子邮件地址中，使用使你可以完成注册。

    ![security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. 转到你的电子邮件程序，查找的消息，这将到站点中具有你确认代码链接和超链接。
12. 单击超链接以激活你的帐户。 确认超链接打开注册确认页。

    ![security-membership-5](16-adding-security-and-membership/_static/image4.png)
- 单击**登录**链接，然后使用你注册的帐户，然后登录。

    你登录后**登录**和**注册**链接替换为**注销**链接。 您的登录名显示为链接。 （链接使你能够转到一个页面，你可以在其中更改你的密码。）

    ![security-membership-6](16-adding-security-and-membership/_static/image5.png)

    > [!NOTE]
    > 默认情况下，ASP.NET 网页将凭据发送到服务器以明文形式 （作为用户可读文本）。 生产站点应使用安全 HTTP (也称为 https://*安全套接字层*或 SSL) 与的服务器交换的敏感信息进行加密。 你可以所需的电子邮件发送的消息使用 SSL 通过设置`WebMail.EnableSsl=true`如同前面的示例。 有关 SSL 的详细信息，请参阅[保护 Web 通信： 证书、 SSL 和 https://](https://go.microsoft.com/fwlink/?LinkId=208660)。

## <a name="additional-membership-functionality-in-the-site"></a>站点中的其他成员身份功能

您的网站包含其他功能，从而让用户管理他们的帐户。 用户可以执行以下操作：

- 更改其密码。 登录后，他们可以单击的用户名 （这是一个链接）。 这使它们转到一个页面它们可以在其中创建新的密码 (*Account/ChangePassword.cshtml*)。
- 恢复忘记的密码。 在登录页上，是的链接 (**是否忘记了密码？**) 采用用户定向到页 (*Account/ForgotPassword.cshtml*) 它们可以在其中输入电子邮件地址。 站点将其发送电子邮件具有一个链接，则可以单击以设置新密码 (*Account/PasswordReset.cshtml*)。

你还可以让用户还可以使用外部站点，登录，详见后面。

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>创建仅限成员页

目前，任何人都可以浏览到你的网站中的任意页。 但你可能想要仅适用于已登录的人员的页 (即，对成员)。 ASP.NET 允许您创建可以访问只能由登录的成员的页。 通常情况下，如果匿名用户尝试访问仅限成员的页，你将它们重定向到登录页。

在此过程中，将创建一个文件夹将包含仅适用于登录的用户的页。

1. 在站点的根目录，创建一个新文件夹。 (在功能区中，单击下的箭头**新建**，然后选择**新文件夹**。)
2. 将新文件夹命名*成员*。
3. 内部*成员*文件夹中，创建一个新页，并将其命名为*MembersInformation.cshtml*。
4. 将现有内容替换下面的代码和标记：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    此代码测试`IsAuthenticated`属性`WebSecurity`对象，它返回`true`如果用户已登录。 如果用户未登录，该代码调用`Response.Redirect`发送到用户*Login.cshtml*页面*帐户*文件夹。

    重定向 URL 包括`returnUrl`查询使用的字符串值`Request.Url.LocalPath`设置当前页的路径。 如果你设置`returnUrl`如下查询字符串中的值 （和返回 URL 是否是本地路径），登录页将用户后返回到此页在登录。

    此代码还设置 *\_SiteLayout.cshtml*作为其布局页的页。 (有关详细信息布局页，请参阅[在 ASP.NET 网页站点中创建一致的布局](https://go.microsoft.com/fwlink/?LinkId=202891)。)
5. 运行该站点。 如果你仍登录，请单击**注销**页顶部的按钮。
6. 在浏览器中，请求页*/Members/MembersInformation*。 例如，URL 可能如下所示：

    `http://localhost:38366/Members/MembersInformation`

    （端口号 (38366) 可能将你 URL 中不同。）

    要重定向到*Login.cshtml*页上，因为您没有登录。
- 使用前面创建的帐户登录。 要重定向回*MembersInformation*页。 因为在你登录的此时会显示页面内容。

若要保护对多个页的访问，可以执行此操作：

- 添加到每个页面的安全检查。
- 创建 *\_PageStart.cshtml*页中的文件夹，其中保留受保护的页，并添加的安全检查。 *\_PageStart.cshtml*页用作一种类型的全局文件夹中的所有页的页。 此技术中的详细说明了[自定义站点范围的行为的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access)。

## <a name="creating-security-for-groups-of-users-roles"></a>创建安全组的用户 （角色）

如果你的站点具有大量成员，它不是有效之前让他们看到一个页面，分别检查每个用户的权限。 你可以执行的操作相反的是，创建组，或*角色*，各个成员属于。 然后，你可以检查基于角色的权限。 在本部分中，你将创建&quot;管理员&quot;角色，然后创建用户都可以访问的页面中 （属于） 该角色。

ASP.NET 成员资格系统设置以支持角色。 但是，与成员身份注册和登录名，不同**入门站点**模板不包含页，可帮助你管理角色。 （管理角色是一个管理任务，而不是一个用户任务。）但是，您可以直接在 WebMatrix 中的成员资格数据库中添加组。

1. 在 WebMatrix 中，单击**数据库**工作区中选择器。
2. 在左窗格中，打开*StarterSite.sdf*节点，打开**表**节点，然后再双击*网页\_角色*表。

    ![security-membership-7](16-adding-security-and-membership/_static/image6.png)
3. 添加名为的角色&quot;管理员&quot;。 *RoleId*自动填写字段。 (它是主键，已设置为标识字段中中, 所述[使用 ASP.NET Web Pages 站点中的数据库的简介](https://go.microsoft.com/fwlink/?LinkId=202893)。)
4. 请记下的值是什么*RoleId*字段。 （如果这是你定义的第一个角色，它将是 1）。

    ![security-membership-8](16-adding-security-and-membership/_static/image7.png)
5. 关闭*网页\_角色*表。
6. 打开*UserProfile*表。
7. 记下*UserId*的一个或多个表，然后关闭表中的用户的值。
8. 打开*网页\_UserInRoles*表，然后输入*UserID*和*RoleID*到表的值。 例如，若要将用户 2 到&quot;管理员&quot;角色，则输入这些值：

    ![security-membership-9](16-adding-security-and-membership/_static/image8.png)
9. 关闭*网页\_UsersInRoles*表。

    现在，你已定义的角色，你可以配置该角色中的用户可以访问的页面。
10. 在网站根文件夹中，创建一个名为的新页*AdminError.cshtml*并将现有内容替换为下面的代码。 这将是将用户重定向到如果它们不允许访问的页的页。

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. 在网站根文件夹中，创建一个名为的新页*AdminOnly.cshtml*和替换为以下代码替换现有代码：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole`方法返回`true`当前用户是否属于指定角色 （在此情况下，"管理员"角色）。
12. 运行*Default.cshtml*在浏览器中，但不登录。 （如果你已登录，注销。）
13. 在浏览器的地址栏中，添加*AdminOnly*在 URL 中。 (在换而言之，请求*AdminOnly.cshtml*文件。)要重定向到*AdminError.cshtml*页上，因为你没有当前登录中的用户身份&quot;管理员&quot;角色。
14. 返回到*Default.cshtml*和添加到用户身份登录&quot;管理员&quot;角色。
15. 浏览到*AdminOnly.cshtml*页。 此时会显示页。

## <a name="preventing-automated-programs-from-joining-your-website"></a>阻止自动的程序加入你的网站

登录页不会停止自动的程序 (有时称为*web 机器人*或*机器人*) 注册与您的网站。 此过程介绍如何启用注册页的 ReCaptcha 测试。

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. 注册你的网站在 ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net))。 你已完成注册，你会获得一个公钥和私钥。
2. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未。
3. 在*帐户*文件夹中，打开的文件命名为*Register.cshtml*。
4. 在页面顶部的代码中，找到以下行并取消其注释通过删除`//`注释字符：

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. 替换`PRIVATE_KEY`替换为你自己 ReCaptcha 私有密钥。
6. 在页面的标记中，删除`@*`和`*@`周围的页标记中的以下行的注释字符：

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. 替换`PUBLIC_KEY`与你的密钥。
8. 如果你尚未已删除它，则删除`<div>`包含文本"若要启用 CAPTCHA 验证..."开头的元素。 (删除整个`<div>`元素和其内容。)

1. 运行*Default.cshtml*在浏览器。 如果你登录到站点，请单击**注销**链接。
2. 单击**注册**链接以及测试使用 CAPTCHA 测试的注册。

    ![security-membership-10](16-adding-security-and-membership/_static/image9.png)

有关详细信息`ReCaptcha`帮助器，请参阅[到防止自动程序 （机器人） 从使用你的 ASP.NET Web 站点使用 CATPCHA](https://go.microsoft.com/fwlink/?LinkId=251967)。

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>允许用户在使用外部站点进行登录

**入门站点**模板包括代码和标记，从而让用户使用 Facebook、 Windows Live、 Twitter、 Google 或 Yahoo 登录。 默认情况下，不启用此功能。 一般的过程使用让用户使用这些外部提供程序的登录名是此：

- 确定你想要支持哪个外部站点。
- 如果需要，转到该网站并设置登录应用。 （例如，你必须这样做以便允许 Facebook 登录名。）
- 在你站点中，配置提供程序。 在大多数情况下，你只需取消注释中的某些代码 *\_AppStart.cshtml*文件。
- 将标记添加到让用户能够注册页登录的外部站点的链接。 通常，你可以复制需要并略有更改的文本的标记。

你可以在主题中找到的分步说明[启用从 ASP.NET Web Pages 站点中的外部网站的登录名](https://go.microsoft.com/fwlink/?LinkId=251969)。

用户登录从另一个站点后，用户返回到你的站点和*将相关联*该登录名与您的网站。 实际上，这将在用户的外部登录你站点中创建成员身份条目。 这允许你使用的外部登录名使用 （例如角色） 的成员身份的正常功能。

## <a name="adding-security-to-an-existing-website"></a>将安全添加到现有网站

在本文前面的过程依赖于使用**入门站点**模板作为网站的安全性的基础。 如果它不适合将你从启动**入门站点**模板或者若要从基于该模板站点复制相关页，你可以实现相同类型的安全自己站点中通过自己编码。 创建相同类型的页-注册、 登录名，等等 — 然后使用帮助器和类将成员资格设置。

基本过程篇博客文章中所述[最基本的方法来实现 ASP.NET Razor 安全](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)。 完成大部分工作的以下方法和属性使用`WebSecurity`帮助器：

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx)， [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx)。 这些方法使你可以确定是否有人已注册并对它们进行注册。
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). 此属性允许您确定当前用户是否已登录。 这可用于将用户重定向到登录页，如果它们未已登录。
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx)， [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx)。 这些方法将用户登录，或缩小。
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). 此属性可用于显示当前用户的登录名 （如果用户已登录）。
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). 此方法是设置注册的电子邮件确认的情况下很有用。 (详细信息所述的博客文章[确认功能用于 ASP.NET Web Pages 安全](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)。)

若要管理角色，可以使用[角色](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx)和[成员资格](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx)类，如博客文章中所述。

## <a name="additional-resources"></a>其他资源

- [自定义站点范围内的行为](https://go.microsoft.com/fwlink/?LinkId=202906)
- [保护 Web 通信： 证书、 SSL 和 https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [最基本的方法来实现 ASP.NET Razor 安全](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240)和[确认功能用于 ASP.NET Web Pages 安全](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)。 这些是描述如何实现 ASP.NET 成员资格功能而无需使用的博客文章**入门站点**模板。
- [在 ASP.NET 网站中启用从外部站点进行登录](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity 类 API 参考](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99))(MSDN)
- [SimpleRoleProvider 类 API 参考](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99))(MSDN)
- [SimpleMembershipProvider 类 API 参考](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99))(MSDN)
