---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: "恢复并更改密码 (C#) |Microsoft 文档"
author: rick-anderson
description: "ASP.NET 包括两个 Web 控件，以帮助进行恢复，以及更改密码。 如何启用访问者来恢复其丢失的 pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 76c02a3da7dffad25a7bee03efff6b693f261d85
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="recovering-and-changing-passwords-c"></a>恢复并更改密码 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip)或[下载 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET 包括两个 Web 控件，以帮助进行恢复，以及更改密码。 说明允许访问者来恢复丢失的密码。 ChangePassword 控件允许用户更新其密码。 与其他登录名相关的 Web 控件类似情况下，我们已了解在本系列教程的取回整个并 ChangePassword 控制在后台重置或修改用户的密码的成员资格框架的工作。


## <a name="introduction"></a>介绍

之间我 bank、 实用工具公司、 电话公司、 电子邮件帐户和个性化的 web 门户网站，I，像大多数人而言，具有多个不同要记住的密码。 使用凭据太多，需记住这些天，是很常见的人员忘记了密码。 若要考虑到这一点，提供用户帐户的网站需要包括用户能够恢复其密码的方法。 此过程通常涉及生成一个新的随机密码和相关文件上的用户的电子邮件地址。 接收其新密码后大多数用户返回到该站点，并从指向一个更容易记忆的随机生成一个更改其密码。

ASP.NET 包括两个 Web 控件，以帮助进行恢复，以及更改密码。 说明允许访问者来恢复丢失的密码。 ChangePassword 控件允许用户更新其密码。 与其他登录名相关的 Web 控件类似情况下，我们已了解在本系列教程的取回整个并 ChangePassword 控制在后台重置或修改用户的密码的成员资格框架的工作。

在本教程中我们将检查使用这两个控件。 我们还将了解如何以编程方式更改和重置用户的密码通过`MembershipUser`类的`ChangePassword`和`ResetPassword`方法。

## <a name="step-1-helping-users-recover-lost-passwords"></a>步骤 1： 帮助用户恢复丢失密码

支持用户帐户的所有网站都需要为用户提供一些机制来恢复其忘记了的密码。 好消息是在 ASP.NET 中实现此类功能轻松感谢取回 Web 控件。 说明控件呈现提示用户输入其用户名的接口并根据需要其安全问题的答案。 它然后电子邮件用户其密码。

> [!NOTE]
> 由于通过网络在纯文本中传输电子邮件会带来安全风险涉及发送通过电子邮件的用户的密码。


说明控件包含三个视图：

- **用户名**-提示有关他们的用户名在距访客。 这是初始视图。
- **问题**-显示为文本，以及为用户输入其安全问题答案的文本框中的用户的用户名和安全问题。
- **成功**-显示一条消息，告知用户其密码已电子邮件发送。

视图显示，并由取回控件执行的操作取决于以下的成员身份配置设置：

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

成员资格 framework`RequiresQuestionAndAnswer`设置指示用户是否必须指定一个安全提示问题和答案时注册的帐户。 如我们所述<a id="_msoanchor_1"> </a> [*创建用户帐户*](../membership/creating-user-accounts-cs.md)教程，如果`RequiresQuestionAndAnswer`为 True （默认值），则 CreateUserWizard 接口包括文本框中为新用户的安全问题和答案; 控件如果`RequiresQuestionAndAnswer`为 False 时，会收集任何此类信息。 同样，如果`RequiresQuestionAndAnswer`为 True，则这一问题查看用户输入其用户名后取回控件显示; 仅当在用户输入正确的安全性答案恢复密码。 如果`RequiresQuestionAndAnswer`为 False 时，但是，说明将直接从用户名视图移动到成功视图。

用户已提供其用户名中的或其用户名和安全提示问题答案，如果后`RequiresQuestionAndAnswer`为 True-取回电子邮件发送用户其密码。 如果`EnablePasswordRetrieval`选项设置为 True，则用户通过电子邮件他们当前的密码。 如果设置为 False 和`EnablePasswordReset`设置为 True，则说明生成一个新的随机密码，为用户和电子邮件发送给他们此新密码。 如果这两个`EnablePasswordRetrieval`和`EnablePasswordReset`为 False，则取回控件将引发异常。

> [!NOTE]
> 回想一下，`SqlMembershipProvider`将用户的密码存储在以下三种格式之一： 清除、 Hashed （默认值） 或加密。 使用的存储机制依赖于的成员身份配置设置;演示应用程序使用 Hashed 密码格式。 使用 Hashed 密码格式时`EnablePasswordRetrieval`选项必须设置为 False，因为系统无法确定用户的实际密码从数据库中存储的哈希版本。


图 1 显示了如何取回的接口和行为会影响的成员身份配置。


[![RequiresQuestionAndAnswer、 EnablePasswordRetrieval 和 EnablePasswordReset 影响取回控件的外观和行为](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**图 1**: `RequiresQuestionAndAnswer`， `EnablePasswordRetrieval`，和`EnablePasswordReset`影响取回控件的外观和行为 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> 在<a id="_msoanchor_2"> </a> [*在 SQL Server 中创建成员身份架构*](../membership/creating-the-membership-schema-in-sql-server-cs.md)我们通过设置配置成员资格提供程序的教程`RequiresQuestionAndAnswer`为 True，`EnablePasswordRetrieval`到为 false，和`EnablePasswordReset`为 True。


### <a name="using-the-passwordrecovery-control"></a>使用说明

让我们看一下使用 ASP.NET 页中的说明。 打开`RecoverPassword.aspx`和拖动和删除取回控件从工具箱中拖动到设计器; 设置其`ID`到`RecoverPwd`。 登录名和 CreateUserWizard Web 与控件相似，取回控件的视图呈现丰富的复合界面，其中包括标签、 文本框、 按钮和验证控件。 你可以自定义视图，通过该控件的样式属性或通过将视图转换为模板的外观。 我将此作为练习感读取器。

当用户访问此页她将输入其用户名，然后单击提交按钮。 因为我们已经设置了`RequiresQuestionAndAnswer`属性设置为 True 在我们的成员身份配置设置，取回控制将则显示问题视图。 用户输入其正确的安全性应答并单击提交后，说明将用户的密码更新为一个随机生成的活动，并通过电子邮件发送到文件的电子邮件地址此密码。 上述所有操作均为但却没有我们无需编写一行代码 ！

测试此页之前，没有配置以往往到的最后一项： 我们需要指定中的邮件传递设置`Web.config`。 用于发送电子邮件，说明依赖这些设置。

通过指定邮件传递配置[`<system.net>`元素](https://msdn.microsoft.com/library/6484zdc1.aspx)的[`<mailSettings>`元素](https://msdn.microsoft.com/library/w355a94k.aspx)。 使用[`<smtp>`元素](https://msdn.microsoft.com/library/ms164240.aspx)以指示传递方法和默认发件人地址。 以下标记将配置为使用名为网络 SMTP 服务器的电子邮件设置`smtp.example.com`端口 25 并带有用户名/密码凭据的用户名和密码。

> [!NOTE]
> `<system.net>`是根的子元素`<configuration>`元素和节点的同级`<system.web>`。 因此，不要将放`<system.net>`中的元素`<system.web>`元素; 相反，将其放在相同的级别。


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

除了在网络上使用的 SMTP 服务器，或者可以指定要存放要发送的电子邮件消息的其中一个拾取目录。

一旦你已配置 SMTP 设置，请访问`RecoverPassword.aspx`通过浏览器的页。 首先请尝试输入的用户名的用户存储中不存在。 如图 2 所示，取回控件将显示一条消息指出无法访问的用户信息。 可以通过控件的自定义消息的文本[`UserNameFailureText`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)。


[![如果输入无效的用户名，显示错误消息](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**图 2**： 如果输入无效的用户名，将显示错误消息 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image6.png))


现在输入一个用户名。 在系统中通过电子邮件地址，您可以访问，并且其安全回答你的帐户用户名知道的使用。 输入用户名并单击提交后, 取回控件显示其问题视图。 为使用用户名视图中，如果输入不正确回答取回控件显示错误消息 （请参见图 3）。 使用[`QuestionFailureText`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx)自定义此错误消息。


[![如果用户输入无效的安全提示问题答案，将显示一条错误消息](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**图 3**： 如果用户输入无效的安全提示问题答案，将显示错误消息 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image9.png))


最后，输入的正确安全提示问题答案，然后单击提交。 在后台，说明将生成随机的密码、 将其分配给用户帐户、 发送电子邮件，告知其新密码的用户 （请参见图 4），然后显示成功视图。


[![向用户发送一封电子邮件与 His 新密码](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**图 4**： 的用户发送包含 His 新密码的电子邮件 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>自定义电子邮件

说明控件发送的默认电子邮件是暗色，而是 （请参见图 4）。 默认情况下，消息中指定的帐户从`<smtp>`元素的`from`使用者密码的属性和纯文本正文：

请返回到该站点，然后使用以下信息登录。

用户名：*用户名*

密码：*密码*

此消息都可以通过取回控件的事件处理程序以编程方式自定义[`SendingMail`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)，或以声明方式通过[`MailDefinition`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx)。 让我们了解这两种选项。

`SendingMail`电子邮件发送，并且都可以以编程方式调整电子邮件我们最后一次机会之前，正确激发事件。 当引发此事件时，事件处理程序传递的类型对象[ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx)、 其`Message`属性包含到即将发送的电子邮件的引用。

创建的事件处理程序`SendingMail`事件并添加以下代码，以编程方式添加`webmaster@example.com`到抄送列表。

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

此外可以通过声明性方式配置电子邮件。 取回的`MailDefinition`属性是类型的对象[ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx)。 `MailDefinition`类提供的电子邮件相关的属性，包括主机`From`， `CC`， `Priority`， `Subject`， `IsBodyHtml`， `BodyFileName`，和其他人。 对于初学者，设置[`Subject`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx)为更具描述性比使用默认情况下 （密码），如重置密码...

若要自定义电子邮件消息我们需要创建单独的电子邮件模板文件的正文包含正文的内容。 首先创建一个新文件夹中名为的网站`EmailTemplates`。 接下来，将新的文本文件添加到名为此文件夹`PasswordRecovery.txt`并添加以下内容：

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

请注意，使用占位符`<%UserName%>`和`<%Password%>`。 取回控件自动替换这些两个占位符用户的用户名和电子邮件在发送之前的恢复的密码。

最后，点`MailDefinition`的[`BodyFileName`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)到我们刚创建的电子邮件模板 (`~/EmailTemplates/PasswordRecovery.txt`)。

进行这些更改重新查看后`RecoverPassword.aspx`页上，然后输入用户名和安全答案。 你收到应如下所示图 5 中的一个电子邮件。 请注意， `webmaster@example.com` CC 将和是否已更新的主题和正文。


[![已更新的主题，正文和抄送列表](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**图 5**: 使用者、 正文和抄送已更新列表 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image15.png))


若要发送的 HTML 格式的电子邮件设置[ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) True （默认值为 False） 和更新电子邮件模板，以包括 HTML。

`MailDefinition`属性不是唯一的取回类。 我们将会看到在步骤 2 中，还提供了 ChangePassword 控件`MailDefinition`属性。 此外，通过包括这样的属性，你可以配置自动向新用户发送欢迎电子邮件消息。

> [!NOTE]
> 当前没有链接在左侧导航窗格中因达到`RecoverPassword.aspx`页。 用户仅将兴趣如果她已成功登录到该站点无法访问此页。 因此，更新`Login.aspx`页后，可以包括指向的`RecoverPassword.aspx`页。


### <a name="programmatically-resetting-a-users-password"></a>以编程方式重置用户的密码

当重置用户的密码取回控制调用`MembershipUser`对象的[`ResetPassword`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)。 此方法有两个重载：

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)**-重置用户的密码。 如果使用此重载`RequiresQuestionAndAnswer`为 False。
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)**-重置用户的密码才提供*securityAnswer*正确无误。 如果使用此重载`RequiresQuestionAndAnswer`为 True。

这两个重载均返回的新的随机生成的密码。

与成员资格 framework 中的其他方法类似`ResetPassword`方法委托给配置的提供程序。 `SqlMembershipProvider`时，将调用`aspnet_Membership_ResetPassword`存储过程中，通过在用户的用户名、 新密码和提供的密码答案，其他字段中。 存储的过程可确保密码提示问题答案匹配，然后更新用户的密码。

几个低级别的实现说明：

- 锁定的用户无法重置其密码。 但是，可能未批准的用户。 我们将讨论锁定出并批准状态中的更详细地<a id="_msoanchor_3"> </a> [ *Unlocking 和批准用户*](unlocking-and-approving-user-accounts-cs.md)帐户教程。
- 如果密码提示问题答案不正确，用户的失败的密码答案尝试计数将会递增。 如果在指定的时间窗口内出现指定的数量的无效的安全提示问题答案尝试，用户被锁定。

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>生成的单词如何随机的密码

在图 4 和 5 中的电子邮件消息中所示的随机生成密码创建的成员资格类[`GeneratePassword`方法](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)。 此方法接受两个整数输入的参数-*长度*和*numberOfNonAlphanumericCharacters* -并至少返回一个字符串*长度*长时间与在字符至少*numberOfNonAlphanumericCharacters*非字母数字字符数。 在此方法从成员资格类或登录名相关的 Web 控件中调用的取决于这两个参数的值的成员身份配置`MinRequiredPasswordLength`和`MinRequiredNonalphanumericCharacters`属性，我们将设置为 7 和 1，分别。

`GeneratePassword`方法使用的加密型强随机数生成器来确保中选择哪些随机字符没有偏差。 此外，`GeneratePassword`是`public`，这意味着，你可以使用它直接从 ASP.NET 应用程序如果需要生成随机字符串或密码。

> [!NOTE]
> `SqlMembershipProvider`类始终生成随机的密码长度至少 14 个字符，因此，如果`MinRequiredPasswordLength`小于 14 则其值将被忽略。


## <a name="step-2-changing-passwords"></a>步骤 2： 更改密码

随机生成密码很难记住。 图 4 中所示的密码，请考虑： `WWGUZv(f2yM:Bd`。 请尝试提交的内存到 ！ 当然，用户发送此类的随机生成密码后，她将想要将密码更改为更容易记忆。

使用 ChangePassword 控件创建一个接口，使用户能够更改其密码。 得多取回控件，如的 ChangePassword 控件包含两个视图： 更改密码和成功。 更改密码视图提示用户输入其旧密码和新密码。 在提供正确的旧密码和满足最小长度和非字母数字字符要求的新密码，ChangePassword 控件更新用户的密码，并显示成功视图。

> [!NOTE]
> ChangePassword 控件通过调用修改用户的密码`MembershipUser`对象的[`ChangePassword`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)。 ChangePassword 方法接受两个`string`输入参数- *oldPassword*和*newPassword*-和更新用户的帐户与*newPassword*，假定提供*oldPassword*正确无误。


打开`ChangePassword.aspx`页上，并将 ChangePassword 控件添加到页上，其命名为`ChangePwd`。 此时，设计视图应显示更改密码查看 （请参阅图 6）。 如与取回控件中，你可以在视图之间切换通过控件的智能标记。 此外，这些视图的外观是通过各种的样式属性或通过将它们转换为模板，可自定义的。


[![ChangePassword 控件添加到页面](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**图 6**: ChangePassword 控件添加到页面 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image18.png))


ChangePassword 控件可以更新当前登录的用户的密码*或*另一个，指定用户的密码。 如图 6 所示，默认更改密码视图呈现只需三个文本框控件： 一个用于旧密码，两个用于新密码。 此默认接口用于更新当前登录的用户的密码。

若要使用 ChangePassword 控件更新其他用户的密码，设置控件的[`DisplayUserName`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx)为 True。 这样将第四个文本框中添加到页上，若要更改其密码提示输入用户的用户名。

设置`DisplayUserName`到很有用，如果你想要让已注销用户更改其密码而无需登录，则为 True。 个人，我认为没有任何问题在允许她可以更改其密码前需登录用户。 因此，请让`DisplayUserName`设置为 False （其默认值）。 在决策时，但是，我们实质上禁止访问此页的匿名用户。 更新以便拒绝匿名用户访问的站点的 URL 授权规则`ChangePassword.aspx`。 如果您需要刷新你的 URL 授权规则语法上的内存，回头参考<a id="_msoanchor_4"> </a> [*基于用户的授权*](../membership/user-based-authorization-cs.md)教程。

> [!NOTE]
> 它可能看起来，`DisplayUserName`属性可用于允许管理员更改其他用户的密码。 但是，即使`DisplayUserName`设置为 True 必须已知正确的旧密码，然后将其输入。 我们将讨论有关技术允许管理员就可以更改在步骤 3 中的用户的密码。


请访问`ChangePassword.aspx`通过浏览器页并更改你的密码。 请注意是否输入不能满足的密码长度和非字母数字字符要求在成员资格配置中指定的新密码，显示一条错误消息 （请参阅图 7）。


[![ChangePassword 控件添加到页面](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**图 7**: ChangePassword 控件添加到页面 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image21.png))


在用户的输入正确的旧密码和有效新密码，登录时更改密码和成功视图显示。

### <a name="sending-a-confirmation-email"></a>发送确认电子邮件

默认情况下，ChangePassword 控件不向刚刚更新了其密码的用户发送电子邮件。 如果你想要发送电子邮件，只需配置该控件的`MailDefinition`属性。 让我们配置 ChangePassword 控件，以便向用户发送包含其新密码的 HTML 格式电子邮件。

首先创建中的新文件`EmailTemplates`文件夹名为`ChangePassword.htm`。 添加以下标记：

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

接下来，设置密码更改控件的`MailDefinition`属性的`BodyFileName`， `IsBodyHtml`，和`Subject`属性设置为 ~ / EmailTemplates/ChangePassword.htm、 True 和你的密码已更改 ！ 分别。

进行这些更改之后, 重新访问该页并再次更改密码。 此时，ChangePassword 控件将发送到用户的电子邮件地址中文件的自定义的 HTML 格式的电子邮件 （请参阅图 8）。


[![电子邮件条消息，通知用户，他们的密码已更改](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**图 8**: 电子邮件条消息，通知用户，他们的密码已更改 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>步骤 3： 允许管理员更改用户的密码

中支持的用户帐户的应用程序的常见功能是为管理用户，以更改其他用户的密码的能力。 有时此功能是必需的因为系统不具备用户更改其自己的密码的功能。 在这种情况下，用户能够恢复其忘记了的密码的唯一方式将让管理员能够将它们分配新密码。 与说明和更改密码控件中，但是，管理用户需要没有忙于本身与更改用户的密码，因为用户能够执行此本身。

但是，如果你的客户端坚持管理用户应该能够更改其他用户的密码？ 遗憾的是工作的，添加此功能可以是工作的不少。 若要更改用户的密码，旧的和新密码必须提供给`MembershipUser`对象的`ChangePassword`方法，但管理员不应具有了解用户的密码，才能对其进行修改。

一个解决方法是先重置用户的密码，然后将其更改为使用代码如下所示的新密码：

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

此代码启动通过检索有关的信息*用户名*，这是管理员想要更改其密码的用户。 接下来，`ResetPassword`调用方法时，哪些分配和新的随机密码的用户。 方法返回此随机生成密码并将其存储在变量`resetPwd`。 现在，我们知道用户的密码，我们可以将其更改通过调用`ChangePassword`。

问题是，此代码仅适用如果成员资格系统配置设置以便`RequiresQuestionAndAnswer`为 False。 如果`RequiresQuestionAndAnswer`为 True 时，这一点与我们的应用程序，则`ResetPassword`方法需要传递的安全提示问题答案，否则它会引发异常。

如果成员资格 framework 配置为需要安全问题和答案，并且你的客户端尚未坚持管理员将能够更改用户的密码，你有三个选项：

- 在无线引发你有权，告诉你的客户端，这是不能完成，只是一件事情。
- 设置`RequiresQuestionAndAnswer`为 False。 这会导致安全级别较低的应用程序。 假设恶意用户获得了访问另一个用户的电子邮件收件箱。 可能是关于泄露的用户已离开办公桌转到午餐，并且未锁定其工作站，或可能是从公共终端访问其电子邮件和未注销。在任一情况下，恶意用户可以访问`RecoverPassword.aspx`页上，输入用户的用户名。 系统然后电子邮件而不提示的安全提示问题答案已恢复的密码。
- 绕过创建成员身份 framework 和直接与 SQL Server 数据库的工作的抽象层。 成员资格架构包括名为的存储的过程`aspnet_Membership_SetPassword`，设置用户的密码并且不会不需要安全答案或旧密码才能完成其任务。

这些选项都特别有吸引力，但这是开发人员的有时出现的方式。

我提前了，实现第三种方法，编写代码时绕过`Membership`和`MembershipUser`类和操作直接针对`SecurityTutorials`数据库。

> [!NOTE]
> 通过直接与数据库工作，成员身份 framework 提供的封装已被破坏。 此决定会绑定到我们`SqlMembershipProvider`，让我们小于可移植代码。 此外，此代码可能不会按预期在将来版本的 ASP.NET，如果成员资格架构更改工作。 这种方法是一种解决方法，并如大多数解决方法，不是最佳实践的示例。


代码具有一些毫无吸引力 bits，并且为非常长。 因此，我不想打乱它深入探讨与本教程。 如果你有兴趣了解详细信息，下载的代码对于此教程和，请访问`~/Administration/ManageUsers.aspx`页。 此页上，我们在中创建<a id="_msoanchor_5"> </a>[前面教程](building-an-interface-to-select-one-user-account-from-many-cs.md)，列出每个用户。 我已更新以包括指向的 GridView`UserInformation.aspx`页上，将传递通过查询字符串的所选的用户的用户名。 `UserInformation.aspx`页显示有关选定的用户和文本框中的信息用于更改其密码 （请参阅图 9）。

输入新密码，确认它在第二个文本框中，然后单击更新用户按钮后, 回发时，才会与`aspnet_Membership_SetPassword`调用存储的过程时，更新用户的密码。 我鼓励这些读取器兴趣此功能将成为更熟悉代码，并尝试扩展的功能，包括将一封电子邮件发送到用户更改其密码。


[![管理员可更改用户的密码](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**图 9**： 管理员可以更改用户的密码 ([单击以查看实际尺寸的图像](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx`网页当前仅，如果成员资格 framework 配置为将密码存储在清除或 Hashed 格式。 尽管你受邀添加此功能，它缺少的代码的新密码进行加密。 建议添加必要的代码的方法是使用如反编译程序[反射器](http://www.aisto.com/roeder/dotnet/)来检查.NET Framework 中; 中的方法的源代码通过检查启动`SqlMembershipProvider`类的`ChangePassword`方法。 这是密码的我用于编写用于创建哈希代码的技术。


## <a name="summary"></a>摘要

ASP.NET 提供两个控件，可以帮助用户管理他们的密码。 说明控件是有用的用户的用户忘记其密码。 根据成员资格框架的配置，用户可以通过电子邮件用户现有的密码或新为随机生成的密码。 ChangePassword 控件使用户可以更新其密码。

登录名和 CreateUserWizard 与控件相似，说明和 ChangePassword 控件呈现丰富的用户界面，而无需编写缺乏对声明性标记或代码行。 如果默认用户界面不满足你的需求，你可以自定义它通过各种样式属性。 或者的控件的接口可能被转换为模板，甚至更精细的控制度。 这些控件使用成员资格 API 中，在后台调用`MembershipUser`对象的`ResetPassword`和`ChangePassword`方法。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ChangePassword 控件快速入门](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [取回控件快速入门](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [在 ASP.NET 中发送电子邮件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`常见问题](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者包括 Michael Emmings 和 Suchi Banerjee。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](building-an-interface-to-select-one-user-account-from-many-cs.md)
[下一页](unlocking-and-approving-user-accounts-cs.md)
