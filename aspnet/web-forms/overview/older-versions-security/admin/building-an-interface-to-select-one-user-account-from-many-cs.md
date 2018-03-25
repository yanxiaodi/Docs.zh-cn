---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: "生成一个接口，以便从很多的 (C#) 中选择一个用户帐户 |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将生成与分页，可筛选网格的用户界面。 具体而言，我们的用户界面将包含的一系列的 LinkButtons..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 42a8fb48b8c8cfb653ac4d64f6efe011f92b966b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>生成一个接口，以便从很多的 (C#) 中选择一个用户帐户
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip)或[下载 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> 在本教程中我们将生成与分页，可筛选网格的用户界面。 具体而言，我们的用户界面将包含的一系列 LinkButtons 筛选基于的用户名，以及用来显示匹配的用户的 GridView 控件的起始字母结果。 我们将开始通过列出所有 GridView 中的用户帐户。 然后，在步骤 3 中，我们将添加筛选器 LinkButtons。 步骤 4 来看待分页筛选的结果。 在步骤 2 中构造至 4 的接口将在后续教程，用于执行针对特定用户帐户管理任务。


## <a name="introduction"></a>介绍

在<a id="_msoanchor_1"> </a> [*向用户分配角色*](../roles/assigning-roles-to-users-cs.md)教程中，我们创建的管理员可以选择用户和管理其角色的基本接口。 具体而言，该接口向管理员显示的所有用户的下拉列表。 此类接口适合，当存在但十几用户帐户，但臃肿针对包含数百或数千个帐户的网站。 分页，可筛选网格是具有大型用户基的网站的更适合用户界面。

在本教程中我们将生成此类用户界面。 具体而言，我们的用户界面将包含的一系列 LinkButtons 筛选基于的用户名，以及用来显示匹配的用户的 GridView 控件的起始字母结果。 我们将开始通过列出所有 GridView 中的用户帐户。 然后，在步骤 3 中，我们将添加筛选器 LinkButtons。 步骤 4 来看待分页筛选的结果。 在步骤 2 中构造至 4 的接口将在后续教程，用于执行针对特定用户帐户管理任务。

让我们进入正题！

## <a name="step-1-adding-new-aspnet-pages"></a>步骤 1： 添加新的 ASP.NET 页

我们将在本教程和后面的两个检查各种与管理相关的函数和功能。 我们将需要一系列的 ASP.NET 网页来实现检查整个这些教程的主题。 让我们创建这些页面和更新站点映射。

首先创建一个新文件夹中名为的项目`Administration`。 接下来，将两个新的 ASP.NET 页添加到文件夹，链接与每个页面`Site.master`母版页。 命名页：

- `ManageUsers.aspx`
- `UserInformation.aspx`

此外将两个页添加到网站的根目录：`ChangePassword.aspx`和`RecoverPassword.aspx`。

这些四个页面，在这种情况下，应有两个内容控件，每个母版页的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

我们希望能够显示主控页的默认标记`LoginContent`ContentPlaceHolder 为这些页面。 因此，删除的声明性标记`Content2`内容控件。 这么做，页的标记应包含几个内容控件。

ASP.NET 页`Administration`文件夹旨在仅供管理用户。 我们将管理员角色添加到中的系统<a id="_msoanchor_2"> </a> [*创建和管理角色*](../roles/creating-and-managing-roles-cs.md)教程; 限制对这些两个页面到此角色的访问。 若要完成此操作，将添加`Web.config`文件为`Administration`文件夹和配置其`<authorization>`元素来承认管理员角色中的用户，并拒绝所有其他人。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

此时你的项目的解决方案资源管理器应类似于的屏幕快照中图 1 所示。


[![四个新页面和 Web.config 文件添加到网站](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**图 1**： 四个新页面和`Web.config`文件已添加到网站 ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


最后，更新站点图 (`Web.sitemap`) 以包括进入`ManageUsers.aspx`页。 添加以下 XML 后的`<siteMapNode>`我们添加了角色教程。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

使用站点映射更新，请访问通过浏览器的站点。 如图 2 所示，在左侧的导航现在将包括管理教程的项。


[![站点图包括标题为用户管理的节点](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**图 2**： 站点图包括节点标题为的用户管理 ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>步骤 2： 列出一个 GridView 中的所有用户帐户

本教程中我们最终目标是创建一个分页，可筛选网格，通过该管理员可以选择要管理的用户帐户。 让我们开始通过列出*所有*GridView 中的用户。 完成此操作后，我们将添加的筛选和分页接口和功能。

打开`ManageUsers.aspx`页面`Administration`文件夹并添加一个 GridView，设置其`ID`到`UserAccounts`。 在稍后，我们将编写代码以将用户帐户的组绑定到 GridView 使用`Membership`类的`GetAllUsers`方法。 如前面的教程中所述，GetAllUsers 方法返回`MembershipUserCollection`对象，这是集合的`MembershipUser`对象。 每个`MembershipUser`集合中包含属性，例如`UserName`， `Email`， `IsApproved`，依次类推。

为了在 GridView 中显示的所需的用户帐户信息，请将设置 GridView`AutoGenerateColumns`属性设置为 False 并添加为 BoundFields `UserName`， `Email`，和`Comment`属性和为 CheckBoxFields `IsApproved`，`IsLockedOut`，和`IsOnline`属性。 通过控件的声明性标记或通过字段对话框中，可以应用此配置。 图 3 显示的字段的屏幕截图的自动生成字段复选框已取消选中并添加并配置 BoundFields 和 CheckBoxFields 后，此对话框。


[![将三个 BoundFields 和三个 CheckBoxFields 添加到 GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**图 3**： 添加三个 BoundFields 和三个 CheckBoxFields 到 GridView ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


在配置后你 GridView，确保其声明性的标记如下所示：

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

接下来，我们需要编写代码，将用户帐户绑定到 GridView。 创建一个名为方法`BindUserAccounts`若要执行此任务，然后调用它从`Page_Load`上第一次的页访问的事件处理程序。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

需要一段时间来测试通过浏览器页面。 如图 4 所示， `UserAccounts` GridView 列出用户名、 电子邮件地址和所有用户的其他相关的帐户信息系统中。


[![在 GridView 中列出的用户帐户](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**图 4**: GridView 中列出的用户帐户 ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>步骤 3： 筛选结果的第一个字母的用户名

当前`UserAccounts`GridView 显示*所有*的用户帐户。 对于具有数百或数千个用户帐户的网站，它是命令性该用户能够快速削减显示的帐户。 这可以通过添加到页筛选 LinkButtons 来实现。 让我们向页面添加 27 LinkButtons： 一个，标题为所有以及一个 LinkButton 字母表中的每个字母的。 如果访问者单击所有 LinkButton，GridView 将显示的所有用户。 如果他们单击特定字母，只有其用户名以所选的字母开头的用户将会显示。

我们第一项任务是添加 27 LinkButton 控件。 一个选项就是以声明方式，创建 27 LinkButtons 一次一个地。 更灵活的方法是使用具有的转发器控件`ItemTemplate`，呈现 LinkButton，然后将筛选选项绑定到作为中继器`string`数组。

通过将重复器控件添加到上述页面中的开始`UserAccounts`GridView。 设置转发器的`ID`属性`FilteringUI`。 配置转发器的模板，以便其`ItemTemplate`呈现 LinkButton 其`Text`和`CommandName`属性绑定到当前数组元素。 正如我们看到在<a id="_msoanchor_3"> </a> [*向用户分配角色*](../roles/assigning-roles-to-users-cs.md)教程，则可以使用完成这`Container.DataItem`数据绑定语法。 使用转发器的`SeparatorTemplate`以显示一条竖线之间每个链接。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

若要填充此转发器与所需的筛选选项，创建一个名为方法`BindFilteringUI`。 一定要调用此方法从`Page_Load`在第一个页面加载的事件处理程序。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

此方法作为中的元素中指定的筛选选项`string`数组`filterOptions`。 对于数组中每个元素，转发器将呈现与 LinkButton 其`Text`和`CommandName`属性分配给数组元素的值。

图 5 所示`ManageUsers.aspx`页上查看通过浏览器时。


[![转发器列出 27 筛选 LinkButtons](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**图 5**: 转发器列出 27 筛选 LinkButtons ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> 用户名可能会启动任何字符，包括数字和标点。 若要查看这些帐户，管理员将需要使用所有 LinkButton 选项。 或者，你可以添加 LinkButton 返回以数字开头的所有用户帐户。 我将此作为练习为读取器。


单击任意筛选 LinkButtons 导致回发并引发转发器的`ItemCommand`事件，但不没有在网格中的任何更改因为我们尚未为编写任何代码来筛选结果。 `Membership`类包括[`FindUsersByName`方法](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)返回其用户名与指定的搜索模式匹配的那些用户帐户。 我们可以使用此方法来检索其用户名以指定的字母开头的用户帐户`CommandName`的被单击筛选 LinkButton。

通过更新开始`ManageUser.aspx`页的代码隐藏类以使其包括一个名为属性`UsernameToMatch`。 此属性在回发期间保持用户名筛选器字符串：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

`UsernameToMatch`属性将分配到其值存储`ViewState`使用集合键用户名。 当读取此属性的值时，它会检查以确定一个值是否存在于`ViewState`集合; 如果不正确，则返回默认值为空字符串。 `UsernameToMatch`属性表现出一种常见模式，即保留一个值，以查看状态，以便在回发期间会保留对该属性的任何更改。 此模式的详细信息，请阅读[了解 ASP.NET 视图状态](https://msdn.microsoft.com/library/ms972976.aspx)。

接下来，更新`BindUserAccounts`方法的调用，因此`Membership.GetAllUsers`，它调用`Membership.FindUsersByName`，并传递的值中`UsernameToMatch`属性追加 SQL 通配符字符，%。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

若要显示其用户名以字母 A 开头的用户，请设置`UsernameToMatch`到 A 的属性，然后调用`BindUserAccounts`。 这将导致调用`Membership.FindUsersByName("A%")`，这样就会返回所有用户其用户名开头 a。 同样，若要都返回*所有*用户，分配到一个空字符串`UsernameToMatch`属性以便`BindUserAccounts`方法将调用`Membership.FindUsersByName("%")`，从而返回所有用户帐户。

为转发器的创建事件处理程序`ItemCommand`事件。 单击其中一个筛选器 LinkButtons; 时将引发此事件它传递单击的 LinkButton`CommandName`值通过`RepeaterCommandEventArgs`对象。 我们需要将相应的值赋给`UsernameToMatch`属性，然后调用`BindUserAccounts`方法。 如果`CommandName`为 All，分配到一个空字符串`UsernameToMatch`，以便显示所有用户帐户。 否则，将分配`CommandName`值赋给`UsernameToMatch`。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

此代码中的位置，测试所筛选的功能。 首次访问页，将显示所有用户帐户 （回头参考图 5 中）。 单击 LinkButton 导致回发，并筛选结果，显示以 A 开头的用户帐户。


[![使用筛选 LinkButtons 显示其用户名的某些字母开头的用户](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**图 6**： 筛选 LinkButtons 用于显示这些用户其用户名以某些字母开头 ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>步骤 4： 更新 GridView 用于分页

图 5 和 6 中所示 GridView 列出所有从返回的记录`FindUsersByName`方法。 如果有数百或数千个用户帐户这可能导致信息重载，查看时的所有帐户 （或最初中访问的页面时单击所有 LinkButton 是这种情况）。 为了帮助更易于管理的小区块中提供的用户帐户，让我们配置 GridView 以一次显示 10 个用户帐户。

GridView 控件提供两种类型的分页：

- **默认分页**-易于实现，但效率低下。 简而言之，使用默认分页 GridView 需要*所有*从其数据源的记录。 它然后仅显示合适的页面的记录。
- **自定义分页**-需要更多的工作，若要实现，但比默认分页因为与将数据分页的自定义源只返回精确集要显示的记录更高效。

当数千条记录进行分页时，可以极大地默认和自定义分页之间的性能差异。 因为我们要生成假定此接口可能是数百或数千个用户帐户，我们将使用自定义分页。

> [!NOTE]
> 有关默认和自定义分页，以及实现自定义分页中涉及的挑战之间的差异的详细讨论，请参阅[高效地分页通过大型金额的数据](https://asp.net/learn/data-access/tutorial-25-cs.aspx)。 默认和自定义分页之间的性能差异一些分析，请参阅[在 ASP.NET 中通过 SQL Server 2005 的自定义分页](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)。


若要实现自定义分页我们首先需要某种机制，用于检索 GridView 显示的记录的精确的子集。 好消息是`Membership`类的`FindUsersByName`方法的重载，使我们可以指定的页索引和页面大小，并且返回仅记录该范围内的这些用户帐户。

具体而言，此重载具有以下签名： [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx)。

*PageIndex*参数指定要返回; 用户帐户的页*pageSize*指示每页显示的记录数量。 *TotalRecords*参数是`out`在用户存储区返回的总用户帐户数的参数。

> [!NOTE]
> 返回的数据`FindUsersByName`按 username; 不能自定义的排序条件。


GridView 可以配置为使用自定义分页，但仅当绑定到 ObjectDataSource 控件。 要实现自定义分页的 ObjectDataSource 控件，它需要两个方法： 开始行索引和记录，若要显示，最大数量传入一个，并返回文本范围; 内的记录的精确子集和通过返回的记录总数的方法正在通过寻呼发送。 `FindUsersByName`重载接受的页索引和页面大小，并返回通过记录的总数`out`参数。 因此，此处接口不匹配。

一个选项就是创建公开对象数据源要求，并且内部调用的接口的代理类`FindUsersByName`方法。 另一个选项-，另一个我们将为此项目使用的是创建我们自己分页接口，并使用，而不是 GridView 的内置分页接口。

### <a name="creating-a-first-previous-next-last-paging-interface"></a>创建第一个，以前，接下来，上次分页接口

让我们使用第一个、 上一步、 下一步，和最后一个 LinkButtons 生成分页接口。 第一个 LinkButton，单击时，将采用用户的第一页数据，而上一步将返回他到以前的页面。 同样下, 一步，最后将用户移到下一个和最后一页上，分别。 添加下面四个 LinkButton 控件`UserAccounts`GridView。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

接下来，为每个 LinkButton 创建事件处理程序`Click`事件。

图 7 显示了四个 LinkButtons 通过 Visual Web Developer 设计视图中查看时。


[![接下来，添加第一个、 上一个、 和最后一个 LinkButtons 下 GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**图 7**： 添加第一个、 上一步、 下一步，和最后一个 LinkButtons 下 GridView ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>跟踪的当前页索引

当用户第一次访问`ManageUsers.aspx`页或单击任一筛选按钮，我们想要在 GridView 中显示数据的第一页。 当用户单击导航 LinkButtons 之一时，但是，我们需要更新的页索引。 若要维护的页索引和要每页显示的记录数，添加到页面的代码隐藏类的以下两个属性：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

如`UsernameToMatch`属性，`PageIndex`属性保持其视图状态的值。 只读`PageSize`属性返回的硬编码值，10。 我恳请感读取器中，若要更新此属性，以使用相同模式`PageIndex`，然后增加`ManageUsers.aspx`页上，以便访问的页面的人员可以指定多少个用户帐户以显示每个页上。

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>检索当前页的记录为止，更新页索引，并启用和禁用分页接口 LinkButtons

使用分页接口可就地和`PageIndex`和`PageSize`添加属性，我们已准备好更新`BindUserAccounts`方法，以便它使用相应`FindUsersByName`重载。 此外，我们需要将此方法启用或禁用分页接口，具体取决于要显示哪些页。 在查看数据的第一页时，应禁用第一个和上一步链接;当查看的最后一页时，应禁用下一步和上一次。

使用以下代码更新 `BindUserAccounts` 方法：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

请注意总正在通过寻呼发送到的记录数由的最后一个参数`FindUsersByName`方法。 这是`out`参数，因此我们需要先声明一个变量以保存此值 (`totalRecords`)，然后它前面加`out`关键字。

返回指定的页面的用户帐户后，四个 LinkButtons 可以启用或禁用，具体取决于是否正在查看数据的第一个或最后一页。

最后一步是为四个 LinkButtons 的编写代码`Click`事件处理程序。 这些事件处理程序需要更新`PageIndex`属性，然后将重新绑定数据到通过调用 GridView `BindUserAccounts`。 第一个、 上一步和下一步的事件处理程序就非常简单。 `Click`事件处理程序上次 LinkButton，但是，很有点复杂，因为我们需要确定显示多少条记录的以便确定最后一个的页索引。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

图 8 和 9 在操作中展示的自定义分页接口。 图 8 显示`ManageUsers.aspx`页上查看所有用户帐户的数据的第一页时。 请注意，只能以 10 为 13 帐户的显示。 单击下一步或上一次链接导致回发，更新`PageIndex`为 1，而第二页的用户帐户添加到网格的绑定 （请参阅图 9）。


[![显示第一个 10 用户帐户](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**图 8**： 显示第一个 10 用户帐户 ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![单击下一步链接显示用户帐户的第二页](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**图 9**： 单击下一步链接将显示第二个页面的用户帐户 ([单击以查看实际尺寸的图像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>摘要

管理员通常需要从帐户列表中选择一个用户。 在前面的教程中我们讨论在使用下拉列表填充对于用户，但这种方法可伸缩性差。 在本教程中我们探讨了更好的选择： 其结果显示在分页 GridView 可筛选接口。 与此用户界面，管理员可以快速、 高效地查找和选择千位之间的一个用户帐户。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [与 SQL Server 2005 的 ASP.NET 中的自定义分页](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [有效地分页的数据量大](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [滚动你自己的网站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Alicja Maziarz。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一篇](recovering-and-changing-passwords-cs.md)
