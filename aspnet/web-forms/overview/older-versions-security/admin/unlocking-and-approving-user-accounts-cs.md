---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: "解锁和批准用户帐户 (C#) |Microsoft 文档"
author: rick-anderson
description: "本教程演示如何生成某一网页寻求管理员可以管理用户的锁定和批准状态。 我们将了解如何批准新的用户 o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: f22a745f42dae66cd64dc38df28c59b910c17070
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="unlocking-and-approving-user-accounts-c"></a>解锁和批准用户帐户 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip)或[下载 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> 本教程演示如何生成某一网页寻求管理员可以管理用户的锁定和批准状态。 我们将了解如何以批准新的用户，仅当它们具有验证其电子邮件地址之后。


## <a name="introduction"></a>介绍

除了用户名、 密码和电子邮件，每个用户帐户具有两个规定用户可以登录到站点的状态字段： 锁定和批准。 用户自动锁定如果它们提供无效凭据指定的大量的时间内指定的 （默认设置后将其锁定用户无效的登录尝试 5 次在 10 分钟内） 的分钟数。 已批准的状态是其中一些操作必须经过多新用户能够登录到该站点之前的方案中十分有用。 例如，用户可能需要首先验证其电子邮件地址或由管理员批准便可登录。

因为锁定或未批准用户无法登录，它是唯一的自然想知道如何重置这些状态。 ASP.NET 不包括任何内置功能或 Web 控件，用于管理用户的锁定和部分批准状态，因为这些决策需要根据站点的站点进行处理。 某些站点可能会自动批准所有的新用户帐户 （默认行为）。 其他人有一位管理员批准新的帐户或不批准用户直到他们访问发送到它们注册时提供的电子邮件地址的链接。 同样，某些站点可能锁定用户，直到管理员重置其状态，而其他站点发送一封电子邮件到使用 URL 锁定状态的用户他们可以访问以解锁其帐户。

本教程演示如何生成某一网页寻求管理员可以管理用户的锁定和批准状态。 我们将了解如何以批准新的用户，仅当它们具有验证其电子邮件地址之后。

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>步骤 1： 管理用户的锁定和批准状态

在<a id="Tutorial12"> </a> [*从许多到选择一个用户帐户生成接口*](building-an-interface-to-select-one-user-account-from-many-cs.md)教程我们构造列出在分页，每个用户帐户的页面筛选 GridView。 网格中列出每个用户的名称和电子邮件、 及其已审批和锁定状态，无论它们是处于联机状态，和有关用户的任何备注。 若要管理用户的批准，锁定状态，我们无法使此网格可编辑。 若要更改用户的批准的状态，管理员将首先找到的用户帐户，并选中或取消选中已批准复选框，然后编辑相应的 GridView 行。 或者，我们可以管理通过单独的 ASP.NET 页的已批准和锁定状态。

对于本教程，让我们使用两个 ASP.NET 页：`ManageUsers.aspx`和`UserInformation.aspx`。 这里的思路是`ManageUsers.aspx`在系统中，列出的用户帐户时`UserInformation.aspx`使管理员能够管理的特定用户的已批准和锁定状态。 我们第一个的顺序的工作是增加在 GridView`ManageUsers.aspx`包括 HyperLinkField，将呈现为一列链接。 我们希望每个链接以指向`UserInformation.aspx?user=UserName`，其中*用户名*是要编辑的用户的名称。

> [!NOTE]
> 如果你下载的代码<a id="Tutorial13"> </a> [ *Recovering 和更改密码*](recovering-and-changing-passwords-cs.md)教程，你可能已经注意到，`ManageUsers.aspx`页已包含的一组"管理"的链接和`UserInformation.aspx`页会提供一个接口更改选定的用户的密码。 我决定不复制该功能在代码中与本教程，因为它通过避开成员资格 API 和操作直接与要更改用户的密码的 SQL Server 数据库的工作。 本教程开始使用从头`UserInformation.aspx`页。


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>添加"管理"指向`UserAccounts`GridView

打开`ManageUsers.aspx`页上，添加到 HyperLinkField `UserAccounts` GridView。 设置 HyperLinkField `Text` "管理"的属性并将其`DataNavigateUrlFields`和`DataNavigateUrlFormatString`属性设置为`UserName`和"UserInformation.aspx?user={0}"，分别。 这些设置配置 HyperLinkField，以便所有超链接显示文本"Manage"，但每个链接将在相应传递*用户名*值转换为查询字符串。

在添加后 HyperLinkField 到 GridView，花些时间查看`ManageUsers.aspx`通过浏览器的页。 如图 1 所示，每个 GridView 行现在包括"管理"链接。 罗斯向她的"管理"链接指向`UserInformation.aspx?user=Bruce`，而 Dave 的"管理"链接指向`UserInformation.aspx?user=Dave`。


[![HyperLinkField 添加](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**图 1**: HyperLinkField 将为每个用户帐户添加的"管理"链接 ([单击以查看实际尺寸的图像](unlocking-and-approving-user-accounts-cs/_static/image3.png))


我们将创建用户界面并代码`UserInformation.aspx`页中时刻，但首先让我们谈一有关如何以编程方式更改用户的锁定并批准状态。 [ `MembershipUser`类](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)具有[ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)和[`IsApproved`属性](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx)。 `IsLockedOut`属性是只读的。 没有任何机制来以编程方式锁定用户;若要解除用户，使用`MembershipUser`类的[`UnlockUser`方法](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)。 `IsApproved`属性是可读写。 若要将任何更改保存到此属性，我们需要调用`Membership`类的[`UpdateUser`方法](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)，并传入已修改`MembershipUser`对象。

因为`IsApproved`属性为读 / 写，复选框控件可能是最佳的用户界面元素，用于配置此属性。 但是，一个复选框将不能用于`IsLockedOut`属性因为管理员不能锁定用户，她可能只能解除用户。 用于的合适的用户界面`IsLockedOut`属性是一个按钮，单击时，取消锁定的用户帐户。 如果用户被锁定，则只应启用此按钮。

### <a name="creating-theuserinformationaspxpage"></a>创建`UserInformation.aspx`页

我们现在已准备好实现中的用户界面`UserInformation.aspx`。 打开此页并添加以下 Web 控件：

- 为超链接控件，单击时，返回到管理员`ManageUsers.aspx`页。
- 标签 Web 控件用于显示所选的用户的名称。 设置此标签的`ID`到`UserNameLabel`和清除其`Text`属性。
- 一个名为的复选框控件`IsApproved`。 设置其`AutoPostBack`属性`true`。
- 用于显示用户的标签控件的上次锁定日期。 命名此标签`LastLockedOutDateLabel`和清除其`Text`属性。
- 用于解锁用户按钮。 此按钮命名`UnlockUserButton`并设置其`Text`为"解锁 User"的属性。
- 标签用于显示的控件状态消息，例如"已更新用户的批准的状态"。 命名此控件`StatusMessage`，清除出其`Text`属性，并设置其`CssClass`属性`Important`。 ( `Important` CSS 类中定义`Styles.css`样式表文件; 它在大型、 红色字体显示对应的文本。)

添加这些控件后, 在 Visual Studio 中的设计视图应类似于的屏幕快照在图 2 中。


[![创建 UserInformation.aspx 的用户界面](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**图 2**： 创建的用户界面`UserInformation.aspx`([单击以查看实际尺寸的图像](unlocking-and-approving-user-accounts-cs/_static/image6.png))


与使用完整的用户界面，我们的下一个任务是设置`IsApproved`复选框和其他控件根据所选的用户的信息。 创建的事件处理程序已在页面的`Load`事件并添加以下代码：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

通过确保这第一次访问的页面并不后续回发启动上面的代码。 随后，将通过传递的用户名`user`查询字符串字段和检索有关通过该用户帐户信息`Membership.GetUser(username)`方法。 如果没有用户名提供通过查询字符串，或如果找不到指定的用户，管理员发送回`ManageUsers.aspx`页。

`MembershipUser`对象的`UserName`值随后显示在`UserNameLabel`和`IsApproved`复选框已选中根据`IsApproved`属性值。

`MembershipUser`对象的[`LastLockoutDate`属性](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx)返回`DateTime`值，该值指示用户上次锁定。如果用户永远不会被锁定，则返回的值将取决于成员资格提供程序。 当创建新帐户时，`SqlMembershipProvider`设置`aspnet_Membership`表的`LastLockoutDate`字段`1754-01-01 12:00:00 AM`。 上面的代码显示在一个空字符串`LastLockoutDateLabel`如果`LastLockoutDate`属性出现年度之前 2000年; 否则为的日期部分`LastLockoutDate`标签中显示属性。 `UnlockUserButton'` S`Enabled`属性设置为用户的锁定状态，这意味着如果用户锁定才会启用此按钮。

花一些时间来测试`UserInformation.aspx`通过浏览器的页。 你将当然，需要开始`ManageUsers.aspx`并选择要管理用户帐户。 在到达时`UserInformation.aspx`，请注意，`IsApproved`仅已选中复选框，如果用户批准。 如果用户曾已被锁定，将显示出日期锁定其上一次。 仅当用户当前被锁定时，才启用解锁用户按钮。选中或取消选中`IsApproved`复选框或单击解锁用户按钮会导致回发，但不修改都会对用户帐户，因为我们尚未为创建这些事件的事件处理程序。

返回到 Visual Studio 并创建事件处理程序`IsApproved`复选框的`CheckedChanged`事件和`UnlockUser`按钮的`Click`事件。 在`CheckedChanged`事件处理程序中，设置用户的`IsApproved`属性`Checked`属性的复选框，然后通过调用所做的更改保存`Membership.UpdateUser`。 在`Click`事件处理程序，只需调用`MembershipUser`对象的`UnlockUser`方法。 在这两个事件处理程序中，显示中的合适消息`StatusMessage`标签。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>测试`UserInformation.aspx`页

与就地这些事件处理程序，重新访问该页和未批准的用户。 如图 3 所示，你应该会看到一条消息，该值指示在页面上用户的简短`IsApproved`已成功修改属性。


[![Chris 已被未批准](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**图 3**: Chris 已被未批准的 ([单击以查看实际尺寸的图像](unlocking-and-approving-user-accounts-cs/_static/image9.png))


接下来，注销，然后重试其帐户以用户身份登录时只需未批准。 用户未获批准，因为他们无法登录。 默认情况下，Login 控件显示同一条消息，如果用户无法登录，无论是什么原因。 但在<a id="Tutorial6"> </a> [*验证用户凭据对成员资格用户存储区*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md)我们看增强登录控件以显示更合适的消息的教程。 如图 4 所示，Chris 显示一条消息说明他不能登录因为他的帐户不尚未获得批准。


[![Chris 无法登录因为 His 帐户未批准](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**图 4**: Chris 无法登录因为 His 帐户是否未批准的 ([单击以查看实际尺寸的图像](unlocking-and-approving-user-accounts-cs/_static/image12.png))


若要测试的锁定状态的功能，尝试登录作为已批准用户，但使用了错误的密码。 重复此过程所需数量的用户的帐户已锁定之前的时间。登录控件也已更新为显示自定义消息如果尝试从锁定的帐户登录。 你知道，帐户已锁定后，你开始看到登录页的以下消息:"你的帐户已锁定由于无效的登录尝试输入太多次。 请与管理员联系以解锁帐户。"

返回到`ManageUsers.aspx`页上，单击锁定状态的用户的管理链接。 如图 5 所示，你应看到在值`LastLockedOutDateLabel`应启用解锁用户按钮。 单击解锁用户按钮以解除对用户帐户。 一旦已解锁用户，他们将能够重新登录。


[![Dave 已从系统被锁定](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**图 5**: Dave 具有已锁定外出时的系统 ([单击以查看实际尺寸的图像](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>步骤 2： 指定新用户的批准状态

已批准的状态是想新用户能够登录并访问该站点的特定于用户的功能之前要执行某些操作的方案中十分有用。 例如，您可能运行的所有页面，除登录名和注册页中，都是仅向经过身份验证的用户可访问的专用网站。 但是，如果陌生人达到你的网站，会发生什么情况找到注册页上，并创建一个帐户？ 若要防止发生此无法将移到登录页`Administration`文件夹，并且要求管理员手动创建的每个帐户。 或者，你可以允许任何人在注册，但禁止站点访问，直到管理员批准的用户帐户。

默认情况下，通过批准新的帐户。 你可以配置使用控件的此行为[`DisableCreatedUser`属性](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)。 将此属性设置为`true`不批准新的用户帐户。

> [!NOTE]
> 默认情况下通过自动登录的新用户帐户。 此行为由该控件的[`LoginCreatedUser`属性](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)。 因为未批准的用户不能登录到站点，当`DisableCreatedUser`是`true`新的用户帐户尚未登录到的站点，无论的值如何`LoginCreatedUser`属性。


如果要以编程方式创建新的用户帐户通过`Membership.CreateUser`方法，创建一个未批准的用户帐户将接受新用户的重载之一`IsApproved`作为输入参数的属性值。

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>步骤 3： 批准用户通过验证其电子邮件地址

支持用户帐户的很多网站不批准新的用户，直到它们验证时注册它们提供的电子邮件地址。 此验证过程通常用于防止机器人、 垃圾邮件发送者和其他家伙，因为它需要唯一的、 经过验证的电子邮件地址，在注册过程中添加一个额外步骤。 与此模型中，当新用户注册就会发送电子邮件消息，其中包含指向验证页的链接。 通过访问以下链接用户已证实它们收到该电子邮件，因此的电子邮件地址提供是有效。 验证页负责批准用户。 这可能会自动发生，从而批准到达此页上，任何用户或用户提供一些额外信息，如后才[CAPTCHA](http://en.wikipedia.org/wiki/Captcha)。

若要适应此工作流，我们需要先更新帐户创建页面上，以便新用户均未批准。 打开`EnhancedCreateUserWizard.aspx`页面`Membership`文件夹并设置通过`DisableCreatedUser`属性`true`。

接下来，我们需要配置通过将一封电子邮件发送到新用户包含有关如何验证他们的帐户的说明。 具体而言，我们将发送到电子邮件中包含一个链接`Verification.aspx`页 (我们尚未为其创建)，并在新用户的传入`UserId`通过查询字符串。 `Verification.aspx`页将查找指定的用户并将其批准标记。

### <a name="sending-a-verification-email-to-new-users"></a>将验证电子邮件发送到新用户

若要从通过发送一封电子邮件，配置其`MailDefinition`属性正确。 中所述<a id="Tutorial13"> </a>[以前一教程](recovering-and-changing-passwords-cs.md)，ChangePassword 和取回控件包括[`MailDefinition`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)相同方式与的方式通过的。

> [!NOTE]
> 若要使用`MailDefinition`需要指定邮件传递的属性中的选项`Web.config`。 有关详细信息，请参阅[在 ASP.NET 中发送电子邮件](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)。


首先创建一个名为的新电子邮件模板`CreateUserWizard.txt`中`EmailTemplates`文件夹。 用于此模板的以下文本：

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

设置`MailDefinition'`s`BodyFileName`属性设置为"~ / EmailTemplates/CreateUserWizard.txt"并将其`Subject`属性设置为"欢迎使用我的网站 ！ 请激活你的帐户。"

请注意，`CreateUserWizard.txt`电子邮件模板包括`<%VerificationUrl%>`占位符。 这就是 URL`Verification.aspx`将放置页。 CreateUserWizard 自动替换`<%UserName%>`和`<%Password%>`占位符替换为新帐户的用户名和密码，但没有任何内置`<%VerificationUrl%>`占位符。 我们需要手动将其替换为相应的验证 URL。

若要实现此目的，创建事件处理程序为 CreateUserWizard [ `SendingMail`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx)并添加以下代码：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail`后，事件将激发`CreatedUser`事件，这意味着，上面的事件处理程序执行新用户时帐户已经创建。 我们就可以访问新用户的`UserId`值通过调用`Membership.GetUser`方法，同时传入`UserName`输入到通过。 接下来，验证 URL 是格式正确。 语句`Request.Url.GetLeftPart(UriPartial.Authority)`返回`http://yourserver.com`URL; 部分`Request.ApplicationPath`返回应用程序的根节点位置的路径。 URL 然后定义为验证`Verification.aspx?ID=userId`。 这两个字符串然后串联在一起以形成完整的 URL。 最后，将电子邮件正文 (`e.Message.Body`) 具有的所有匹配项`<%VerificationUrl%>`替换的完整 URL。

净效果是新用户均未批准的这意味着它们不能登录到网站。 此外，它们向自动发送包含链接的电子邮件的验证 URL （请参阅图 6）。


[![新用户会收到一封电子邮件的验证 URL 的链接](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**图 6**： 新的用户会收到一封电子邮件的验证 URL 的链接 ([单击以查看实际尺寸的图像](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> 通过默认 CreateUserWizard 步骤才会显示消息，告知的用户他们的帐户已创建，并且显示继续按钮。 单击该按钮可以使用户转到由该控件的指定的 URL`ContinueDestinationPageUrl`属性。 在 CreateUserWizard`EnhancedCreateUserWizard.aspx`已配置为发送到的新用户`~/Membership/AdditionalUserInfo.aspx`，随后系统会提示用户输入其家乡、 主页 URL 和签名。 因为此信息可以由添加登录的用户，所以应该以更新该属性将用户发送回站点的主页 (`~/Default.aspx`)。 此外，`EnhancedCreateUserWizard.aspx`应扩充页或 CreateUserWizard 步骤以通知用户他们已发送验证电子邮件，并不会激活其帐户，直到它们按照此电子邮件中的说明。 我将保留这些修改为一个练习为读取器。


### <a name="creating-the-verification-page"></a>创建验证页

我们最后一项任务是创建`Verification.aspx`页。 将此页添加到根文件夹，将其与相关联`Site.master`母版页。 我们已完成大部分前面的内容页添加到该站点，如删除引用的内容控件`LoginContent`ContentPlaceHolder 以便内容页使用母版页的默认内容。

添加标签 Web 控件与`Verification.aspx`页上，设置其`ID`到`StatusMessage`和清除其文本属性。 接下来，创建`Page_Load`事件处理程序并添加以下代码：

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

上面的代码的大部分验证`UserId`通过提供查询字符串存在，它是一个有效`Guid`值，并且它引用现有的用户帐户。 如果通过了所有这些检查，获得批准的用户帐户;否则，会显示一条合适的状态消息。

图 7 显示了`Verification.aspx`页上通过浏览器访问时。


[![新用户的帐户是现在批准](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**图 7**： 的新用户的帐户是现在批准 ([单击以查看实际尺寸的图像](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>摘要

所有成员资格用户帐户具有确定用户可以登录到网站的两个状态：`IsLockedOut`和`IsApproved`。 这两种属性必须是`true`该用户的登录名。

用户的锁定状态用作一种安全措施以减少黑客通过暴力破解方法到站点中断的可能性。 具体而言，锁定用户是否存在一定数量的特定时间段的时间内的无效登录尝试。 这些边界是通过中的成员资格提供程序设置可配置`Web.config`。

已批准的状态通常作为一种方法用于禁止新用户登录，直到某些操作已发生的情况。 站点可能需要先由管理员批准新的帐户或，正如我们看到在步骤 3 中，通过验证其电子邮件地址。

尽情享受编程 ！

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢...

本教程系列已由许多有用的审阅者评审。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](recovering-and-changing-passwords-cs.md)
[下一页](building-an-interface-to-select-one-user-account-from-many-vb.md)
