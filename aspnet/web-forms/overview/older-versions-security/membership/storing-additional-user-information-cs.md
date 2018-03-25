---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: "存储的其他用户信息 (C#) |Microsoft 文档"
author: rick-anderson
description: "在本教程中我们将通过生成一个非常基本的来宾簿应用程序来回答这个问题。 在此情况下，我们将查看 modeli 的不同选项..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 38dfdab395b6b3559d293c5b1488ede2e88bfe91
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
---
<a name="storing-additional-user-information-c"></a>存储的其他用户信息 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip)或[下载 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> 在本教程中我们将通过生成一个非常基本的来宾簿应用程序来回答这个问题。 在此情况下，我们将查看建模在数据库中，用户信息的不同选项，，然后查看如何将此数据与成员资格 framework 创建的用户帐户相关联。


## <a name="introduction"></a>介绍

ASP。NET 的成员资格框架提供了灵活的界面，用于管理用户。 成员资格 API 包括用于验证凭据、 检索有关当前登录用户的信息、 创建新的用户帐户，和删除用户帐户，以及其他方法。 成员资格 framework 中的每个用户帐户包含正在验证凭据和执行基本的用户帐户相关的任务所需的属性。 这通过方法和属性出现[`MembershipUser`类](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)，该模型的成员资格 framework 中的用户帐户。 此类的属性，例如[ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx)， [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)，和[ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)，和等方法[ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx)和[ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)。

通常，应用程序需要存储在成员资格 framework 中不包括的其他用户信息。 例如，一家网上零售商可能需要让每个用户存储其传送和帐单邮寄地址、 付款信息、 传送首选项和联系人的电话号码。 此外，在系统中的每个订单都与特定的用户帐户关联。

`MembershipUser`类不包括属性，例如`PhoneNumber`或`DeliveryPreferences`或`PastOrders`。 我们因此如何跟踪所需的应用程序的用户信息并将其与成员资格 framework 集成？ 在本教程中我们将通过生成一个非常基本的来宾簿应用程序来回答这个问题。 在此情况下，我们将查看建模在数据库中，用户信息的不同选项，，然后查看如何将此数据与成员资格 framework 创建的用户帐户相关联。 让我们进入正题！

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>步骤 1： 创建来宾簿应用程序的数据模型

有多种可用于捕获用户数据库中的信息并将其与成员资格 framework 创建的用户帐户相关联的技术。 为了演示了这些技术，我们将需要增加教程 web 应用程序，以便它会捕获某种形式的用户相关数据。 (目前，应用程序的数据模型包含仅应用程序服务所需的表`SqlMembershipProvider`。)

让我们创建一个非常简单的来宾簿应用身份验证的用户可以在其中发表评论。 除了存储来宾簿注释，让我们允许每个用户存储其主镇、 主页上和签名。 如果提供，用户的主镇主页上和签名会出现在他已处于来宾簿每条消息。

### <a name="adding-theguestbookcommentstable"></a>添加`GuestbookComments`表

为了捕获来宾簿注释，我们需要创建一个名为的数据库表`GuestbookComments`具有类似的列`CommentId`， `Subject`， `Body`，和`CommentDate`。 我们还需要在有每个记录`GuestbookComments`表引用保留注释的用户。

若要将此表添加到我们的数据库中，转到 Visual Studio 中的数据库资源管理器，并向下钻取`SecurityTutorials`数据库。 右键单击表文件夹，然后选择添加新表。 这会打开，我们可以定义新的表的列的接口。


[![将新表添加到 SecurityTutorials 数据库](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**图 1**： 添加新表的`SecurityTutorials`数据库 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image3.png))


接下来，定义`GuestbookComments`的列。 首先，通过添加一个名为列`CommentId`类型的`uniqueidentifier`。 此列将唯一地标识每个来宾簿中的注释，因此不允许`NULL`s 和将其标记为表的主键。 而不是提供的值`CommentId`上每个字段`INSERT`，我们可以指示新`uniqueidentifier`值应自动生成为此字段上`INSERT`通过将列的默认值设置为`NEWID()`。 在添加此第一个字段，将其标记为主键，并设置为其默认值之后, 你的屏幕应类似于的屏幕快照中图 2 所示。


[![添加一个名为 CommentId 的主列](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**图 2**： 添加主列命名为`CommentId`([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image6.png))


接下来，添加一个名为列`Subject`类型的`nvarchar(50)`和一个名为列`Body`类型的`nvarchar(MAX)`、 允许`NULL`这两个列中的 s。 接下来，添加一个名为列`CommentDate`类型的`datetime`。 禁止`NULL`s 和组`CommentDate`列的默认值为`getdate()`。

所有这些剩下是若要添加将用户帐户与每个来宾簿注释相关联的列。 一个选项是在添加一个名为列`UserName`类型的`nvarchar(256)`。 这是适当的选择，而不使用成员资格提供程序时`SqlMembershipProvider`。 但在使用时`SqlMembershipProvider`，因为我们在本教程系列`UserName`中的列`aspnet_Users`表不能保证是唯一的。 `aspnet_Users`表的主键是`UserId`并且类型是`uniqueidentifier`。 因此，`GuestbookComments`表需要一个名为列`UserId`类型的`uniqueidentifier`(不允许`NULL`值)。 请继续并添加此列。

> [!NOTE]
> 如我们所述[*在 SQL Server 中创建成员身份架构*](creating-the-membership-schema-in-sql-server-cs.md)教程，则成员资格框架专门用于多个 web 应用程序可以使用不同的用户帐户共享同一个用户存储区。 之所以如此是因为分区为不同的应用程序的用户帐户。 和时每个用户名可得到保证，在应用程序中唯一，可能使用相同的用户存储的不同应用程序中使用相同的用户名。 没有复合`UNIQUE`中的约束`aspnet_Users`表上`UserName`和`ApplicationId`字段，但不是上一仅`UserName`字段。 因此，很可能 aspnet\_用户表中包含具有相同的两个 （或多个） 记录`UserName`值。 不存在，但是，`UNIQUE`约束`aspnet_Users`表的`UserId`字段 （因为它是主键）。 A`UNIQUE`约束很重要，因为没有它，我们无法建立之间的外键约束`GuestbookComments`和`aspnet_Users`表。


在添加后`UserId`列中，保存对表进行单击工具栏中的保存图标。 命名新表`GuestbookComments`。

我们有一个与处理的最后一个问题`GuestbookComments`表： 我们需要创建[外键约束](https://msdn.microsoft.com/library/ms175464.aspx)之间`GuestbookComments.UserId`列和`aspnet_Users.UserId`列。 若要实现此目的，请单击以启动外键关系对话框中的工具栏中的关系图标。 （或者，你可以启动此对话框中通过转到表设计器菜单并选择关系。）

单击外键关系对话框的左下角中的添加按钮。 这将添加一个新的外键约束，但我们仍需在关系中定义参与的表。


[![使用外键关系对话框中管理表的外键约束](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**图 3**： 外键关系对话框用于管理表的 Foreign Key Constraints ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image9.png))


接下来，单击右侧的"表和列规范"行中的省略号图标。 这将启动表和列对话框中，我们可以在其中指定主键表和列和外的键列从`GuestbookComments`表。 具体而言，选择`aspnet_Users`和`UserId`作为主键表和列和`UserId`从`GuestbookComments`作为外键列的表 （请参见图 4）。 在定义的主键和外键表和列之后, 单击确定以返回到外键关系对话框中。


[![建立的外键约束之间 aspnet_Users 和 GuesbookComments 表](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**图 4**: 外键约束之间建立`aspnet_Users`和`GuesbookComments`表 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image12.png))


此时已建立的外键约束。 此约束是否存在可确保[关系完整性](http://en.wikipedia.org/wiki/Referential_integrity)通过，永远不会将存在来宾簿项引用不存在用户帐户，从而保证两个表之间。 默认情况下外, 键约束将禁止如果那里相应的子记录要删除的父记录。 也就是说，如果用户使一个或多个来宾簿注释，然后我们尝试删除该用户帐户，删除将失败，除非他来宾簿注释会最先删除。

外键约束可以配置为删除父记录时自动删除关联的子记录。 换而言之，我们可以设置该外键约束，以便删除其用户帐户时，会自动删除用户的来宾簿条目。 若要实现此目的，展开"插入和更新规范"部分，并将"删除规则"属性设置为级联。


[![配置到级联删除的外键约束](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**图 5**： 配置到级联删除外键约束 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image15.png))


若要保存的外键约束，请单击关闭按钮退出外键关系。 然后单击保存表，这工具栏中的保存图标关系。

### <a name="storing-the-users-home-town-homepage-and-signature"></a>存储用户的主页镇、 主页上和签名

`GuestbookComments`表说明了如何将存储与用户帐户共享同一个对多关系的信息。 由于每个用户帐户可以有任意数目的关联的注释，此关系可建模通过创建一个表以保存的一套注释包含列链接到特定用户返回每个注释。 使用时`SqlMembershipProvider`，通过创建一个名为列最建立这种链接`UserId`类型的`uniqueidentifier`和此列之间的外键约束和`aspnet_Users.UserId`。

现在，我们需要将三个列与每个用户帐户来存储用户的主镇、 主页上和签名，它将显示在自己的来宾簿建议相关联。 有多种不同方法来实现此目的：

- **添加到新列 * * *`aspnet_Users`* * * 或 * * *`aspnet_Membership`* * * 表。** 我将不推荐这种方法，因为它修改所用架构`SqlMembershipProvider`。 此决策可能回来下道路言词你。 例如，如果 ASP.NET 的未来版本使用不同时`SqlMembershipProvider`架构。 Microsoft 可能包括一个工具，可迁移 ASP.NET 2.0`SqlMembershipProvider`数据复制到新架构，但如果你已修改 ASP.NET 2.0`SqlMembershipProvider`架构，此类转换可能不可行。

- **使用 ASP。NET 的配置文件 framework 定义的家乡、 主页上和签名的配置文件属性。** ASP.NET 包括一个配置文件框架，用于存储其他特定于用户的数据。 成员资格 framework 中，如配置文件 framework 是在提供程序模型之上构建的。 .NET Framework 附带`SqlProfileProvider`成功在 SQL Server 数据库中存储配置文件数据。 事实上，我们的数据库已使用的表`SqlProfileProvider`(`aspnet_Profile`)，如我们添加后的应用程序服务时添加<a id="_msoanchor_2"> </a> [*在 SQL 中创建成员身份架构服务器*](creating-the-membership-schema-in-sql-server-cs.md)教程。   
 配置文件 framework 的主要优势是它允许开发人员定义中的配置文件属性`Web.config`– 任何代码需要要写入序列化配置文件数据传入和传出基础数据存储区。 简单地说，它是极其简便的方式来定义一组配置文件属性并在代码中使用它们。 但是，配置文件系统会留下很多需要改进时涉及到版本控制，因此，如果你有其中希望新的特定于用户的属性，以添加在稍后的时间或现有的要删除或修改，则可能不到配置文件 framework 应用程序 最佳选项。 此外，`SqlProfileProvider`高度非规范化的方式，使其下一步，几乎不可能直接对配置文件数据 （例如，多少用户拥有 New York 主镇） 运行查询存储的配置文件属性。   
 在配置文件架构的详细信息，请在本教程末尾查阅"进一步读数"部分。

- **将这三列添加到数据库中的新表并建立此表之间的一对一关系和 * * *`aspnet_Users`* * *。** 此方法涉及到多做一些工作比使用配置文件框架中，但提供的其他用户属性数据库中的建模方式的最大灵活性。 这是我们将在本教程中使用的选项。

我们将创建名为的新表`UserProfiles`以保存家乡、 主页上，并为每个用户的签名。 右键单击数据库资源管理器窗口中的表文件夹并选择创建新表。 名称的第一列`UserId`并将其类型设置为`uniqueidentifier`。 禁止`NULL`值并将标记为主键列。 接下来，添加名为的列：`HomeTown`类型的`nvarchar(50)`;`HomepageUrl`类型的`nvarchar(100)`; 和类型的签名`nvarchar(500)`。 这三列的每个可接受`NULL`值。


[![创建 UserProfiles 表](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**图 6**： 创建`UserProfiles`表 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image18.png))


保存该表并将其命名`UserProfiles`。 最后，建立之间的外键约束`UserProfiles`表的`UserId`字段和`aspnet_Users.UserId`字段。 正如我们做具有之间的外键约束`GuestbookComments`和`aspnet_Users`的表，具有级联删除此约束。 由于`UserId`字段`UserProfiles`是主密钥，这可确保将中的多个记录`UserProfiles`每个用户帐户的表。 引用此类型的关系为一对一。

现在，我们已创建的数据模型，我们就可以使用它。 步骤 2 和 3 中我们将查看如何当前登录的用户可以查看和编辑其主镇、 主页上和签名信息。 在步骤 4 中，我们将创建经过身份验证的用户可以提交新注释转换为来宾簿并查看现有的接口。

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>步骤 2： 显示用户的主页镇、 主页上和签名

有各种方法来允许当前登录的用户查看和编辑其主镇、 主页上和签名信息。 我们无法使用手动创建用户界面文本框和标签控件，或者我们可以使用一个 Web 控件，如说明如何控制的数据。 若要执行数据库`SELECT`和`UPDATE`我们可以编写 ADO.NET 的语句中我们的页面的代码隐藏类的代码，或者，或者，使用声明性方法使用 SqlDataSource。 理想情况下我们的应用程序将包含分层体系结构，我们无法调用以编程方式从页面的代码隐藏类或以声明方式通过 ObjectDataSource 控件。

由于本系列教程着重于窗体身份验证、 授权、 用户帐户和角色，不会详细讨论了这些不同的数据访问选项或为什么分层体系结构的效果好于直接执行 SQL 语句从 ASP.NET 页中。 我要使用说明和 SqlDataSource – 最快和最简单选项 – 指导，但所述的概念可以肯定是可应用于备用的 Web 控件和数据访问逻辑。 有关使用 ASP.NET 中的数据的详细信息，请参阅我*[在 ASP.NET 2.0 中使用数据](../../data-access/index.md)*教程系列。

打开`AdditionalUserInfo.aspx`页面`Membership`文件夹并将说明如何控件添加到页上，设置其`ID`属性`UserProfile`和清除其`Width`和`Height`属性。 展开说明的智能标记，并选择要绑定到一个新的数据源控件。 这将启动数据源配置向导 （请参阅图 7）。 第一步会要求您指定的数据源类型。 由于我们将直接连接到`SecurityTutorials`数据库中，选择数据库图标指定`ID`作为`UserProfileDataSource`。


[![添加一个名为 UserProfileDataSource 的新 SqlDataSource 控件](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**图 7**： 添加新的 SqlDataSource 控件命名`UserProfileDataSource`([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image21.png))


下一个屏幕会提示输入要使用的数据库。 我们已定义中的连接字符串`Web.config`为`SecurityTutorials`数据库。 此连接字符串名称 – `SecurityTutorialsConnectionString` – 应会出现在下拉列表。 选择此选项，然后单击下一步。


[![从下拉列表中选择 SecurityTutorialsConnectionString](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**图 8**： 选择`SecurityTutorialsConnectionString`从下拉列表 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image24.png))


后续屏幕要求我们需要指定的表和查询的列。 选择`UserProfiles`表从下拉列表，然后检查所有列。


[![将从 UserProfiles 表返回的所有列](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**图 9**： 使返回的所有列从`UserProfiles`表 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image27.png))


图 9 返回中的当前查询*所有*中记录的`UserProfiles`，但我们只是想在当前登录的用户记录中。 若要添加`WHERE`子句中，单击`WHERE`按钮以打开添加`WHERE`子句对话框中 （请参阅图 10）。 在此处你可以选择要作为筛选依据的列、 运算符和筛选器参数的源。 选择`UserId`作为列并选择"="为运算符。

遗憾的是返回当前登录的用户的任何内置参数源`UserId`值。 我们将需要以编程方式获取此值。 因此，设置为"None，"单击添加按钮以添加参数，然后单击确定的源下拉列表。


[![上 UserId 列添加筛选器参数](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**图 10**： 上添加筛选器参数`UserId`列 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image30.png))


单击确定后你将返回到图 9 所示的屏幕。 此时间，但是，在屏幕底部的 SQL 查询应包括`WHERE`子句。 单击下一步以转到"测试查询"屏幕。 此处你可以运行查询并查看结果。 单击完成以完成向导。

完成数据源配置向导之后，Visual Studio 创建 SqlDataSource 控件基于向导中指定的设置。 此外，它将 BoundFields 手动添加到每个列由 SqlDataSource 的说明`SelectCommand`。 无需以显示`UserId`字段中的说明，因为用户不需要知道此值。 你可以直接从说明如何控制的声明性标记中删除此字段，或单击"编辑字段"链接从其智能标记。

此时页面的声明性标记应类似以下：

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

我们需要以编程方式设置 SqlDataSource 控件`UserId`参数对当前登录用户的`UserId`选择数据之前。 这可以通过为 SqlDataSource 创建事件处理程序来实现`Selecting`事件和添加以下代码那里：

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

上面的代码首先通过调用获取对当前登录用户的引用`Membership`类的`GetUser`方法。 这将返回`MembershipUser`对象，其`ProviderUserKey`属性包含`UserId`。 `UserId`值之后将赋给 SqlDataSource`@UserId`参数。

> [!NOTE]
> `Membership.GetUser()`方法返回有关当前登录用户的信息。 如果匿名用户访问的页面，则会返回值为`null`。 在这种情况下，这将导致`NullReferenceException`代码时尝试读取下一行`ProviderUserKey`属性。 当然，我们无需担心`Membership.GetUser()`返回`null`中的值`AdditionalUserInfo.aspx`页，因为我们在前面的教程中配置 URL 授权，以便只有经过身份验证的用户无法访问此文件夹中的 ASP.NET 资源。 如果你需要访问有关在页中允许进行匿名访问当前登录用户的信息，请务必检查非`null MembershipUser`从返回的对象`GetUser()`之前引用其属性的方法。


如果你访问`AdditionalUserInfo.aspx`页通过浏览器您将看到一个空白页，因为我们尚未添加到任何行`UserProfiles`表。 步骤 6 中我们将了解如何自定义通过自动添加到新行`UserProfiles`表时创建一个新的用户帐户。 现在，但是，我们将需要手动创建一条记录表中。

导航到 Visual Studio 中的数据库资源管理器并展开表文件夹。 右键单击`aspnet_Users`表，并且选择"显示表数据"以查看表中的记录，执行相同的操作`UserProfiles`表。 图 11 显示这些结果时纵向平铺方式。 在我的数据库中当前有`aspnet_Users`记录罗斯向她、 Fred 和 Tito，但在没有记录`UserProfiles`表。


[![显示的内容 aspnet_Users 和 UserProfiles 表](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**图 11**: 内容`aspnet_Users`和`UserProfiles`显示表 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image33.png))


添加到新的记录`UserProfiles`通过手动输入的值中的表`HomeTown`， `HomepageUrl`，和`Signature`字段。 若要获取有效的最简单方法`UserId`中的新值`UserProfiles`记录是选择`UserId`字段中的特定用户帐户从`aspnet_Users`表和复制并将其粘贴到`UserId`字段`UserProfiles`。 图 12 显示`UserProfiles`表后已为罗斯向她添加一条新记录。


[![到 UserProfiles 罗斯向她为添加一条记录](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**图 12**: A 记录已添加到`UserProfiles`的罗斯向她 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image36.png))


返回到`AdditionalUserInfo.aspx`页上，在中记录为罗斯向她。 如图 13 所示，将显示罗斯向她的设置。


[![当前访问用户是显示 His 设置](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**图 13**: 当前访问用户是显示 His 设置 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> 请转到继续操作并手动添加中的记录`UserProfiles`每个成员资格用户的表。 步骤 6 中我们将了解如何自定义通过自动添加到新行`UserProfiles`表时创建一个新的用户帐户。


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>步骤 3： 允许用户编辑其主页镇、 主页上和签名

此时，当前登录的用户可以查看其主镇、 主页上，和签名设置，但不是能尚未修改它们。 让我们更新说明，以便可以编辑数据。

我们需要做的第一件事是添加`UpdateCommand`SqlDataSource，用于指定`UPDATE`要执行的语句和其相应的参数。 选择 SqlDataSource，然后从属性窗口中，单击要打开命令和参数编辑器对话框中的 UpdateQuery 属性旁边的省略号。 输入以下`UPDATE`在文本框中的语句：

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

接下来，单击"刷新参数"按钮，将创建 SqlDataSource 控件中的参数`UpdateParameters`集合中的参数的每个`UPDATE`语句。 为无保留所有的参数集的源，然后单击确定按钮以完成对话框。


[![指定 SqlDataSource UpdateCommand 和 UpdateParameters](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**图 14**： 指定 SqlDataSource`UpdateCommand`和`UpdateParameters`([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image42.png))


由于添加我们进行到 SqlDataSource 控件中，控件现在可以支持编辑说明。 从说明的智能标记，请检查"启用编辑"复选框。 这将添加到控件的 CommandField`Fields`具有集合其`ShowEditButton`属性设置为 True。 说明如何显示只读模式和更新中和取消按钮时显示在编辑模式时，这会呈现一个编辑按钮。 而不是需要用户单击编辑，不过，我们可以说明如何呈现"始终可编辑"状态中通过设置说明如何控制[`DefaultMode`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx)到`Edit`。

进行这些更改后，说明如何控制的声明性标记应类似于以下：

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

请注意 CommandField 添加和`DefaultMode`属性。

继续操作并测试通过浏览器的此页。 在使用具有相应的记录中的用户访问时`UserProfiles`，可编辑界面中显示的用户的设置。


[![说明如何呈现的可编辑的接口](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**图 15**: 说明如何呈现可编辑的接口 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image45.png))


请尝试更改这些值并单击更新按钮。 似乎像会执行任何操作。 没有回发和值保存到数据库，但没有任何保存发生的视觉反馈。

若要解决此问题，返回到 Visual Studio，并添加上面说明的标签控件。 设置其`ID`到`SettingsUpdatedMessage`，将其`Text`属性设置为"你的设置已更新，"并将其`Visible`和`EnableViewState`属性设置为`false`。

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

我们需要显示`SettingsUpdatedMessage`标签每当更新说明。 若要完成此操作，可为说明如何创建事件处理程序`ItemUpdated`事件并添加以下代码：

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

返回到`AdditionalUserInfo.aspx`通过浏览器页上，并更新数据。 这次，将显示一条很有帮助的状态消息。


[![一条短消息是显示时设置将会更新](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**图 16**： 更新设置后，将显示短消息 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> 说明如何控制的编辑界面使许多需要改进。 它使用标准尺寸的文本框中，但的签名字段可能应为多行文本框。 RegularExpressionValidator 应该用于确保，主页的 URL，如果输入，以"http://"或"https://"开头。 此外，由于控件具有说明其`DefaultMode`属性设置为`Edit`，取消按钮不执行任何操作。 它应是删除或，单击时，将用户重定向到其他页面上 (如`~/Default.aspx`)。 我将保留这些增强功能为练习为读取器。


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>添加到链接`AdditionalUserInfo.aspx`母版页中的页

目前，网站不提供任何指向`AdditionalUserInfo.aspx`页。 若要访问它的唯一方法是直接在浏览器的地址栏中输入该页面的 URL。 让我们的链接添加到在此页`Site.master`母版页。

回想一下母版页包含 LoginView Web 控件在其`LoginContent`ContentPlaceHolder 为经过身份验证和匿名访客显示不同的标记。 更新 LoginView 控件`LoggedInTemplate`包含链接`AdditionalUserInfo.aspx`页。 LoginView 进行这些更改后应类似于以下控件的声明性标记：

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

请注意添加`lnkUpdateSettings`超链接控件添加到`LoggedInTemplate`。 为包含就地此链接，经过身份验证的用户可以快速跳转到页后，可以查看和修改其主镇、 主页上，以及签名设置。

## <a name="step-4-adding-new-guestbook-comments"></a>步骤 4： 添加新的来宾簿注释

`Guestbook.aspx`页是经过身份验证的用户可以查看来宾簿并发表评论。 让我们开始创建要添加新的来宾簿注释的接口。

打开`Guestbook.aspx`在 Visual Studio 中的页上，并构建用户界面，包含两个文本框控件、 一个针对新注释的使用者，一个用于其正文。 设置第一个文本框控件的`ID`属性`Subject`及其`Columns`为 40 的属性; 设置的秒`ID`到`Body`，将其`TextMode`到`MultiLine`，并将其`Width`和`Rows`属性设置为"95%"和 8，分别。 若要完成用户界面，将添加一个名为的按钮 Web 控件`PostCommentButton`并设置其`Text`属性设置为"Post 您注释"。

由于每个来宾簿注释需要主题和正文，请为每个文本框中添加 RequiredFieldValidator。 设置`ValidationGroup`到"EnterComment"; 这些控件的属性同样，设置`PostCommentButton`控件的`ValidationGroup`"EnterComment"的属性。 有关 ASP 的详细信息。NET 的验证控件，签出[在 ASP.NET 中的窗体验证](http://www.4guysfromrolla.com/webtech/090200-1.shtml)，[将 ASP.NET 2.0 中的验证控件](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)，和[验证服务器控件教程](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp)上[W3Schools](http://www.w3schools.com/)。

编写用户界面后页面的声明性标记看上去应如下所示：

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

与使用完整的用户界面，我们的下一个任务是插入新记录到`GuestbookComments`表时`PostCommentButton`单击。 这可以通过多种方式实现： 我们可以编写的 ADO.NET 代码中的按钮`Click`事件处理程序; 我们可以将 SqlDataSource 控件添加到页，配置其`InsertCommand`，然后调用其`Insert`方法从`Click`事件处理程序;或者，我们可以生成负责插入新的来宾簿注释，中间层，调用此功能从`Click`事件处理程序。 由于我们查看在步骤 3 中使用 SqlDataSource，让我们此处使用的 ADO.NET 代码。

> [!NOTE]
> 用于以编程方式从 Microsoft SQL Server 数据库中访问数据的 ADO.NET 类位于`System.Data.SqlClient`命名空间。 你可能需要将此命名空间导入页的代码隐藏类 (即， `using System.Data.SqlClient;`)。


创建的事件处理程序`PostCommentButton`的`Click`事件并添加以下代码：

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

`Click`事件处理程序启动： 检查用户提供的数据是否有效。 如果不是这样，将记录插入之前退出事件处理程序。 假定提供的数据是否有效，当前登录的用户的`UserId`检索值并将其存储在`currentUserId`本地变量。 此值必须的因为我们必须提供`UserId`值插入到的记录时`GuestbookComments`。

接下来，该连接字符串为`SecurityTutorials`数据库检索从`Web.config`和`INSERT`未指定 SQL 语句。 A`SqlConnection`对象然后创建并打开。 接下来，`SqlCommand`对象构造和使用的参数值`INSERT`分配查询。 `INSERT`然后执行语句，连接关闭。 在事件处理程序，末尾`Subject`和`Body`文本框中的`Text`属性会清除，以便用户的值不会贯穿回发。

继续操作并测试浏览器中的此页。 由于此页为`Membership`文件夹不能由匿名访问者访问。 因此，你将需要第一次登录 （如果还没有）。 输入一个值`Subject`和`Body`文本框中，单击`PostCommentButton`按钮。 这将导致一条新记录添加到`GuestbookComments`。 在回发时，从文本框中擦除的主题和你提供的正文。

单击后`PostCommentButton`存在按钮是没有可视反馈注释已添加到来宾簿。 我们仍需要更新此页后，可以显示现有来宾簿注释，我们将在步骤 5 中执行此操作。 后我们完成此操作，只添加注释将显示在列表中的注释，提供足够的视觉反馈。 现在，确认你来宾簿注释已保存的检查的内容`GuestbookComments`表。

图 17 显示的内容`GuestbookComments`表后处于两个注释。


[![你可以看到 GuestbookComments 表中的来宾簿注释](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**图 17**： 你可以看到中的来宾簿注释`GuestbookComments`表 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> 如果用户尝试插入可能包含来宾簿注释危险标记 – 如 HTML – ASP.NET 将引发`HttpRequestValidationException`。 若要了解有关此异常时，引发原因，以及如何允许用户提交潜在危险值的详细信息，请查阅[请求验证白皮书](../../../../whitepapers/request-validation.md)。


## <a name="step-5-listing-the-existing-guestbook-comments"></a>步骤 5： 列出现有来宾簿注释

除了使用户访问的注释，`Guestbook.aspx`页还应能够查看来宾簿的现有注释。 若要完成此操作，将添加一个名为的 ListView 控件`CommentList`到页面底部。

> [!NOTE]
> ListView 控件是为 ASP.NET 3.5 版新增功能。 它旨在要在非常可自定义和灵活布局中，显示的项的列表，但仍会提供内置编辑、 插入、 删除、 分页和排序功能，例如 GridView。 如果你在使用 ASP.NET 2.0，你将需要改为使用的 DataList 或转发器控件。 使用列表视图的详细信息，请参阅[Scott Guthrie](https://weblogs.asp.net/scottgu/)的博客文章[asp: ListView 控件](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)，和我文章[与 ListView 控件中显示数据](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)。


打开 ListView 的智能标记，并从选择数据源下拉列表，请将控件绑定到新的数据源。 正如我们在步骤 2 中看到的这将启动数据源配置向导。 选择数据库图标，将生成 SqlDataSource `CommentsDataSource`，单击确定。 接下来，选择`SecurityTutorialsConnectionString`连接字符串从下拉列表并单击下一步。

此时在步骤 2 中我们指定查询的数据选择`UserProfiles`表从下拉列表并选择要返回的列 （回头参考图 9 中）。 这一次，但是，我们想要创建 SQL 语句返回拉取不仅从记录`GuestbookComments`，但还注释的家乡、 主页、 签名和用户名。 因此，选择"指定自定义 SQL 语句或存储的过程"单选按钮，然后单击下一步。

此时会弹出"定义自定义语句或存储过程"屏幕。 单击查询生成器按钮来以图形方式生成查询。 查询生成器启动由提示我们指定我们想要查询的表。 选择`GuestbookComments`， `UserProfiles`，和`aspnet_Users`表并单击确定。 这将添加到设计图面上的所有三个表。 由于有之间的外键约束`GuestbookComments`， `UserProfiles`，和`aspnet_Users`表，查询生成器自动`JOIN`s 这些表。

所有这些剩下是以指定要返回的列。 从`GuestbookComments`表选择`Subject`， `Body`，和`CommentDate`列; 返回`HomeTown`， `HomepageUrl`，和`Signature`中的列`UserProfiles`表; 并返回`UserName`从`aspnet_Users`. 此外，将添加"`ORDER BY CommentDate DESC`"到末尾`SELECT`查询，以便首先返回最新的文章。 进行这些选择后, 查询生成器接口应类似于的屏幕快照图 18 中。


[![将 Constructed 的查询联接 GuestbookComments、 UserProfiles 和 aspnet_Users 表](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**图 18**： 构造查询`JOIN`s `GuestbookComments`， `UserProfiles`，和`aspnet_Users`表 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image54.png))


单击确定以关闭查询生成器窗口并返回到"定义自定义语句或存储过程"屏幕。 单击旁边前进到"测试查询"屏幕中，你可以通过单击测试查询按钮查看查询结果。 当准备好后时，单击完成以完成配置数据源向导。

当我们完成步骤 2，关联的说明如何控件中的配置数据源向导`Fields`集合已更新为包括为返回的每个列 BoundField `SelectCommand`。 ListView，但是，保持不变;我们仍需要定义其布局。 通过其声明性的标记，或从其智能标记中的"配置 ListView"选项，可手动构造 ListView 的布局。 我通常更喜欢手动，定义标记，但使用的任何方法将极其自然地你。

我结束使用以下`LayoutTemplate`， `ItemTemplate`，和`ItemSeparatorTemplate`我 ListView 控件：

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

`LayoutTemplate`定义标记发出的控件，而`ItemTemplate`呈现 SqlDataSource 所返回每个项。 `ItemTemplate`的生成的标记放置在`LayoutTemplate`的`itemPlaceholder`控件。 除了`itemPlaceholder`、`LayoutTemplate`包括 DataPager 控件，这可以限制到显示每页 （默认值） 的只是 10 来宾簿注释的 ListView 并呈现分页接口。

我`ItemTemplate`显示中的每个来宾簿注释的使用者`<h4>`位于以下使用者将正文的元素。 请注意用于显示正文的语法采用返回的数据`Eval("Body")`数据绑定语句，将其转换为字符串，并替换其中的换行符与`<br />`元素。 为了显示换行符，输入提交注释，因为空白将被忽略的 HTML 时需要此转换。 下面将以斜体显示，用户的主镇，其主页、 日期和进行注释，时间和保留注释的人员的用户名的链接后跟正文显示用户的签名。

花些时间查看通过浏览器的页。 你应看到添加到此处显示的步骤 5 中来宾簿的注释。


[![Guestbook.aspx 现在显示来宾簿的注释](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**图 19**:`Guestbook.aspx`现在显示来宾簿的注释 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image57.png))


请尝试向来宾簿添加新的注释。 如果单击`PostCommentButton`按钮页回发和注释添加到数据库，但不是更新 ListView 控件以显示新的注释。 可以通过以下任一方法解决此问题：

- 更新`PostCommentButton`按钮的`Click`事件处理程序，使它时，将调用 ListView 控件`DataBind()`方法之后将新的注释插入到数据库，或
- 设置 ListView 控件`EnableViewState`属性`false`。 因为通过禁用控件的视图状态，它必须重新绑定到基础数据在每个回发时，此方法适用。

教程的网站可下载从本教程说明了这两种技术。 ListView 控件`EnableViewState`属性`false`并以编程方式重新绑定为 ListView 的数据所需的代码存在于`Click`事件处理程序，但已被注释掉。

> [!NOTE]
> 当前`AdditionalUserInfo.aspx`页允许用户查看和编辑其主镇、 主页上，以及签名设置。 它会更好更新`AdditionalUserInfo.aspx`以显示已登录用户的来宾簿注释中。 也就是说，除了检查和修改她的信息，用户可以访问`AdditionalUserInfo.aspx`页面以查看哪些来宾簿注释她在过去进行。 我将此作为练习感读取器。


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>步骤 6： 自定义通过包括用于主页镇、 主页上和签名的接口

`SELECT`使用的查询`Guestbook.aspx`页上使用`INNER JOIN`要合并的之间相关的记录`GuestbookComments`， `UserProfiles`，和`aspnet_Users`表。 如果在具有任何记录的用户`UserProfiles`发出来宾簿注释，注释将不能显示在列表视图，因为`INNER JOIN`仅返回`GuestbookComments`记录中的匹配记录时`UserProfiles`和`aspnet_Users`。 正如我们看到在步骤 3 中，如果用户没有记录和`UserProfiles`她无法查看或编辑在她设置`AdditionalUserInfo.aspx`页。

当然，由于我们设计的决策很重要的成员资格系统中每个用户帐户具有匹配记录中`UserProfiles`表。 我们想要为相应的记录添加到`UserProfiles`每当通过 CreateUserWizard 创建一个新的成员资格用户帐户。

中所述[*创建用户帐户*](creating-user-accounts-cs.md)教程中，通过创建新的成员资格用户帐户后引发其[`CreatedUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). 我们可以创建此事件的事件处理，对于刚创建的用户，获取用户 Id，然后插入到记录`UserProfiles`表使用的默认值`HomeTown`， `HomepageUrl`，和`Signature`列。 更重要的是，就可以通过自定义通过的接口，以包括其他文本框中提示用户输入这些值。

让我们首先看一下如何向其中添加新行`UserProfiles`表中`CreatedUser`事件处理程序替换默认值。 接下来，我们将了解如何自定义通过用户界面，若要包含其他窗体字段以收集新用户的主镇、 主页上和签名。

### <a name="adding-a-default-row-touserprofiles"></a>添加到默认行`UserProfiles`

在[*创建用户帐户*](creating-user-accounts-cs.md)教程，我们添加到通过`CreatingUserAccounts.aspx`页面`Membership`文件夹。 为了具有 CreateUserWizard 控件添加到记录`UserProfiles`表用户帐户创建后，我们需要更新通过的功能。 而不是进行这些更改到`CreatingUserAccounts.aspx`页上，让我们改为添加到新通过`EnhancedCreateUserWizard.aspx`页上，并那里本教程进行的修改。

打开`EnhancedCreateUserWizard.aspx`页在 Visual Studio 中，将从工具箱拖到页中通过。 设置通过`ID`属性`NewUserWizard`。 如我们所述<a id="_msoanchor_5"> </a> [*创建用户帐户*](creating-user-accounts-cs.md)教程，CreateUserWizard 的默认用户界面将提示所需的信息的访问者。 一旦已提供此信息，该控件内部创建新的用户帐户在成员资格 framework 中，所有操作我们无需编写一行代码。

通过其工作流期间引发事件的数。 通过访问者提供了请求的信息并提交该表单后，最初激发其[`CreatingUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)。 如果在创建过程中，问题[`CreateUserError`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)激发; 但是，如果已成功创建用户，则[`CreatedUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)引发。 在<a id="_msoanchor_6"> </a> [*创建用户帐户*](creating-user-accounts-cs.md)我们创建的事件处理程序的教程`CreatingUser`事件，以确保提供的用户名不包含任何前导或尾随空格，而且用户名未不显示任何位置在密码。

为了添加中的行`UserProfiles`表对于刚创建的用户，我们需要创建的事件处理程序`CreatedUser`事件。 按时间`CreatedUser`激发事件，使我们能够检索帐户的用户 Id 值的成员资格 framework 已创建的用户帐户。

创建的事件处理程序`NewUserWizard`的`CreatedUser`事件并添加以下代码：

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

上面的代码朋友通过检索刚添加用户帐户的用户 Id。 这通过使用实现`Membership.GetUser(username)`方法以返回有关特定用户，然后使用信息`ProviderUserKey`要检索其用户 Id 属性。 用户在通过输入的用户名是可通过其[`UserName`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)。

接下来，连接字符串检索从`Web.config`和`INSERT`未指定语句。 实例化必要的 ADO.NET 对象和执行此命令。 该代码将分配[ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx)到实例`@HomeTown`， `@HomepageUrl`，和`@Signature`参数，插入数据库的效果`NULL`值`HomeTown`， `HomepageUrl`，和`Signature`字段。

请访问`EnhancedCreateUserWizard.aspx`页上通过浏览器并创建一个新的用户帐户。 之后这样做，返回到 Visual Studio，并检查内容`aspnet_Users`和`UserProfiles`表 （例如，我们进来图 12 所做的那样）。 你应看到中的新用户帐户`aspnet_Users`和相应`UserProfiles`行 (使用`NULL`值`HomeTown`， `HomepageUrl`，和`Signature`)。


[![已添加的新用户帐户和 UserProfiles 记录](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**图 20**: 新的用户帐户和`UserProfiles`已添加记录 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image60.png))


在距访客已提供其新的帐户信息并单击"创建用户"按钮、 创建用户帐户并添加一行后`UserProfiles`表。 CreateUserWizard 然后显示其`CompleteWizardStep`，它会显示一条成功消息和继续按钮。 单击继续按钮会导致回发，但不执行任何操作，而使用户始终显示`EnhancedCreateUserWizard.aspx`页。

我们可以指定要将用户发送到通过通过单击继续按钮时的 URL [ `ContinueDestinationPageUrl`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)。 设置`ContinueDestinationPageUrl`属性设置为"~ / Membership/AdditionalUserInfo.aspx"。 这将使新用户添加到`AdditionalUserInfo.aspx`，其中他们可以查看和更新其设置。

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>自定义提示输入新用户的主页镇、 主页上，以及签名 CreateUserWizard 的接口

通过默认接口足以满足简单的帐户创建方案，需要收集唯一核心用户帐户信息，如用户名、 密码和电子邮件。 但如果想要提示创建她的帐户时输入她家乡、 主页上，和签名者？ 可以自定义通过的界面，以收集其他信息在注册，并且可能在使用此信息`CreatedUser`要基础数据库中插入其他记录的事件处理程序。

通过扩展 ASP.NET 向导控件，即允许页开发人员可以定义一系列的有序的控件， `WizardSteps`。 向导控件呈现活动步骤，并提供导航的界面，允许访问者将通过这些步骤。 向导控件非常适合用于长任务拆分成几个简单的步骤。 在向导控件上的详细信息，请参阅[使用 ASP.NET 2.0 向导控件创建分步用户界面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)。

通过的默认标记定义了两个`WizardSteps`:`CreateUserWizardStep`和`CompleteWizardStep`。

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

第一个`WizardStep`， `CreateUserWizardStep`，呈现提示输入用户名、 密码、 电子邮件和等等的界面。 她在距访客提供此信息并单击"创建用户"后，将显示`CompleteWizardStep`，它显示成功消息和继续按钮。

若要自定义通过的界面，若要包含其他窗体字段，我们可以：

- **创建一个或多个新 * * *`WizardStep`* * * s 以包含附加用户界面元素**。 若要添加新`WizardStep`CreateUserWizard，请单击"添加/删除`WizardSteps`"从其智能标记上以启动链接`WizardStep`集合编辑器。 从这里你可以添加、 删除或重新排序向导中的步骤。 这是我们将使用本教程中的方法。

- **将转换 * * *`CreateUserWizardStep`* * * 到可编辑 * * *`WizardStep`* * *。** 这将替换`CreateUserWizardStep`为等效`WizardStep`其标记定义匹配的用户界面`CreateUserWizardStep`s。 通过将转换`CreateUserWizardStep`到`WizardStep`我们可以重新定位控件，或将附加用户界面元素添加到此步骤。 要转换`CreateUserWizardStep`或`CompleteWizardStep`到可编辑`WizardStep`、 单击"自定义创建用户步骤"或"自定义完成步骤"链接从控件的智能标记。

- **使用上面的两个选项中的某种组合。**

要记住的一个重要的是通过执行其用户帐户创建过程，在单击"创建用户"按钮时其`CreateUserWizardStep`。 如果有其他并不重要`WizardStep`后的 s`CreateUserWizardStep`与否。

添加自定义时`WizardStep`到通过来收集额外的用户输入，自定义`WizardStep`之前或之后可以放置`CreateUserWizardStep`。 如果前面`CreateUserWizardStep`其他用户输入从自定义的收集然后`WizardStep`适用于`CreatedUser`事件处理程序。 但是，如果自定义`WizardStep`之后`CreateUserWizardStep`然后由时间自定义`WizardStep`显示已创建新的用户帐户和`CreatedUser`已触发事件。

图 21 显示了工作流时添加`WizardStep`之前`CreateUserWizardStep`。 由于已按时间收集其他用户信息`CreatedUser`事件激发，我们需要做是更新`CreatedUser`事件处理程序来检索这些输入并将其用于`INSERT`语句的参数值 （而不是`DBNull.Value`).


[![时其他 WizardStep 之前 CreateUserWizardStep CreateUserWizard 工作流](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**图 21**: CreateUserWizard 工作流时其他`WizardStep`Precedes `CreateUserWizardStep` ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image63.png))


如果自定义`WizardStep`放置*后* `CreateUserWizardStep`，但是，在创建用户帐户过程发生在用户有机会输入她家乡、 主页上或签名之前。 在这种情况下，需要按图 22 说明插入到数据库后创建的用户帐户后，此其他信息。


[![时其他 WizardStep 之后 CreateUserWizardStep CreateUserWizard 工作流](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**图 22**: CreateUserWizard 工作流时其他`WizardStep`附带后`CreateUserWizardStep`([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image66.png))


图 22 中所示的工作流等待插入到记录`UserProfiles`完成步骤 2 后表格中，直到。 如果访问者在步骤 1 后关闭其浏览器，但是，我们将已达到其中用户帐户已创建，但没有记录已添加到的状态`UserProfiles`。 一个解决方法是使用记录`NULL`或默认值插入到`UserProfiles`中`CreatedUser`事件处理程序 （它将在步骤 1 后），然后更新此记录后步骤 2 完成。 这样可确保`UserProfiles`记录将添加的用户帐户，即使在用户退出注册过程中途。

对于本教程让我们创建一个新`WizardStep`之后将发生这种情况`CreateUserWizardStep`之前`CompleteWizardStep`。 我们首先获取在 WizardStep 放置和配置，然后我们将看一下代码。

从通过的智能标记，选择"添加/删除`WizardStep`s"，它可提供`WizardStep`集合编辑器对话框。 添加新`WizardStep`，设置其`ID`到`UserSettings`，将其`Title`到"你设置"并将其`StepType`到`Step`。 然后将其置于，以便它之后`CreateUserWizardStep`（"注册新帐户的"） 和之前`CompleteWizardStep`（"完成"），在图 23 所示。


[![将新 WizardStep 添加到通过](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**图 23**： 添加新`WizardStep`到通过 ([单击以查看实际尺寸的图像](storing-additional-user-information-cs/_static/image69.png))


单击确定以关闭`WizardStep`集合编辑器对话框。 新`WizardStep`出现由通过的更新的声明性标记：

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

请注意新`<asp:WizardStep>`元素。 我们需要添加要收集新用户的主镇、 主页上，以及签名此处的用户界面。 声明性语法中或通过设计器，你可以输入此内容。 若要使用设计器，请从智能标记上以查看设计器中的步骤中的下拉列表中选择"你设置"步骤。

> [!NOTE]
> 选择通过智能标记的下拉列表的步骤更新通过[`ActiveStepIndex`属性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)，它指定起始步骤的索引。 因此，如果使用此下拉列表来编辑设计器中的"你设置"步骤，请务必将其设置回"登录向上 for 新帐户"，以便当用户首次访问此步骤将显示`EnhancedCreateUserWizard.aspx`页。


创建用户界面内包含名为的三个文本框控件的"你设置"步骤`HomeTown`， `HomepageUrl`，和`Signature`。 构造此接口之后, CreateUserWizard 的声明性标记应如下所示类似：

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

请继续并访问此页面，通过浏览器并创建新的用户帐户，指定主镇、 主页上和签名值。 完成后`CreateUserWizardStep`在成员资格框架中创建用户帐户和`CreatedUser`事件处理程序将运行，这将添加到一个新行`UserProfiles`，但与数据库`NULL`值`HomeTown`， `HomepageUrl`，和`Signature`. 永远不会使用为家乡、 主页上，以及签名输入的值。 最终结果是与新的用户帐户`UserProfiles`记录其`HomeTown`， `HomepageUrl`，和`Signature`字段尚未指定。

我们需要在采用用户输入的家庭镇、 honepage 和签名值并更新相应的"你设置"步骤之后执行代码`UserProfiles`记录。 每次向导中的步骤之间移动用户控制，向导的[`ActiveStepChanged`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx)激发。 我们可以创建事件处理程序为此事件以及更新`UserProfiles`表时"你设置"步骤已完成。

添加事件处理程序 CreateUserWizard`ActiveStepChanged`事件并添加以下代码：

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

通过判断我们已到达"完成"步骤来启动上面的代码。 由于在"完成"步骤发生时立即在"你设置"步骤后，然后当在距访客达到"完成"步骤，意味着她刚刚完成了"你设置"步骤。

在这种情况下，我们需要以编程方式引用内的将文本框控件`UserSettings WizardStep`。 这通过先使用实现`FindControl`方法以编程方式引用`UserSettings WizardStep`，，然后再次从引用文本框中`WizardStep`。 一旦已引用文本框中，我们已准备好执行`UPDATE`语句。 `UPDATE`语句具有相同数量的参数作为`INSERT`中的语句`CreatedUser`事件处理程序，但我们在此处使用的用户提供的主镇、 主页上，和签名值。

用就地此事件处理程序，请访问`EnhancedCreateUserWizard.aspx`页上通过浏览器并创建新的用户帐户指定主镇、 主页上和签名值。 创建新帐户之后你应重定向到`AdditionalUserInfo.aspx`页上，其中显示刚才输入主镇、 主页上和签名信息。

> [!NOTE]
> 我们的网站当前有两个访问者可以从中创建新的帐户的页：`CreatingUserAccounts.aspx`和`EnhancedCreateUserWizard.aspx`。 网站的站点地图和登录页指向`CreatingUserAccounts.aspx`页上，但`CreatingUserAccounts.aspx`页不提示用户输入其主镇、 主页上和签名信息，并且不会添加到相应的行`UserProfiles`。 因此，更新`CreatingUserAccounts.aspx`页，以便它提供此功能或更新站点地图和登录页后，可以引用`EnhancedCreateUserWizard.aspx`而不是`CreatingUserAccounts.aspx`。 如果你选择后一种方式，请务必更新`Membership`文件夹的`Web.config`文件以便允许匿名用户访问`EnhancedCreateUserWizard.aspx`页。


## <a name="summary"></a>摘要

在本教程中我们介绍了用于与用户帐户的成员资格框架中的数据进行建模的技术。 具体而言，我们介绍了建模与用户帐户，以及共享一对一关系的数据共享一个对多关系的实体。 此外，我们已了解如何这相关的信息可能显示、 插入和更新，与使用 SqlDataSource 控件，而其他一些示例使用的 ADO.NET 代码。

本教程中完成我们看用户帐户。 从下一教程，我们将打开我们注意到角色。 在接下来几个教程，我们将查看角色 framework，请参阅如何创建新角色、 如何将角色分配给用户，如何来确定哪些角色用户所属，以及如何将基于角色的授权。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [访问和更新 ASP.NET 2.0 中的数据](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 向导控件](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [使用 ASP.NET 2.0 向导控件创建分步的用户界面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [创建自定义数据源控件参数](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [自定义通过](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [说明如何控制快速入门](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [使用 ListView 控件显示数据](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [将 ASP.NET 2.0 中的验证控件](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [编辑插入和删除数据](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [在 ASP.NET 中的窗体验证](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [收集自定义用户注册信息](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [ASP.NET 2.0 中的配置文件](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView 控件](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [用户配置文件快速入门](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢...

本教程系列已由许多有用的审阅者评审。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](user-based-authorization-cs.md)
[下一页](creating-the-membership-schema-in-sql-server-vb.md)
