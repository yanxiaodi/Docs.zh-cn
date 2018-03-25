---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: "正在验证用户凭据对成员资格用户存储区 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将了解如何验证成员资格用户存储区使用的编程方法和登录控制针对用户的凭据..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f8f4db63ba8c1f1c1df7c1c5c1f92184bf6841d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>正在验证用户凭据对成员资格用户存储区 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip)或[下载 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> 在本教程中我们将查看如何验证针对使用的编程方法和登录控件成员资格用户存储区的用户的凭据。 我们还将了解如何自定义登录控件的外观和行为。


## <a name="introduction"></a>介绍

在<a id="Tutorial05"> </a>[前面教程](creating-user-accounts-cs.md)我们介绍了如何在成员资格框架中创建新的用户帐户。 我们首先看以编程方式创建用户帐户通过`Membership`类的`CreateUser`方法，然后使用和检查 CreateUserWizard Web 控件。 但是，登录页当前验证根据硬编码的用户名和密码对的列表提供的凭据。 我们需要更新登录页的逻辑，以便它验证针对成员资格框架的用户存储的凭据。

更像与创建用户帐户，可以验证凭据以编程方式或以声明方式。 成员资格 API 包含用于以编程方式验证对用户存储的用户的凭据的方法。 和 ASP.NET 附带登录 Web 控件，来呈现使用文本框中输入用户名和密码和一个按钮以登录的用户界面。

在本教程中我们将查看如何验证针对使用的编程方法和登录控件成员资格用户存储区的用户的凭据。 我们还将了解如何自定义登录控件的外观和行为。 让我们进入正题！

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>步骤 1： 验证针对成员资格用户存储凭据

对于使用 forms 身份验证的网站，用户登录到网站访问登录页并输入其凭据。 然后，针对用户存储比较这些凭据。 如果它们有效，然后将向用户授予窗体身份验证票证，这是安全令牌，该值指示的标识和访问者的真实性。

若要验证对成员资格 framework 用户，使用`Membership`类的[`ValidateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)。 `ValidateUser`方法采用两个输入参数-中 *`username`* 和 *`password`*  -并返回一个布尔值，该值指示凭据是否有效。 与类似`CreateUser`方法，在前面的教程，我们探讨`ValidateUser`方法会委托给配置成员资格提供程序实际的验证。

`SqlMembershipProvider`验证提供的凭据通过获取指定的用户的密码通过`aspnet_Membership_GetPasswordWithFormat`存储过程。 回想一下，`SqlMembershipProvider`存储使用三种格式之一的用户的密码： 清除，加密，或哈希处理。 `aspnet_Membership_GetPasswordWithFormat`存储的过程返回其原始格式的密码。 加密或经过哈希处理密码，`SqlMembershipProvider`转换 *`password`* 值传递给`ValidateUser`方法划分为它的等效项加密或哈希状态，然后将它与什么返回从进行比较数据库。 如果存储在数据库中的密码与用户输入的格式化的密码匹配，则凭据有效。

让我们更新我们的登录页 (~ /`Login.aspx`)，以便它会验证对成员资格 framework 用户存储区提供的凭据。 我们创建了此登录页进来<a id="Tutorial02"> </a> [*概述窗体身份验证的*](../introduction/an-overview-of-forms-authentication-cs.md)教程中，使用两个文本框中输入用户名和密码，创建一个接口记住我复选框，和登录按钮 （请参见图 1）。 代码验证输入的凭据与硬编码的用户名和密码对 （Scott/密码、 Jisun/密码和 Sam/密码） 列表。 在<a id="Tutorial03"> </a> [*窗体身份验证配置和高级主题*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)我们更新了登录页的代码，以在窗体存储附加信息的教程身份验证票证`UserData`属性。


[![登录页的接口包含两个文本框中，按和一个按钮](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**图 1**: 登录页接口包括两个文本框中，按和一个按钮 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


登录页的用户界面可以保持不变，但我们需要将登录按钮`Click`事件处理程序替换验证成员资格 framework 用户存储区对用户的代码。 更新事件处理程序，以便其代码如下所示：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

此代码是非常简单。 我们首先调用`Membership.ValidateUser`方法，并传递中提供的用户名和密码。 如果该方法返回 true，则用户登录到站点中，通过`FormsAuthentication`类的 RedirectFromLoginPage 方法。 (如我们所述<a id="Tutorial2"> </a> [*概述窗体身份验证的*](../introduction/an-overview-of-forms-authentication-cs.md)教程中，`FormsAuthentication.RedirectFromLoginPage`创建窗体身份验证票证，然后将用户重定向适当的页。）凭据无效，但是，如果`InvalidCredentialsMessage`标签会显示，从而告知用户其用户名或密码不正确。

这就是所有到它 ！

若要测试的登录页按预期方式工作，请尝试使用在前面的教程创建的用户帐户之一进行登录。 或者，如果尚未创建一个帐户，请继续并创建一个从`~/Membership/CreatingUserAccounts.aspx`页。

> [!NOTE]
> 当用户输入其凭据，并提交在登录页表单时，通过 Internet 建立到中的 web 服务器传输凭据，包括她的密码，*纯文本*。 这意味着探查的网络流量任何黑客可以查看的用户名和密码。 若要防止此情况，请务必使用加密的网络流量[安全套接字层 (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)。 这将确保凭据 （以及整个页面的 HTML 标记） 进行加密的那一刻他们离开浏览器，直到其被接收由 web 服务器。


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>成员资格 Framework 如何处理无效的登录尝试次数

当访问者达到登录页，并提交其凭据时，其浏览器将发出到登录页的 HTTP 请求。 凭据有效，则 HTTP 响应包括身份验证票证的 cookie 中。 因此，黑客尝试中断到你的站点无法创建彻底将 HTTP 请求发送到登录页，其中一个有效的用户名以及在密码猜测的程序。 如果密码猜测值正确，登录页将返回身份验证票证 cookie，此时该程序知道它已偶然发现有效的用户名/密码对。 通过暴力破解，这样的程序可能能够在用户的密码时碰到，尤其是如果密码是弱。

若要防止此类暴力破解攻击，成员身份 framework 将锁定用户是否存在一定数量的一段时间内的失败登录尝试次数。 确切参数均可通过进行以下两个成员资格提供程序配置设置：

- `maxInvalidPasswordAttempts`-指定多少无效密码前的帐户已被锁定的时间段内的用户允许使用尝试。默认值为 5。
- `passwordAttemptWindow`-指示在此期间指定的无效的登录尝试次数可导致用户帐户被锁定的分钟的时间段。默认值为 10。

如果用户已被锁定，她无法登录直到管理员将解锁其帐户。 当用户已被锁定时，`ValidateUser`方法将*始终*返回`false`，即使在提供有效凭据。 虽然此行为可以减小黑客通过暴力破解方法将到你的站点中断的可能性越小，它可以得到锁定只需忘记其密码或意外具有了 Caps Lock 或者是具有错误键入每天的有效用户。

遗憾的是，没有内置工具用于解锁的用户帐户。 若要解锁帐户，你可以修改数据库直接-更改`IsLockedOut`字段中`aspnet_Membership`表相应的用户帐户或创建基于 web 的界面，其中列出了与选项才可解锁其帐户已锁定。 我们将检查创建的管理接口，用于在将来的教程中实现常见用户帐户和角色相关任务。

> [!NOTE]
> 一个缺点`ValidateUser`方法是，提供的凭据无效时，它不提供任何原因的说明，解释。 凭据可能无效，因为没有任何匹配的用户名/密码对用户存储中，或因为用户尚未经过批准，或者用户已锁定。在步骤 4 中，我们将了解如何向用户显示更详细的消息，其登录尝试失败时。


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>步骤 2： 收集通过登录 Web 控件的凭据

[登录 Web 控件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)呈现默认用户界面非常类似于我们创建返回<a id="SKM5"> </a> [*概述窗体身份验证的*](../introduction/an-overview-of-forms-authentication-cs.md)教程。 使用 Login 控件可节省我们无需创建要收集的访问者的凭据的接口的工作。 此外，Login 控件自动签名中用户 （假设已提交的凭据有效），从而保存我们无需编写任何代码。

让我们更新`Login.aspx`、 替换手动创建的接口和代码与登录控件。 先删除现有的标记和代码中`Login.aspx`。 你可能会删除迫切地，或只需注释掉。注释掉声明性的标记，请在它与`<%--`和`--%>`分隔符。 你可以手动输入这些分隔符，或如图 2 所示，你可以选择要注释掉，然后单击工具栏中的选定的行图标出注释的文本。 同样，你可以使用注释掉的选定的行图标来注释掉的代码隐藏类中的所选代码。


[![注释掉现有声明性标记和在 Login.aspx 的源代码](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**图 2**： 注释掉现有声明性标记和中的源代码`Login.aspx`([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> 在 Visual Studio 2005 中查看的声明性的标记时，注释掉的选定的行图标不可用。 如果你不使用 Visual Studio 2008 你将需要手动添加`<%--`和`--%>`分隔符。


接下来，将从工具箱拖到页上的登录控件并设置其`ID`属性`myLogin`。 此时你的屏幕应类似于图 3。 请注意登录控件的默认接口，包括用户名和密码，记住我的 TextBox 控件下次复选框和一个按钮日志。 也有`RequiredFieldValidator`两个文本框中的控件。


[![将登录控件添加到页](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**图 3**： 将登录控件添加到页 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


大功告成 ！ 单击登录控件的登录按钮，将发生回发，并且登录控件将调用`Membership.ValidateUser`方法，同时传入的输入的用户名和密码。 如果凭据无效，Login 控件将显示一条指明此类消息。 但是，如果凭据有效，然后登录控件创建窗体身份验证票证，并将用户重定向到相应的页面。

登录控件使用四个因素来确定合适的页面，一旦成功登录到将用户重定向：

- 登录控件是否的登录页上定义`loginUrl`设置在窗体身份验证配置中; 此设置的默认值是`Login.aspx`
- 是否存在`ReturnUrl`查询字符串参数
- 登录控件的值[`DestinationUrl`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl`身份验证配置设置在窗体中指定值; 此设置的默认值为`Default.aspx`

图 4 展示了如何登录控件使用这些四个参数来到达其合适的页面决策。


[![将登录控件添加到页](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**图 4**： 将登录控件添加到页 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


需要一段时间来测试 Login 控件通过访问通过浏览器的站点，成员身份 framework 中的现有用户的身份登录。

登录控件的呈现的接口是高度可配置。 有多个属性可以影响它的外观;更重要的是，Login 控件可以转换为精确的控制布局的模板的用户界面元素。 此步骤的其余部分检查如何自定义的外观和布局。

### <a name="customizing-the-login-controls-appearance"></a>自定义登录控件的外观

登录控件的默认属性设置呈现具有 （日志中） 的标题的用户接口，对于用户名和密码输入，记住我的文本框和标签控件下次复选框和一个登录按钮。 这些元素的外观进行所有配置通过登录控件的多个属性。 而且，可以通过设置属性或两个添加附加用户界面元素-例如到页面上以创建新的用户帐户的链接。

让我们花一些时间才能 gussy 向上我们登录控件的外观。 由于`Login.aspx`页已显示登录页顶部的文本，则登录控件的标题都是多余。 因此，清除[`TitleText`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx)以删除登录控件的标题的值。

用户名: 和密码： 可以通过自定义的两个文本框中控件左侧标签[ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx)和[`PasswordLabelText`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)分别。 让我们更改用户名： 标签以读取用户名:。 标签和文本框中样式均可通过进行[ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx)和[`TextBoxStyle`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)分别。

记住我可以通过登录控件的设置下一步时间复选框的文本属性[ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)，并其默认检查状态都可通过配置[ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (默认值为 False)。 请继续并设置`RememberMeSet`将默认选中属性设置为 True，以便记住我下次复选框。

登录控制提供有关如何调整其用户界面控件的布局的两个属性。 [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx)指示是否用户名: 和密码： 左侧的其对应的文本框中 （默认值），或其上方显示标签。 [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx)指示是否用户名和密码输入且恰好垂直位于 （上面另一个） 或水平。 我要将设置为其默认值，这两个属性，但我建议你尝试将这两个属性设置为其非默认值，若要查看产生的效果。

> [!NOTE]
> 在下一部分中，配置登录控件的布局中，我们将了解使用模板来定义布局控件的用户界面元素的精确布局。


通过设置包装登录控件的属性设置[ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx)和[`CreateUserUrl`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx)与非尚未注册？ 创建帐户 ！ 和`~/Membership/CreatingUserAccounts.aspx`分别。 这会将超链接添加到我们在中创建的登录控件的接口指针指向页<a id="SKM6"> </a>[前面教程](creating-user-accounts-cs.md)。 登录控件的[ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx)和[`HelpPageUrl`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx)和[ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx)和[`PasswordRecoveryUrl`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx)即可在同样的方式呈现的帮助页和密码恢复页的链接。

进行这些属性更改后，登录控件的声明性的标记和外观应类似于图 5 所示。


[![登录控件的属性的值决定其外观](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**图 5**: 登录控件的属性的值决定其外观 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>配置登录控件的布局

登录 Web 控件的默认用户界面布局在 HTML 接口`<table>`。 但如果我们需要更好地控制呈现输出？ 我们可能想要替换`<table>`具有一系列`<div>`标记。 或者，如果我们的应用程序需要其他凭据进行身份验证？ 很多财务网站中，例如，要求用户提供不仅用户名和密码，但还个人标识号 (PIN) 或其他标识信息。 原因可能存在的内容，可将登录控件转换为模板，从中我们可以显式定义接口的声明性标记。

我们需要执行两项操作以便更新登录控件以收集其他凭据：

1. 更新以包括 Web 控件来收集其他凭据的登录控件的接口。
2. 重写登录控件的内部身份验证逻辑，以便用户仅进行身份验证，其用户名和密码有效，并且也是有效，其附加的凭据。

若要完成第一个任务，我们需要将登录控件转换为模板并添加所需的 Web 控件。 与第二个任务，则可以通过创建控件的事件处理程序取代登录控件的身份验证逻辑[`Authenticate`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)。

让我们更新 Login 控件，以便它提示用户输入其用户名、 密码和电子邮件地址，如果提供的电子邮件地址与匹配的文件在其电子邮件地址仅进行身份验证用户。 我们首先需要将登录控件的接口转换为模板。 从登录控件的智能标记上，选择转换为模板选项。


[![将登录控件转换为模板](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**图 6**： 转换为模板 Login 控件 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> 若要恢复到其预 template 版本 Login 控件，请单击从控件的智能标记的重置链接。


将登录控件转换为模板将添加`LayoutTemplate`到控件的 HTML 元素和定义的用户界面的 Web 控件与声明性标记。 如图 7 所示，将控件转换到模板中删除多个属性从属性窗口中，如`TitleText`， `CreateUserUrl`，依此类推，因为使用模板时，将忽略这些属性值。


[![较少的属性是可用的时登录控件转换为模板](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**图 7**： 较少的属性是可用的时登录控件转换为模板 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


中的 HTML 标记`LayoutTemplate`可能根据需要修改。 同样，随意向模板添加任何新 Web 控件。 但是，很重要，该登录控件的核心 Web 控件保留在该模板，并保留其分配`ID`值。 具体而言，不删除或重命名`UserName`或`Password`文本框中，`RememberMe`复选框，`LoginButton`按钮，`FailureText`标签，或`RequiredFieldValidator`控件。

若要收集访问者的电子邮件地址，我们需要将一个文本框添加到模板。 添加以下声明性标记表行之间 (`<tr>`)，该字符串包含`Password`文本框中和下一步保存记住我的表行时间复选框：

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

在添加后`Email`文本框中，请访问通过浏览器页面。 如图 8 所示，登录控件的用户界面现在包括第三个文本框。


[![登录控件现在包括有关用户的电子邮件地址文本框中](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**图 8**： 登录控件现在包括有关用户的电子邮件地址文本框中 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


此时，Login 控件仍在使用`Membership.ValidateUser`方法以验证提供的凭据。 相应地，在输入值`Email`文本框中没有任何影响在用户可以登录。 在步骤 3 中，我们将介绍如何重写登录控件的身份验证逻辑，以便凭据仅被视为有效如果的用户名和密码有效，并且提供电子邮件地址匹配文件上的电子邮件地址。

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>步骤 3： 修改登录控件的身份验证逻辑

当访问者提供其凭据并单击登录按钮，回发时，才会和登录名来控制其身份验证工作流中的进行处理时。 工作流实例开始通过引发[`LoggingIn`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx)。 与此事件关联的任何事件处理程序可能会取消操作中的日志，通过设置`e.Cancel`属性`true`。

如果在操作中的日志不会被取消，工作流执行过程通过引发[`Authenticate`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)。 如果没有一个事件处理程序`Authenticate`事件，然后它负责确定提供的凭据是否有效。 如果不指定任何事件处理程序，则使用登录控件`Membership.ValidateUser`方法来确定所提供的凭据的有效性。

如果提供的凭据是否有效，则将创建窗体身份验证票证， [ `LoggedIn`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx)引发时，用户被重定向到合适的页面。 如果，但是，凭据被认为是无效，则[`LoginError`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx)引发是显示一条消息，告知用户其凭据已无效。 默认情况下，在失败登录控件只需将设置其`FailureText`标签控件文本属性 （您登录尝试未成功失败消息。 请重试）。 但是，如果登录控件的[`FailureAction`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx)设置为`RedirectToLoginPage`，然后登录控制问题`Response.Redirect`至登录页追加查询字符串参数`loginfailure=1`（这将导致该登录名控件来显示失败消息）。

图 9 提供的身份验证工作流的流程图。


[![登录控件的身份验证工作流](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**图 9**: 登录控件的身份验证工作流 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> 如果你是否想知道当你将使用`FailureAction`的`RedirectToLogin`选项页上，请考虑以下方案。 现在我们`Site.master`母版页当前具有，Hello，stranger 中显示的文本的左边的列时访问由匿名用户，但假设我们想要将该文本替换为登录控件。 这样可以允许匿名用户若要从该站点，而无需直接访问登录页上的任何页登录。 但是，如果用户无法登录通过登录控件由主控页呈现，它可能使意义上，若要将它们重定向到登录页 (`Login.aspx`) 因为该页可能包含其他说明、 链接和其他帮助-例如，若要创建的链接新的帐户或检索的丢失密码的未被添加到母版页。


### <a name="creating-theauthenticateevent-handler"></a>创建`Authenticate`事件处理程序

若要插入到我们的自定义身份验证逻辑中，我们需要创建登录名控件的一个事件处理程序`Authenticate`事件。 创建的事件处理程序`Authenticate`事件将生成以下事件处理程序定义：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

如你所见，`Authenticate`事件处理程序传递的类型对象[ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx)作为其第二个输入参数。 `AuthenticateEventArgs`类包含名为的布尔属性`Authenticated`用于指定提供的凭据是否有效。 我们的任务，然后，是编写代码此处以确定提供的凭据是否有效或不是，并将设置`e.Authenticate`属性相应地。

### <a name="determining-and-validating-the-supplied-credentials"></a>确定和验证提供的凭据

使用登录控件的[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx)和[`Password`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx)来确定由用户输入的用户名和密码凭据。 若要确定在任何其他 Web 控件中输入的值 (如`Email`我们在上一步中添加的文本框中)，使用 *`LoginControlID`*  `.FindControl`(" *`controlID`* ") 来获取以编程方式引用为 Web 控件模板中其`ID`属性等于 *`controlID`* 。 例如，若要获取对引用`Email`文本框中，使用以下代码：

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

若要验证用户的凭据，我们需要做两件事：

1. 确保提供的用户名和密码有效
2. 确保输入的电子邮件地址与试图进行登录的用户的文件上的电子邮件地址

若要完成第一项检查我们就可以使用`Membership.ValidateUser`像我们在步骤 1 中看到的方法。 对于第二项检查，我们需要确定用户的电子邮件地址，以便我们可以将其向文本框控件输入的电子邮件地址进行比较。 若要获取有关特定用户的信息，请使用`Membership`类的[`GetUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)。

`GetUser`方法具有大量的重载。 如果使用未传入任何参数，它将返回有关当前已登录用户的信息。 若要获取有关特定用户的信息，请调用`GetUser`在其用户名中传递。 两种方式的`GetUser`返回[`MembershipUser`对象](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)，它具有属性，例如`UserName`， `Email`， `IsApproved`， `IsOnline`，依次类推。

下面的代码实现这些两个检查。 如果同时通过，然后`e.Authenticate`设置为`true`，否则它将分配`false`。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

使用此代码中的位置，尝试为输入正确的用户名、 密码和电子邮件地址的有效用户登录。 重试，但这次有意使用不正确的电子邮件地址 （请参阅图 10）。 最后，它尝试使用不存在用户名第三次。 在第一种情况下你应已成功登录到站点，但在最后两个情况下，你应看到登录控件的凭据无效消息。


[![提供不正确的电子邮件地址时，Tito 无法登录。](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**图 10**: Tito 无法日志中时提供不正确的电子邮件地址 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> 步骤 1 中的如何成员资格 Framework 处理无效登录尝试次数部分中所述时`Membership.ValidateUser`方法调用该方法并传递凭据无效，它跟踪的无效登录尝试以及某一特定超出时阻止用户指定的时间窗口内的无效尝试次数的阈值。 因为我们自定义身份验证逻辑调用`ValidateUser`方法，有效用户名不正确的密码将递增无效的登录尝试计数器，但此计数器不会增加在这种情况，其中的用户名和密码是否有效，但电子邮件地址不正确。 很有，此行为很合适，因为它不太黑客将知道的用户名和密码，但需要使用暴力破解技术来确定用户的电子邮件地址。


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>步骤 4： 提高登录控件的凭据无效消息

当用户尝试使用无效的凭据登录时，登录名，将显示一条消息说明的登录尝试不成功。 具体而言，该控件将显示由指定的消息其[`FailureText`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx)，具有默认值为您的登录尝试未成功。 请重试。

回想一下，原因有很多原因用户的凭据可能无效：

- 用户名可能不存在
- 用户名已存在，但密码为无效
- 用户名和密码是虽然有效，但尚未批准的用户
- 用户名和密码是否有效，但是用户已被锁定扩展 （最有可能是由于它们超出指定的时间范围内的无效的登录尝试次数）

和可能存在其他原因时使用自定义身份验证逻辑。 例如，替换为代码，我们编写了在步骤 3 中，用户名和密码可能有效，但可能不正确的电子邮件地址。

无论原因的凭据无效，Login 控件显示相同的错误消息。 这种反馈缺乏可以让其帐户尚未批准或人员已锁定的用户感到困惑。通过工作很少，不过，我们可以具有的登录控制显示更适当的消息。

每当用户尝试登录与登录控件引发的凭据无效其`LoginError`事件。 继续并创建事件处理程序对于此事件，并添加以下代码：

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

上面的代码首先设置登录控件的`FailureText`属性设置为默认值 （您的登录尝试未成功。 请重试）。 它然后检查是否提供的用户名将映射到现有的用户帐户。 如果是，向生成`MembershipUser`对象的`IsLockedOut`和`IsApproved`属性以确定是否帐户已锁定，或者尚未批准。 在任一情况下，`FailureText`属性更新为相应的值。

若要测试此代码，有意尝试登录作为现有用户，但使用了错误的密码。 在 10 分钟时间范围内的行中执行操作这五次，该帐户将被锁定。图 11 显示，后续的登录尝试将始终会失败 （甚至使用正确的密码），但现在将显示更具描述性你的帐户已锁定由于无效的登录尝试输入太多次。 请与管理员联系，让你的帐户解锁的邮件。


[![Tito 执行无效的登录尝试输入太多次，并且已被锁定。](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**图 11**: Tito 执行太多无效登录尝试次数和具有已锁定 ([单击以查看实际尺寸的图像](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>摘要

以前本教程中，我们登录页验证的用户名/密码对硬编码列表提供的凭据。 在本教程中，我们更新页后，可以验证针对成员资格框架的凭据。 在步骤 1 中我们讨论在使用`Membership.ValidateUser`方法以编程方式。 在步骤 2 中我们已更换我们手动创建的用户界面和代码与登录控件。

登录控件呈现标准登录用户界面，并自动验证用户的凭据针对成员资格框架。 此外，在发生时有效的凭据，登录控制用户在登录通过窗体身份验证。 简单地说，完全正常运行的登录名的用户体验是可用只需将登录控件拖动到页面、 没有额外声明性标记或所需代码。 更多的是什么，Login 控件是可高度自定义，允许进行精细大程度上控制呈现的用户界面和身份验证逻辑。

此时我们的网站的访问者可以创建新的用户帐户和日志到站点，但我们尚未查看限制对页基于经过身份验证的用户访问权限。 当前，任何用户，经过身份验证或匿名的可以在我们的站点上查看的任何页面。 以及控制访问我们的站点上的页面以用户的用户为基础，我们可能具有其功能依赖于用户某些页。 下一教程讲述如何限制和根据在已登录用户的页面中功能的访问。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [显示自定义消息到锁定和未经批准的用户](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [检查 ASP.NET 2.0 的成员资格、 角色和配置文件](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [如何： 创建 ASP.NET 登录页](https://msdn.microsoft.com/library/ms178331.aspx)
- [登录控件技术文档](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [使用登录控件](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨和 Michael Olivero。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

>[!div class="step-by-step"]
[上一页](creating-user-accounts-cs.md)
[下一页](user-based-authorization-cs.md)
