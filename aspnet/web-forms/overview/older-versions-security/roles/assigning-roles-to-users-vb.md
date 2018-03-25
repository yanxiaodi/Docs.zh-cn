---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: "将角色分配给用户 (VB) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将生成两个 ASP.NET 页，以帮助管理用户属于哪些角色。 第一页将包括设施以查看..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: c790f5f9b486b6598955459827c07ec9ad33ae38
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="assigning-roles-to-users-vb"></a>将角色分配给用户 (VB)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip)或[下载 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> 在本教程中我们将生成两个 ASP.NET 页，以帮助管理用户属于哪些角色。 第一页将包括设施以查看哪些用户属于一个给定的角色，哪些特定用户所属的角色和分配或从特定角色中删除特定用户的能力。 在第二页我们将增加通过，以便其包括指定新创建的用户属于哪些角色的步骤。 这是管理员是能够创建新用户帐户的方案中十分有用。


## <a name="introduction"></a>介绍

<a id="_msoanchor_1"> </a>[以前一教程](creating-and-managing-roles-vb.md)检查角色 framework 和`SqlRoleProvider`; 我们已了解如何使用`Roles`类来创建、 检索和删除角色。 除了创建和删除角色，我们需要能够分配或从角色中删除用户。 遗憾的是，ASP.NET 不随附于用于管理哪些用户属于哪些角色的任何 Web 控件。 相反，我们必须创建我们自己的 ASP.NET 页来管理这些关联。 好消息是该添加和删除角色的用户操作会相当简单。 `Roles`类包含多种方法来将一个或多个用户添加到一个或多个角色。

在本教程中我们将生成两个 ASP.NET 页，以帮助管理用户属于哪些角色。 第一页将包括设施以查看哪些用户属于一个给定的角色，哪些特定用户所属的角色和分配或从特定角色中删除特定用户的能力。 在第二页我们将增加通过，以便其包括指定新创建的用户属于哪些角色的步骤。 这是管理员是能够创建新用户帐户的方案中十分有用。

让我们进入正题！

## <a name="listing-what-users-belong-to-what-roles"></a>列出用户属于哪些角色

对于本教程第一个的顺序的业务就是创建 web 页，从中可以将用户分配给角色。 我们考虑自己如何将用户分配到角色之前，让我们首先专注于如何确定哪些用户属于哪些角色。 有两种方法要显示此信息:"角色"或"被用户。" 我们无法允许访问者可以选择一个角色，然后显示它们的所有用户属于角色 （"按角色"显示中），或者我们无法提示访问者选择的用户，然后显示其分配到该用户 （"由用户"显示屏） 的角色。

"按角色"视图都适用于情况在距访客希望知道属于特定角色，则用户的一组当访问者需要知道特定的用户角色时，"由用户"视图不理想。 让我们来使用我们的页面包括"的角色"和"按用户"接口。

我们将从开始创建"由用户"接口。 此接口将下拉列表和复选框的列表组成。 下拉列表将填入系统; 中的用户组复选框将枚举这些角色。 从下拉列表中选择用户将检查用户属于这些角色。 访问的页面的人员可以然后选中或取消选中复选框以添加或从相应的角色中删除选定的用户。

> [!NOTE]
> 使用下拉列表添加到列表的用户帐户不是用于网站的理想选择其中可能有数百个用户帐户。 下拉列表旨在允许用户选取从相对较短列表的选项的一个项。 将变得难以操作随着列表项的数目的增长。 如果您正在生成将具有可能很大数量的用户帐户的网站，你可能想要考虑使用一个替代用户界面，如可分页 GridView 或列出的可筛选界面将提示在距访客选择字母，然后仅显示其用户名以所选的字母开头这些用户。


## <a name="step-1-building-the-by-user-user-interface"></a>步骤 1： 生成"由用户"用户界面

打开`UsersAndRoles.aspx`页。 在页面顶部，添加一个名为的标签 Web 控件`ActionStatus`和清除其`Text`属性。 我们将使用此标签来提供反馈，所执行的操作上显示的消息，"用户 Tito 已添加到管理员角色中，"或者"用户 Jisun 已从主管角色。" 若要使这些邮件突出设置标签的`CssClass`属性设置为"Important"。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

接下来，添加以下 CSS 类定义添加到`Styles.css`样式表：

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

此 CSS 定义会指示浏览器显示使用大型、 红色字体的标签。 图 1 显示了这种效果通过 Visual Studio 设计器。


[![标签的 CssClass 属性会导致大型、 红色字体](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**图 1**: 标签`CssClass`属性导致大型，红色字体 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image3.png))


接下来，将 DropDownList 添加到页上，设置其`ID`属性`UserList`，并设置其`AutoPostBack`属性为 True。 我们将使用此 DropDownList 系统中列出的所有用户。 此 DropDownList 将绑定到的 MembershipUser 对象的集合。 因为我们希望下拉列表来显示 MembershipUser 对象的用户名属性 （并将它用作列表项的值），设置的 DropDownList`DataTextField`和`DataValueField`为"UserName"的属性。

下方下拉列表中，添加名为中继器`UsersRoleList`。 此转发器将列出所有角色的系统中为一系列的复选框。 定义转发器的`ItemTemplate`使用声明性的以下标记：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

`ItemTemplate`标记包含名为的单个复选框 Web 控件`RoleCheckBox`。 复选框的`AutoPostBack`属性设置为 True 和`Text`属性绑定到`Container.DataItem`。 原因的数据绑定语法是只需`Container.DataItem`是因为角色框架将返回一个字符串数组，在列表的角色的名称，我们将绑定到中继器此字符串数组。 为何使用此语法显示数组绑定到数据 Web 控件的内容的全面说明是超出了本教程的范围。 有关此问题的详细信息，请参阅[标量数组绑定到数据 Web 控件](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)。

此时你"按用户"的接口的声明性标记应类似以下：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

现在我们就可以编写代码以将用户帐户添加到下拉列表的一套和转发器的角色集的绑定。 在页面的代码隐藏类中，添加一个名为`BindUsersToUserList`，另一个名为`BindRolesList`，使用下面的代码：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

`BindUsersToUserList`方法检索的所有用户帐户通过系统中[`Membership.GetAllUsers`方法](https://msdn.microsoft.com/library/dy8swhya.aspx)。 这将返回[`MembershipUserCollection`对象](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx)，这是一套[`MembershipUser`实例](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)。 然后，此集合将绑定到`UserList`DropDownList。 `MembershipUser`实例的集合包含的各种属性，如该构成`UserName`， `Email`， `CreationDate`，和`IsOnline`。 若要指示要显示的值的 DropDownList`UserName`属性，确保`UserList`DropDownList 的`DataTextField`和`DataValueField`属性已设置为"UserName"。

> [!NOTE]
> `Membership.GetAllUsers`方法有两个重载： 一个可接受任何输入的参数并返回的所有用户，，另一个中的页索引和页大小的整数值采用并返回仅指定用户的子集。 当存在大量的用户帐户的可分页用户界面元素在显示时，第二个重载可以使用到更有效地通过用户的页中，因为它返回的用户帐户，而不是所有这些只是精确的子集。


`BindRolesToList`方法通过调用会启动`Roles`类的[`GetAllRoles`方法](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)，这将返回一个字符串数组，包含系统中的角色。 此字符串数组，然后绑定到转发器。

最后，我们需要在首次加载页时调用这两种方法。 将以下代码添加到 `Page_Load` 事件处理程序中：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

与此代码中的位置，需要花费一段时间来访问通过浏览器; 页你的屏幕应类似于图 2。 所有用户帐户会填充下拉列表中，，在其中，每个角色显示为一个复选框。 因为我们设置`AutoPostBack`的属性的 DropDownList 和复选框为 True，更改所选的用户或选中或取消选中角色导致回发。 但是，因为我们尚未编写代码来处理这些操作，不执行任何操作。 例如，我们将介绍以下两部分中的这些任务。


[![页面显示的用户和角色](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**图 2**: 页面显示的用户和角色 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>检查角色所选的用户属于

当首次加载页时，或者每当在距访客从下拉列表中选择新用户时，我们需要更新`UsersRoleList`的复选框，以便仅当所选的用户属于该角色，一个给定的角色的复选框已选中。 若要实现此目的，创建一个名为方法`CheckRolesForSelectedUser`替换为以下代码：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

通过确定所选的用户是谁启动上面的代码。 然后，它使用角色类的[`GetRolesForUser(userName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)以返回指定的用户组作为字符串数组的角色。 接下来，枚举转发器的项和每个项的`RoleCheckBox`复选框以编程方式引用。 仅当它对应于该角色包含在选中的复选框`selectedUsersRoles`字符串数组。

> [!NOTE]
> `Linq.Enumerable.Contains(Of String)(...)`如果你使用的 ASP.NET 2.0 版中，将不会编译语法。 `Contains(Of String)`方法是的一部分[LINQ 库](http://en.wikipedia.org/wiki/Language_Integrated_Query)，这是新为 ASP.NET 3.5。 如果你仍在使用 ASP.NET 2.0 版中，使用[`Array.IndexOf(Of String)`方法](https://msdn.microsoft.com/library/eha9t187.aspx)相反。


`CheckRolesForSelectedUser`方法需要在两个情况下调用： 首次加载页时，只要`UserList`DropDownList 的所选的索引已更改。 因此，调用此方法从`Page_Load`事件处理程序 (对的调用后`BindUsersToUserList`和`BindRolesToList`)。 此外，为 DropDownList 创建事件处理程序`SelectedIndexChanged`事件并从那里调用此方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

使用此代码中的位置，可以通过浏览器页进行测试。 但是，由于`UsersAndRoles.aspx`页当前不具备将用户分配到角色的功能，没有用户具有的角色。 我们将创建用于将用户分配到角色在稍后，你可以获取我的单词的此代码有效，并可以验证，则它会以更高版本，因此的接口或你可以手动向角色添加用户的方法是插入记录合并到`aspnet_UsersInRoles`若要测试此 functi 表现在 onality。

### <a name="assigning-and-removing-users-from-roles"></a>分配和从角色中删除用户

当在距访客选中或取消选中一个复选框在`UsersRoleList`我们需要添加或从相应的角色中删除所选的用户的转发器。 复选框的`AutoPostBack`属性当前设置为 True，这会导致回发，每当转发器中的复选框为选中或取消选中。 简单地说，我们需要的复选框为创建事件处理程序`CheckChanged`事件。 由于该复选框将在转发器控件中，我们需要手动添加事件处理程序检测。 通过将事件处理程序添加到代码隐藏类作为启动`Protected`方法，如下所示：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

我们将返回以在稍后为此事件处理程序编写代码。 但首先让我们完成的事件处理管道。 在转发器的复选框从`ItemTemplate`，添加`OnCheckedChanged="RoleCheckBox_CheckChanged"`。 此语法条电源线条`RoleCheckBox_CheckChanged`事件处理程序`RoleCheckBox`的`CheckedChanged`事件。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

我们最后一项任务是要完成`RoleCheckBox_CheckChanged`事件处理程序。 我们需要通过引用引发事件，因为此复选框实例告诉我们什么角色已选中或通过取消选中该复选框控件来启动其`Text`和`Checked`属性。 使用所选用户的用户名以及此信息，我们将添加或从通过角色中删除用户`Roles`类的[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)或[`RemoveUserFromRole`方法](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

上面的代码启动通过以编程方式引用引发事件，它可以通过该复选框`sender`输入的参数。 如果选中的复选框，则将所选的用户添加到指定角色，否则为从角色中删除。 在任一情况下，`ActionStatus`标签显示一条消息，汇总只需执行的操作。

需要一段时间来测试通过浏览器的此页。 选择用户 Tito，然后将 Tito 添加到管理员和主管角色。


[![Tito 已添加到管理员和主管角色](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**图 3**: Tito 已添加到管理员和主管角色 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image9.png))


接下来，从下拉列表中选择用户罗斯向她。 没有回发，并通过更新转发器的复选框`CheckRolesForSelectedUser`。 由于罗斯向她不尚未属于任何角色，两个复选框均未选中。 接下来，将罗斯向她添加到主管角色。


[![罗斯向她已添加到主管角色](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**图 4**： 罗斯向她已添加到主管角色 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image12.png))


若要进一步验证功能`CheckRolesForSelectedUser`方法中，选择一个 Tito 或罗斯向她以外的用户。 请注意自动取消选中复选框的方式，表示它们并不属于任何角色。 返回到 Tito。 应检查的管理员和主管复选框。

## <a name="step-2-building-the-by-roles-user-interface"></a>步骤 2： 生成"通过角色"用户界面

此时我们已完成"的用户"接口，并准备好开始处理"的角色"接口。 "按角色"接口提示用户从下拉列表中选择一个角色，然后显示组属于一个 GridView 中的角色的用户。

另一个的 DropDownList 将控件添加到`UsersAndRoles.aspx page`。 将此某个中继器控件下，将其`RoleList`，并设置其`AutoPostBack`属性为 True。 下方，添加一个 GridView，并将其命名`RolesUserList`。 此 GridView 将列出属于所选角色的用户。 设置 GridView`AutoGenerateColumns`属性设置为 False，将为 TemplateField 添加到网格的`Columns`集合，并设置其`HeaderText`的"用户"的属性。 定义 TemplateField `ItemTemplate` ，使其显示数据绑定表达式的值`Container.DataItem`中`Text`属性名为的标签`UserNameLabel`。

在添加和配置 GridView 之后, 你"按角色"的接口的声明性标记应看起来类似以下：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

我们需要填充`RoleList`DropDownList 与系统中的角色集。 若要实现此目的，更新`BindRolesToList`方法，这就是绑定以便通过返回的字符串数组`Roles.GetAllRoles`方法`RolesList`DropDownList (以及`UsersRoleList`转发器)。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

中的最后两行`BindRolesToList`方法已添加要绑定到的角色集`RoleList`DropDownList 控件。 图 5 显示的最终结果时查看通过浏览器 – 使用系统的角色填充下拉列表。


[![角色显示在 RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**图 5**: 角色将显示在`RoleList`DropDownList ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>显示属于所选角色的用户

当首次加载页，或从选中新的角色时`RoleList`DropDownList，我们需要以显示属于 GridView 中的角色的用户的列表。 创建一个名为方法`DisplayUsersBelongingToRole`使用下面的代码：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

此方法会启动通过获取从所选的角色`RoleList`DropDownList。 然后，它使用[`Roles.GetUsersInRole(roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)检索属于该角色的用户的用户名的字符串数组。 然后，此数组绑定到`RolesUserList`GridView。

需要在两个的情况下调用此方法： 最初加载页面时以及在所选的角色`RoleList`DropDownList 更改。 因此，更新`Page_Load`事件处理程序，以便在调用后调用此方法`CheckRolesForSelectedUser`。 接下来，创建的事件处理程序`RoleList`的`SelectedIndexChanged`事件，并从那里，太调用此方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

与就地情况下，此代码`RolesUserList`GridView 应显示那些属于所选角色的用户。 如图 6 所示，主管角色都包含两个成员： 罗斯向她和 Tito。


[![GridView 列出那些属于所选角色的用户](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**图 6**: GridView 列出那些用户到属于所选角色 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>从所选角色中删除用户

让我们增加`RolesUserList`GridView，以使其包括的列"删除"按钮。 单击特定用户的"删除"按钮将其从该角色中删除。

开始删除按钮字段添加到 GridView。 使此字段显示为左边最字段并更改其`DeleteText`属性从"删除"（默认值） 为"删除"。


[![添加](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**图 7**： 将"删除"按钮添加到 GridView ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image21.png))


单击"删除"按钮时回发时，才会和 GridView`RowDeleting`引发事件。 我们需要创建此事件的事件处理和编写代码，从所选角色中删除用户。 创建事件处理程序，然后添加以下代码：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

代码将启动通过确定所选的角色的名称。 然后，它以编程方式引用`UserNameLabel`控件从其"删除"按钮被单击以确定要删除用户的用户名的行。 然后从通过调用角色中删除该用户`Roles.RemoveUserFromRole`方法。 `RolesUserList` GridView 然后刷新，并且通过显示一条消息`ActionStatus`标签控件。

> [!NOTE]
> "删除"按钮不需要任何种类的用户之前从角色中删除用户的确认。 我恳请你添加某一级别的用户进行确认。 确认某项操作的最简单方法之一是通过客户端确认对话框。 有关此技术的详细信息，请参阅[删除时添加客户端确认](https://asp.net/learn/data-access/tutorial-42-vb.aspx)。


图 8 显示的页后从主管组中删除用户 Tito。


[![哎呀，Tito 不再主管](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**图 8**： 哎呀，Tito 不再主管 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>将新用户添加到所选角色

从所选角色中删除用户，以及此页面在距访客还应能够将用户添加到所选角色。 将用户添加到所选角色的最佳界面取决于你预期获得的用户帐户数。 如果你的网站将位于只需少量几十个用户帐户或更少，可以在此处使用的 DropDownList。 可能有数千个用户帐户，如果你想要包括允许访问者可以逐页查看的帐户，一个特定帐户，请搜索或筛选以某种其他方式的用户帐户的用户界面。

此页让我们使用在系统中工作而不考虑用户帐户数非常简单的界面。 也就是说，我们将使用文本框中，提示在距访客键入她希望将添加到所选角色的用户的用户名。 如果不存在具有该名称的任何用户，或者如果用户已是角色的成员，我们将显示在一条消息`ActionStatus`标签。 但如果用户存在并不是角色的成员，我们将它们添加到角色，刷新该网格。

添加一个文本框和按钮下 GridView。 设置文本框的`ID`到`UserNameToAddToRole`并设置按钮的`ID`和`Text`属性设置为`AddUserToRoleButton`和"添加用户到角色"，分别。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

接下来，创建`Click`事件处理程序`AddUserToRoleButton`并添加以下代码：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

中的代码的大部分`Click`事件处理程序执行各种验证检查。 它可确保在距访客提供中的用户名`UserNameToAddToRole`文本框中，用户是否存在在系统中，以及它们已不属于所选角色。 如果下列任一检查失败，相应的消息将显示在`ActionStatus`并退出事件处理程序。 如果通过了所有检查，将用户添加到通过角色`Roles.AddUserToRole`方法。 之后，文本框中的`Text`属性已清除了、 刷新 GridView，则与`ActionStatus`标签显示一条消息，该值指示指定的用户已成功添加到所选角色。

> [!NOTE]
> 若要确保不已属于所选角色指定的用户，我们使用[`Roles.IsUserInRole(userName, roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)，这将返回一个布尔值，该值指示是否*用户名*属于*roleName*。 我们将使用此方法在再次<a id="_msoanchor_2"> </a>[下一教程](role-based-authorization-vb.md)我们在查看基于角色的授权时。


访问通过浏览器页面并选择从主管角色`RoleList`DropDownList。 请尝试输入无效的用户名 – 你应看到一条消息说明用户不存在系统中。


[![不能将不存在用户添加到角色](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**图 9**： 不能将不存在用户添加到角色 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image27.png))


现在尝试添加的有效用户。 继续操作并将 Tito 重新添加到主管角色。


[![Tito 是再一次主管 ！](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**图 10**: Tito 是再一次主管 ！  ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>步骤 3： 跨更新"由用户"和"由角色"接口

`UsersAndRoles.aspx`页提供用于管理用户和角色的两个不同的接口。 目前，这两个接口独立于另一个操作，因此很可能，在一个接口中所做的更改将不会立即反映另一部分中。 例如，想象一下这样到页访问者选择的主管角色`RoleList`作为其成员列出罗斯向她和 Tito 的 DropDownList。 接下来，在距访客选择从 Tito`UserList`签入的管理员和主管复选框的 DropDownList`UsersRoleList`转发器。 如果在距访客然后取消选中主管角色从转发器，Tito 删除从主管角色，但此修改不会反映在"按角色"的接口中。 GridView 仍将显示 Tito 视为主管角色的成员。

若要修复此，我们需要刷新 GridView，只要角色是 checked 或 unchecked 从`UsersRoleList`转发器。 同样，我们需要删除或从"按角色"接口向角色中添加用户时刷新转发器。

在"由用户"界面中继器刷新通过调用`CheckRolesForSelectedUser`方法。 "按角色"接口可以修改在`RolesUserList`GridView 的`RowDeleting`事件处理程序和`AddUserToRoleButton`按钮的`Click`事件处理程序。 因此，我们需要调用`CheckRolesForSelectedUser`从每个这些方法的方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

同样，在"按角色"界面 GridView 刷新通过调用`DisplayUsersBelongingToRole`方法和"按用户"接口修改通过`RoleCheckBox_CheckChanged`事件处理程序。 因此，我们需要调用`DisplayUsersBelongingToRole`来自此事件处理程序方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

与这些细微的代码更改后，"由用户"和"由角色"接口现在正确跨更新。 若要验证这一点，请访问通过浏览器页面，然后选择 Tito 和从主管`UserList`和`RoleList`DropDownLists，分别。 请注意你取消勾选"由用户"界面中中继器从 Tito 主管角色，如 Tito 将会自动删除从在"按角色"界面 GridView。 将添加 Tito 回主管角色从"按角色"接口自动重新检查"由用户"接口中的主管复选框。

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>步骤 4： 自定义 CreateUserWizard 以包括"指定角色"的步骤

在<a id="_msoanchor_3"> </a> [*创建用户帐户*](../membership/creating-user-accounts-vb.md)教程，我们已了解如何使用 CreateUserWizard Web 控件来提供用于创建新的用户帐户的接口。 通过可在两种方式之一：

- 作为一种方法，为访客在站点上，创建他们自己的用户帐户和
- 作为一种方法，管理员可以创建新帐户

在第一个使用情况下，访问者进入这个网站，并填写 CreateUserWizard，以便在站点上注册输入他们的信息。 在第二个用例，管理员将给其他人创建一个新的帐户。

如果通过某些其他人的管理员创建帐户后，可能很有帮助，可允许管理员指定的新用户帐户属于哪些角色。 在<a id="_msoanchor_4"> </a> [*存储**其他用户信息*](../membership/storing-additional-user-information-vb.md)我们已了解如何通过添加其他自定义 CreateUserWizard 的教程`WizardSteps`. 让我们看一下如何将一个额外的步骤添加到 CreateUserWizard，以指定新用户的角色。

打开`CreateUserWizardWithRoles.aspx`页上，添加名为通过`RegisterUserWithRoles`。 设置控件的`ContinueDestinationPageUrl`属性设置为"~ / Default.aspx"。 其目的是，管理员将使用此通过创建新用户帐户，因为设置控件的`LoginCreatedUser`属性设置为 False。 这`LoginCreatedUser`属性指定是否在距访客自动登录刚创建的用户，并且它默认为 True。 我们将其设置为 False 则因为当管理员创建新帐户时，我们想要保留他自己作为登录。

接下来，选择"添加/删除`WizardSteps`..." 选项从 CreateUserWizard 的智能标记，然后添加一个新`WizardStep`，设置其`ID`到`SpecifyRolesStep`。 移动`SpecifyRolesStep WizardStep`，以便它是在"登录向上 for 新帐户"步骤中之后, 但在"完成"步骤之前。 设置`WizardStep`的`Title`属性设置为"指定的角色"其`StepType`属性`Step`，并将其`AllowReturn`属性设置为 False。


[![添加](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**图 11**： 添加"指定角色"`WizardStep`到 CreateUserWizard ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image33.png))


此更改之后 CreateUserWizard 的声明性标记应如下所示：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

在"指定角色" `WizardStep`，添加名为按`RoleList.`此如何将列表的可用角色中，启用用户访问的页面检查哪些角色的新创建的用户属于。

我们仍然面临着两个编码任务： 首先我们必须填充`RoleList`系统; 中的角色与按第二个，我们需要将创建的用户添加到所选角色中，当用户从"指定角色"的步骤移到"完成"步骤。 我们可以完成的第一个任务`Page_Load`事件处理程序。 下面的代码以编程方式引用`RoleList`复选框的第一天到页访问，并将角色中系统绑定到它。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

上面的代码看起来应熟悉。 在<a id="_msoanchor_5"> </a> [*存储**其他用户信息*](../membership/storing-additional-user-information-vb.md)教程，我们使用了两个`FindControl`语句来引用 Web 控件从自定义内`WizardStep`。 并将角色绑定到按的代码摘自此前在本教程。

若要执行第二个的编程任务，我们需要知道"指定角色"步骤完成时。 回想一下，CreateUserWizard 具有`ActiveStepChanged`触发的事件，每次在距访客从某一步导航到另一个。 此处我们可以确定用户是否已达到"完成"步骤中;如果是这样，我们需要将用户添加到所选角色。

创建的事件处理程序`ActiveStepChanged`事件并添加以下代码：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

如果用户只需已达到"已完成"步骤，事件处理程序进行枚举的项`RoleList`按和刚创建的用户分配给所选角色。

访问此页面通过浏览器。 CreateUserWizard 的第一步是标准的"登录向上 for 新帐户"步骤，该对话框提示输入新用户的用户名、 密码、 电子邮件和其他密钥的信息。 输入要创建名为 Wanda 的新用户的信息。


[![创建名为 Wanda 的新用户](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**图 12**： 创建新的用户名为 Wanda ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image36.png))


单击"创建用户"按钮。 CreateUserWizard 内部调用`Membership.CreateUser`方法，创建新用户帐户，然后将前进到下一步中，"指定角色。" 此处列出的系统角色。 检查主管复选框，然后单击下一步。


[![请 Wanda 主管角色的成员](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**图 13**： 请 Wanda 主管角色的成员 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image39.png))


单击下一步会导致回发和更新`ActiveStep`到"完成"步骤。 在`ActiveStepChanged`事件处理程序的最近创建的用户帐户分配给主管角色。 若要验证这一点，返回到`UsersAndRoles.aspx`页，选择从主管`RoleList`DropDownList。 如图 14 所示，两个监控器现在组成的三个用户： 罗斯向她、 Tito 和 Wanda。


[![罗斯向她、 Tito 和 Wanda 是所有主管](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**图 14**： 罗斯向她、 Tito 和 Wanda 是所有主管 ([单击以查看实际尺寸的图像](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>摘要

角色 framework 可提供用于检索有关特定用户的角色和方法来确定哪些用户属于指定角色信息的方法。 此外，有多种方法来添加和删除一个或多个角色的一个或多个用户。 在本教程中我们侧重于只使用两个这些方法：`AddUserToRole`和`RemoveUserFromRole`。 出现了用于将多个用户添加到单个角色并将多个角色分配给单个用户的其他变体。

本教程还包括一看扩展通过包括`WizardStep`指定新创建用户角色。 此类步骤可帮助管理员简化创建的新用户的用户帐户的过程。

此时我们已了解如何创建和删除角色以及如何添加和从角色中删除用户。 但我们尚无一下应用基于角色的授权。 在<a id="_msoanchor_6"> </a>[以下教程](role-based-authorization-vb.md)我们将着眼于基于角色的角色定义 URL 授权规则以及如何根据当前已登录的用户的角色来限制页级功能。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [ASP.NET 网站管理工具概述](https://msdn.microsoft.com/library/ms228053.aspx)
- [正在检查 ASP。NET 的成员资格、 角色和配置文件](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [滚动你自己的网站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢...

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](creating-and-managing-roles-vb.md)
[下一页](role-based-authorization-vb.md)
