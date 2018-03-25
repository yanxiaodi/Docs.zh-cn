---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: "在 SQL Server (VB) 中创建成员身份架构 |Microsoft 文档"
author: rick-anderson
description: "本教程开始通过检查用于向数据库添加必要的架构，才能使用 SqlMembershipProvider 技术。 以下，我们 wi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 2dadf091c6ae77fdfaf76f4e1bda92fd3e949678
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>在 SQL Server (VB) 中创建成员身份架构
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip)或[下载 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> 本教程开始通过检查用于向数据库添加必要的架构，才能使用 SqlMembershipProvider 技术。 接下来，我们将检查架构中的键的表，并讨论它们的目的和重要性。 本教程结尾一下如何判断成员资格框架应使用的提供程序的 ASP.NET 应用程序。


## <a name="introduction"></a>介绍

前面的两个教程，用于检查使用窗体身份验证标识网站访问者。 窗体身份验证框架轻松开发人员可以将用户登录到网站，并通过使用身份验证票证的网页访问跨记住它们。 `FormsAuthentication`类包含用于生成票证和将其添加到访问者的 cookie 的方法。 `FormsAuthenticationModule`检查所有传入请求，用于具有有效的身份验证票证，创建并关联`GenericPrincipal`和`FormsIdentity`与当前请求的对象。 窗体身份验证是仅用于授予访问者的身份验证票证时日志记录中，然后在后续请求，分析该票证，以确定用户的标识的机制。 Web 应用程序以支持用户帐户，我们仍需要实现用户存储区并添加功能来验证凭据，注册新用户，并与其他用户帐户相关的任务花费更多。

在 ASP.NET 2.0 中之前, 开发人员在实现所有这些用户帐户相关的任务的挂钩。 幸运的是 ASP.NET 团队识别这种不足，并引入 ASP.NET 2.0 的成员资格 framework。 成员资格 framework 是一套.NET Framework 中提供了完成核心用户帐户相关任务的编程接口的类。 此框架生成之上[提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，它允许开发人员插入标准化 API 的自定义的实现。

中所述<a id="Tutorial1"> </a> [*安全性基础知识和 ASP.NET 支持*](../introduction/security-basics-and-asp-net-support-vb.md)教程中，.NET Framework 附带两个内置的成员资格提供程序： [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx)和[ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)。 顾名思义，`SqlMembershipProvider`使用 Microsoft SQL Server 数据库作为用户存储区。 若要在应用程序中使用此提供程序，我们需要告诉哪些数据库以使用作为存储的提供程序。 正如您想像，`SqlMembershipProvider`需要具有某些数据库表、 视图和存储的过程对用户存储数据库。 我们需要将此预期的架构添加到所选数据库。

本教程开始通过检查用于向数据库添加必要的架构，若要使用技术`SqlMembershipProvider`。 接下来，我们将检查架构中的键的表，并讨论它们的目的和重要性。 本教程结尾一下如何判断成员资格框架应使用的提供程序的 ASP.NET 应用程序。

让我们进入正题！

## <a name="step-1-deciding-where-to-place-the-user-store"></a>步骤 1： 决定在何处放置用户存储区

ASP.NET 应用程序的数据通常存储在多个数据库中的表中。 在实现时`SqlMembershipProvider`我们必须决定是否要将成员资格架构放在应用程序数据与相同的数据库或另一个数据库中的数据库架构。

建议的应用程序数据与同一数据库中查找成员资格架构，原因如下：

- **可维护性**其数据封装在一个数据库的应用程序更轻松地理解、 维护和比具有两个单独的数据库的应用程序部署。
- **关系的完整性**通过定位同一数据库中的成员资格相关的表，为应用程序表它是可以建立[外键约束](http://en.wikipedia.org/wiki/Foreign_key)之间中的主键成员资格相关的表和相关的应用程序表。

仅分离到不同的数据库的用户存储和应用程序数据是有意义，如果你有多个应用程序每个使用单独的数据库，但需要共享通用的用户存储。

### <a name="creating-a-database"></a>创建数据库

因为第二个教程不需要数据库时，才具有已我们构建的应用程序。 我们需要一个现在，但是，用户存储区。 让我们创建一个，然后向其中添加所需的架构`SqlMembershipProvider`提供程序 （请参阅步骤 2）。

> [!NOTE]
> 我们将使用在本教程系列整个[Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx)数据库来存储我们的应用程序表和`SqlMembershipProvider`架构。 有两个原因做出此决定是： 首先，由于其成本-免费的 Express Edition 是最 readably 访问版本的 SQL Server 2005;其次，SQL Server 2005 Express Edition 数据库可以放置直接在 web 应用程序的`App_Data`文件夹，使其简单来打包数据库和 web 应用程序一起在一个 ZIP 文件并将它重新部署而无需任何特殊的设置说明或配置选项。 如果你想要遵循使用非-Express Edition 版本的 SQL Server，随意。 几乎相同的步骤。 `SqlMembershipProvider`架构将工作的任何版本的 Microsoft SQL Server 2000 和最多。

从解决方案资源管理器，右键单击`App_Data`文件夹，然后选择添加新项。 (如果看不到`App_Data`文件夹在项目中，右键单击解决方案资源管理器中的项目，选择添加 ASP.NET 文件夹，并选择`App_Data`。)从添加新项对话框中，选择要添加名为的新 SQL 数据库`SecurityTutorials.mdf`。 在本教程中我们将添加`SqlMembershipProvider`架构到此数据库; 在后续教程中我们将创建其他要捕获应用程序数据的表。


[![添加新命名的 App_Data 文件夹 SecurityTutorials.mdf 数据库的 SQL 数据库](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**图 1**： 添加新的 SQL 数据库命名`SecurityTutorials.mdf`数据库到`App_Data`文件夹 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


添加到数据库`App_Data`文件夹会自动包括它在数据库资源管理器视图中。 （在 Visual Studio 的非-速成版版本中，数据库资源管理器称为服务器资源管理器。）转到数据库资源管理器并展开刚-添加`SecurityTutorials`数据库。 如果你未显示在屏幕的数据库资源管理器，转到视图菜单上，选择数据库资源管理器，或按 Ctrl + Alt + S。 如图 2 所示，`SecurityTutorials`数据库为空-它包含任何表、 任何视图和存储的过程。


[![SecurityTutorials 数据库目前是空](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**图 2**:`SecurityTutorials`数据库目前是空 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>步骤 2： 添加`SqlMembershipProvider`到数据库的架构

`SqlMembershipProvider`需要一组特定的表、 视图和存储的过程以安装在用户存储数据库中。 可以使用添加这些必备项的数据库对象[`aspnet_regsql.exe`工具](https://msdn.microsoft.com/library/ms229862.aspx)。 此文件位于`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`文件夹。

> [!NOTE]
> `aspnet_regsql.exe`工具提供命令行功能和图形用户界面。 图形界面是多个用户友好，我们在本教程中将检查。 命令行界面非常有用的添加`SqlMembershipProvider`架构需要可来自动执行，例如，生成脚本或自动测试方案。

`aspnet_regsql.exe`工具用于添加或删除*ASP.NET 应用程序服务*到指定的 SQL Server 数据库。 ASP.NET 应用程序服务包含的架构`SqlMembershipProvider`和`SqlRoleProvider`，以及为其他 ASP.NET 2.0 框架的基于 SQL 的提供程序的架构。 我们需要提供的信息的两个位`aspnet_regsql.exe`工具：

- 我们是否想要添加或删除应用程序服务和
- 从其中添加或删除应用程序服务架构的数据库

在提示输入数据库后，若要使用，`aspnet_regsql.exe`工具会询问一些我们提供的安全凭据连接到数据库，数据库所在的服务器的名称和数据库名称。 如果使用非 Express Edition 的 SQL Server，您应该已经知道此信息，因为它是使用通过 ASP.NET web 页的数据库时，必须提供通过连接字符串的相同信息。 使用中的 SQL Server 2005 Express Edition 数据库时确定的服务器和数据库名称`App_Data`文件夹，但是，会有点更为复杂。

以下部分介绍采用简单的方式，用于指定中的 SQL Server 2005 Express Edition 数据库的服务器和数据库名称`App_Data`文件夹。 如果你不使用 SQL Server 2005 Express Edition 随意直接跳至安装应用程序服务部分。

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>确定服务器和数据库名称中的 SQL Server 2005 Express Edition 数据库`App_Data`文件夹

若要使用`aspnet_regsql.exe`我们需要了解的服务器和数据库名称的工具。 服务器名称是`localhost\InstanceName`。 最有可能， *InstanceName*是`SQLExpress`。 但是，如果你手动安装 SQL Server 2005 Express Edition （即，你不安装它自动安装 Visual Studio 时），则可能选择不同的实例名称。

数据库名称是有点棘手，以确定。 数据库在`App_Data`文件夹通常具有一个包括的数据库名称[全局唯一标识符](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)以及数据库文件的路径。 我们需要确定此数据库名称以添加应用程序服务架构通过`aspnet_regsql.exe`。

确定数据库名称的最简单方法是通过 SQL Server Management Studio 对其进行检查。 SQL Server Management Studio 提供一个图形界面，用于管理 SQL Server 2005 数据库，但不随附 Express Edition 的 SQL Server 2005。 好消息是[可以下载](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)免费 Express Edition 的 SQL Server Management Studio。

> [!NOTE]
> 如果你还可以安装在你的桌面上，则可能安装 Management Studio 的完整版本的 SQL Server 2005 的非-Express Edition 版本。 完整版本可用于确定数据库名称，按如下所述的 Express 版本执行相同的步骤。

通过关闭 Visual Studio 以确保任何锁施加的数据库文件上的 Visual Studio 开始，将关闭。 接下来，启动 SQL Server Management Studio 并连接到`localhost\InstanceName`SQL Server 2005 Express edition 数据库。 如前文所述，则很有实例名称是`SQLExpress`。 对于身份验证选项中，选择 Windows 身份验证。


[![连接到 SQL Server 2005 Express Edition 实例](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**图 3**： 连接到 SQL Server 2005 Express Edition 实例 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


连接到 SQL Server 2005 Express Edition 实例后，Management Studio 显示文件夹的数据库、 安全设置、 服务器对象和等等。 如果展开数据库选项卡你将看到`SecurityTutorials.mdf`数据库是*不*我们需要先附加该数据库中的数据库实例的注册。

右键单击数据库文件夹，然后从上下文菜单中选择附加。 这将显示附加数据库对话框。 从这里，单击添加按钮，浏览到`SecurityTutorials.mdf`数据库，并单击确定。 图 4 显示了附加数据库对话框后的`SecurityTutorials.mdf`选择数据库。 图 5 演示 Management Studio 对象资源管理器之后已成功附加该数据库。


[![附加 SecurityTutorials.mdf 数据库](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**图 4**： 附加`SecurityTutorials.mdf`数据库 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![SecurityTutorials.mdf 数据库显示在数据库文件夹](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**图 5**:`SecurityTutorials.mdf`数据库将出现在数据库文件夹 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


如图 5 所示，`SecurityTutorials.mdf`数据库具有而是 abstruse 名称。 让我们将其更改为更容易记忆 （和更方便地键入） 名称。 右键单击数据库，从上下文菜单中，选择重命名，然后将其重命名`SecurityTutorialsDatabase`。 这不会更改文件名，只是数据库的名称用于向 SQL Server 标识自身。


[![将数据库重命名为 SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**图 6**： 重命名为数据库`SecurityTutorialsDatabase`([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


此时我们知道的服务器和数据库名称`SecurityTutorials.mdf`数据库文件：`localhost\InstanceName`和`SecurityTutorialsDatabase`分别。 我们现在已准备好安装应用程序服务通过`aspnet_regsql.exe`工具。

### <a name="installing-the-application-services"></a>安装应用程序服务

若要启动`aspnet_regsql.exe`工具，请转到开始菜单，选择运行。 输入`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe`在文本框中单击确定。 或者，你可以使用 Windows 资源管理器中深化到相应文件夹然后通过双击`aspnet_regsql.exe`文件。 这两种方法将 net 相同的结果。

运行`aspnet_regsql.exe`工具而无需任何命令行参数启动 ASP.NET SQL Server 安装向导的图形用户界面。 该向导可以轻松添加或删除指定的数据库上的 ASP.NET 应用程序服务。 图 7 所示在向导的第一个屏幕描述该工具的目的。


[![使用 ASP.NET SQL Server 安装程序向导使添加成员资格架构](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**图 7**： 使用 ASP.NET SQL Server 安装程序向导使网添加成员资格架构 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


在向导中的第二步要求我们是否我们想要添加的应用程序服务或删除它们。 由于我们想要添加表、 视图和存储的过程所需`SqlMembershipProvider`，选择为应用程序服务选项的配置 SQL Server。 更高版本，如果你想要从数据库中删除此架构，重新运行此向导，但改为选择从现有的数据库选项的删除应用程序服务信息。


[![选择配置 SQL Server 的应用程序服务选项](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**图 8**： 为应用程序服务选项中选择配置 SQL Server ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


第三个步骤提示输入数据库信息： 服务器名称、 身份验证信息和数据库名称。 如果你按照本教程中，并添加了`SecurityTutorials.mdf`数据库到`App_Data`，附加到`localhost\InstanceName`，和重命名到`SecurityTutorialsDatabase`，然后使用以下值：

- 服务器：`localhost\InstanceName`
- Windows 身份验证
- 数据库：`SecurityTutorialsDatabase`


[![输入数据库信息](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**图 9**： 输入数据库信息 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


输入数据库信息后，单击下一步。 最后一步总结了将采取的步骤。 单击下一步以安装应用程序服务并完成以完成向导。

> [!NOTE]
> 如果你使用 Management Studio 将附加数据库以及如何重命名数据库文件，请务必分离数据库并重新打开 Visual Studio 之前关闭 Management Studio。 若要分离`SecurityTutorialsDatabase`数据库，右键单击数据库名称，然后从任务菜单中选择分离。

在向导完成，返回到 Visual Studio，并导航到数据库资源管理器。 展开表文件夹。 你应看到名称以前缀开头的表的一系列`aspnet_`。 同样，可以在视图和存储过程文件夹下找到的各种视图和存储的过程。 这些数据库对象构成了应用程序服务架构。 我们将检查在步骤 3 中的成员资格和角色特定数据库对象。


[![各种表、 视图和存储的过程已添加到数据库](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**图 10**: 各种的表、 视图和存储过程已添加到数据库 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe`工具的图形用户界面安装整个应用程序服务架构。 但在执行时`aspnet_regsql.exe`可以从命令行指定哪些特定的应用程序服务组件，安装 （或移除）。 因此，如果你想要添加只需表，视图，以及存储过程所需`SqlMembershipProvider`和`SqlRoleProvider`提供程序，运行`aspnet_regsql.exe`从命令行。 或者，你可以手动运行相应的 T-SQL 子集创建使用脚本`aspnet_regsql.exe`。 这些脚本位于`WINDIR%\Microsoft.Net\Framework\v2.0.50727\`具有类似名称文件夹`InstallCommon.sql`， `InstallMembership.sql`， `InstallRoles.sql`， `InstallProfile.sql`， `InstallSqlState.sql`，依次类推。

此时，我们已创建所需的数据库对象`SqlMembershipProvider`。 但是，我们仍需要指示它应使用成员资格 framework `SqlMembershipProvider` (而不是，说， `ActiveDirectoryMembershipProvider`) 并且`SqlMembershipProvider`应使用`SecurityTutorials`数据库。 我们将了解如何指定要使用哪些提供程序以及如何自定义在步骤 4 中的所选提供程序的设置。 但首先让我们深入了解一下刚刚创建的数据库对象。

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>步骤 3： 查看架构的核心表

在使用 ASP.NET 应用程序中的成员资格和角色框架，提供程序封装的实现详细信息。 在将来我们将与通过.NET Framework 的这些框架之间的接口的教程`Membership`和`Roles`类。 使用这些高级 Api 时我们不需要考虑自己的低级别的详细信息，如执行哪些查询的或哪些表进行修改`SqlMembershipProvider`和`SqlRoleProvider`。

鉴于此，我们可以放心地使用成员资格和角色框架而无需具有探讨在步骤 2 中创建的数据库架构。 但是，在创建表以存储应用程序数据时我们可能需要创建与用户或角色相关的实体。 它有助于熟悉`SqlMembershipProvider`和`SqlRoleProvider`架构时建立外键约束的应用程序数据的表和在步骤 2 中创建这些表之间。 此外，在某些极少数情况下，我们可能需要与用户交互并直接在数据库级别的角色将存储 (而不是通过`Membership`或`Roles`类)。

### <a name="partitioning-the-user-store-into-applications"></a>分区到应用程序的用户存储区

成员资格和角色框架经过专门设计，这样，可以在许多不同的应用程序之间共享单个用户和角色存储。 ASP.NET 应用程序使用的成员身份或角色框架必须指定要使用哪些应用程序分区。 简单地说，多个 web 应用程序可以使用相同的用户和角色存储区。 图 11 描述了分区到三个应用程序的用户和角色存储： HRSite、 CustomerSite 和 SalesSite。 这些三个 web 应用程序每个有其自己的唯一用户和角色，但它们都以物理方式将他们的用户帐户和角色信息存储在同一个数据库表。


[![用户帐户可能会被分解跨多个应用程序](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**图 11**： 用户帐户可能是分区跨多个应用程序 ([单击以查看实际尺寸的图像](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


`aspnet_Applications`表是定义这些分区的内容。 此表中的行表示每个应用程序使用数据库来存储用户帐户信息。 `aspnet_Applications`表具有四个列： `ApplicationId`， `ApplicationName`， `LoweredApplicationName`，和`Description`。`ApplicationId` 类型[ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx)和表的主键;`ApplicationName`提供每个应用程序的唯一用户友好名称。

其他成员资格和角色相关的表将链接回`ApplicationId`字段`aspnet_Applications`。 例如，`aspnet_Users`表，其中包含每个用户帐户的记录，具有`ApplicationId`外键字段; 有关 ditto`aspnet_Roles`表。 `ApplicationId`这些表中的字段指定应用程序分区的用户帐户或所属角色。

### <a name="storing-user-account-information"></a>存储用户帐户信息

用户帐户信息存放在两个表：`aspnet_Users`和`aspnet_Membership`。 `aspnet_Users`表中包含保存的基本的用户帐户信息的字段。 三个最相关列有：

- `UserId`
- `UserName`
- `ApplicationId`

`UserId`是的主键 (和类型的`uniqueidentifier`)。 `UserName`类型`nvarchar(256)`以及的密码，使用户的凭据。 (用户的密码存储在`aspnet_Membership`表。)`ApplicationId`链接到特定的应用程序中的用户帐户`aspnet_Applications`。 没有复合[`UNIQUE`约束](https://msdn.microsoft.com/library/ms191166.aspx)上`UserName`和`ApplicationId`列。 这可确保在给定的应用程序中每个用户名是唯一的但它允许对同一`UserName`要在不同的应用程序中使用。

`aspnet_Membership`表包括其他用户帐户信息，如用户的密码、 电子邮件地址、 最后一个登录名日期和时间，以及等。 中的记录之间不存在一对一的对应关系`aspnet_Users`和`aspnet_Membership`表。 通过将确保此关系`UserId`字段`aspnet_Membership`，它用作表的主键。 如`aspnet_Users`表，`aspnet_Membership`包括`ApplicationId`会绑定到特定应用程序分区此信息的字段。

### <a name="securing-passwords"></a>保护密码

密码信息存储在`aspnet_Membership`表。 `SqlMembershipProvider`允许密码存储在数据库中使用以下三个方法之一：

- **清除**-密码存储在数据库作为纯文本。 我强烈建议您不要使用此选项。 如果数据库被泄露的可由黑客则找到后门或不满的员工具有数据库访问的每个单个用户的凭据有用于接管。
- **哈希处理**-密码哈希使用单向哈希算法和随机生成的 salt 值。 此哈希的值 （连同 salt) 存储在数据库中。
- **加密**-在数据库中存储密码的加密的版本。

使用的密码存储方法取决于`SqlMembershipProvider`中指定设置`Web.config`。 我们将着眼于自定义`SqlMembershipProvider`在步骤 4 中的设置。 默认行为是存储密码哈希。

负责存储密码的列是`Password`， `PasswordFormat`，和`PasswordSalt`。 `PasswordFormat`是类型的字段`int`其值指示用于存储密码的技术： 0 清除; 1 表示 Hashed; 2 表示加密。 `PasswordSalt`分配一个随机生成的字符串而不考虑使用; 的密码存储技术值`PasswordSalt`计算密码的哈希值时才使用。 最后，`Password`列包含的实际密码数据，可以是纯文本密码，密码或加密的密码的哈希。

表 1 说明什么这三列可能如下所示的各种存储技术存储的密码 MySecret 时 ！ .

| **存储技术&lt;\_o3a\_p /&gt;** | **Password&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| 清除 | MySecret ！ | 0 | tTnkPlesqissc2y2SMEygA== |
| 哈希处理 | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| 加密 | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**表 1**： 密码相关字段存储密码 MySecret 时的示例值 ！

> [!NOTE]
> 特定的加密或使用的哈希算法`SqlMembershipProvider`由中设置`<machineKey>`元素。 我们讨论了此配置元素的步骤 3 中<a id="Tutorial3"> </a> [*窗体身份验证配置和高级主题*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)教程。

### <a name="storing-roles-and-role-associations"></a>存储的角色和角色关联

该角色框架允许开发人员定义的角色集和指定哪些用户属于哪些角色。 在数据库中通过两个表中捕获此信息：`aspnet_Roles`和`aspnet_UsersInRoles`。 在每个记录`aspnet_Roles`表表示为特定的应用程序的角色。 非常类似`aspnet_Users`表，`aspnet_Roles`表具有与我们讨论相关的三个列：

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId`是的主键 (和类型的`uniqueidentifier`)。 `RoleName` 的类型为 `nvarchar(256)`。 和`ApplicationId`链接到特定的应用程序中的用户帐户`aspnet_Applications`。 没有复合`UNIQUE`约束`RoleName`和`ApplicationId`列，确保在给定的应用程序中每个角色名称是否唯一。

`aspnet_UsersInRoles`表用作用户和角色之间的映射。 只有两个列-`UserId`和`RoleId`-和一起构成复合主键。

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>步骤 4： 指定提供程序和自定义其设置

所有支持提供程序模型-如的成员资格和角色的框架的框架缺少本身的实现详细信息，并改用该责任委派给提供程序类。 成员资格 framework 中，对于`Membership`类定义了 API，用于管理用户帐户，但不直接与任何用户存储区不进行交互。 相反，`Membership`类的方法移交到配置提供程序的请求，我们将使用`SqlMembershipProvider`。 当我们调用中的方法之一`Membership`类，如何的成员资格框架知道委派对的调用`SqlMembershipProvider`？

`Membership`类具有[`Providers`属性](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx)成员资格 framework 包含对所有可供使用的已注册的提供程序类的引用。 每个已注册的提供程序具有关联的名称和类型。 名称提供用户友好的方式来引用中的特定提供程序`Providers`集合，而类型标识提供程序类。 此外，每个已注册的提供程序可能包括配置设置。 成员资格框架的配置设置包括`PasswordFormat`和`requiresUniqueEmail`，此外还有许多其他。 有关使用配置设置的完整列表请参阅表 2 `SqlMembershipProvider`。

`Providers` Web 应用程序的配置设置可以通过指定服务属性的内容。 默认情况下，所有 web 应用程序都具有名为提供程序`AspNetSqlMembershipProvider`类型的`SqlMembershipProvider`。 此默认成员资格提供程序注册中`machine.config`(位于`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

为上面所示，标记[`<membership>`元素](https://msdn.microsoft.com/library/1b9hw62f.aspx)定义成员资格框架时的配置设置[`<providers>`子元素](https://msdn.microsoft.com/library/6d4936ht.aspx)指定的已注册提供程序。 提供程序可能添加或删除使用[ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx)或[ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx)元素; 请改用[ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx)要删除所有当前元素注册的提供程序。 为上面所示，标记`machine.config`添加提供程序名为`AspNetSqlMembershipProvider`类型的`SqlMembershipProvider`。

除了`name`和`type`特性，`<add>`元素包含定义各种配置设置的值的属性。 表 2 列出了可用`SqlMembershipProvider`-特定的配置设置，以及每个说明。

> [!NOTE]
> 在表 2 中记下任何默认值是指中定义的默认值`SqlMembershipProvider`类。 请注意该 not 中的配置设置的所有`AspNetSqlMembershipProvider`对应的默认值`SqlMembershipProvider`类。 例如，如果未指定成员资格提供程序中,`requiresUniqueEmail`将默认值设置为 true。 但是，`AspNetSqlMembershipProvider`通过显式指定的值来重写此默认值`false`。

| **设置&lt;\_o3a\_p /&gt;** | **Description&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | 回想一下，该成员身份框架允许在单个用户存储分区跨多个应用程序。 此设置指示所使用的成员资格提供程序的应用程序分区的名称。 如果此值不显式指定时，其设置为，在运行时，应用程序的虚拟根路径的值。 |
| `commandTimeout` | 指定的 SQL 命令超时值 （以秒为单位）。 默认值为 30。 |
| `connectionStringName` | 中的连接字符串的名称`<connectionStrings>`元素要用于连接到用户存储区数据库。 此值是必需的。 |
| `description` | 提供已注册的提供程序的用户友好说明。 |
| `enablePasswordRetrieval` | 指定用户是否可能检索其忘记了的密码。 默认值为 `false`。 |
| `enablePasswordReset` | 指示是否允许用户重置其密码。 默认为 `true`。 |
| `maxInvalidPasswordAttempts` | 在指定过程中为给定的用户可能发生的失败登录尝试次数上限`passwordAttemptWindow`用户锁定之前。默认值为 5。 |
| `minRequiredNonalphanumericCharacters` | 最小必须出现在用户的密码的非字母数字字符数。 此值必须介于 0 和 128; 之间默认值为 1。 |
| `minRequiredPasswordLength` | 最小密码中所需的字符数。 此值必须介于 0 和 128; 之间默认值为 7。 |
| `name` | 已注册的提供程序的名称。 此值是必需的。 |
| `passwordAttemptWindow` | 在此期间的分钟数失败的登录尝试进行跟踪。 如果一个用户提供登录凭据无效`maxInvalidPasswordAttempts`时间在此指定窗口，它们被锁定。默认值为 10。 |
| `PasswordFormat` | 密码存储格式： `Clear`， `Hashed`，或`Encrypted`。 默认值为 `Hashed`。 |
| `passwordStrengthRegularExpression` | 如果提供，此正则表达式用于评估的强度的所选用户的密码创建新帐户时，或更改其密码时。 默认值为一个空字符串。 |
| `requiresQuestionAndAnswer` | 指定用户是否必须回答其安全问题时检索或重置其密码。 默认值为 `true`。 |
| `requiresUniqueEmail` | 指示是否在给定应用程序分区中的所有用户帐户必须都具有唯一的电子邮件地址。 默认值为 `true`。 |
| `type` | 指定提供程序的类型。 此值是必需的。 |

**表 2**： 成员身份和`SqlMembershipProvider`配置设置

除了`AspNetSqlMembershipProvider`，其他成员资格提供程序可能通过类似将标记添加到注册应用程序的应用程序根据`Web.config`文件。

> [!NOTE]
> 角色 framework 工作方式在很大程度相同： 没有中的默认值已注册的角色提供程序`machine.config`和注册的提供程序可以自定义应用程序的应用程序形式交付`Web.config`。 在将来的教程中，我们将检查角色 framework 和详细信息中的其配置标记。

### <a name="customizing-thesqlmembershipprovidersettings"></a>自定义`SqlMembershipProvider`设置

默认值`SqlMembershipProvider`(`AspNetSqlMembershipProvider`) 具有其`connectionStringName`属性设置为`LocalSqlServer`。 如`AspNetSqlMembershipProvider`提供程序，该连接字符串名称`LocalSqlServer`中定义`machine.config`。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

如你所见，此连接字符串定义数据库位于 SQL 2005 速成版 |DataDirectory|aspnetdb.mdf。 字符串 |DataDirectory |在运行时将能够指向翻译`~/App_Data/`目录中，因此数据库路径 |DataDirectory|aspnetdb.mdf 都会转换为`~/App_Data` / `aspnet.mdf`。

如果我们未指定任何成员资格提供程序信息中我们的应用程序`Web.config`文件，应用程序使用注册的默认成员资格提供程序， `AspNetSqlMembershipProvider`。 如果`~/App_Data/aspnet.mdf`数据库不存在，ASP.NET 运行时将自动创建它并添加应用程序服务架构。 但是，我们不想使用`aspnet.mdf`数据库; 相反，我们想要使用`SecurityTutorials.mdf`我们在步骤 2 中创建的数据库。 此修改，可以处于两种方式之一实现：

- **指定的值 * * *`LocalSqlServer`* * * 中的连接字符串名称 * * *`Web.config`* * *。** 通过覆盖`LocalSqlServer`连接字符串名称值中的`Web.config`，我们可以使用注册的默认成员资格提供程序 (`AspNetSqlMembershipProvider`)，并将其正确使用`SecurityTutorials.mdf`数据库。 这种方法是如果你是内容替换指定的配置设置正常`AspNetSqlMembershipProvider`。 有关此技术的详细信息，请参阅[Scott Guthrie](https://weblogs.asp.net/scottgu/)的博客文章[配置 ASP.NET 2.0 应用程序服务添加到使用 SQL Server 2000 或 SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)。
- **添加新的已注册提供程序的类型 * * *`SqlMembershipProvider`* * * 并配置其 * * *`connectionStringName`* * * 设置为指向 * * *`SecurityTutorials.mdf`* * * 数据库。** 这种方法是你想要自定义的数据库连接字符串除了其他配置属性的方案中十分有用。 在我自己的项目我始终使用这种方法由于其灵活性和可读性。

我们可以添加新的已注册的提供程序引用之前`SecurityTutorials.mdf`数据库，我们首先需要添加适当的连接字符串值中`<connectionStrings>`主题中`Web.config`。 以下标记将添加名为新的连接字符串`SecurityTutorialsConnectionString`引用 SQL Server 2005 Express Edition`SecurityTutorials.mdf`数据库中`App_Data`文件夹。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> 如果你使用的另一个数据库文件，请根据需要更新连接字符串。 有关构成正确的连接字符串的详细信息，请参阅[ConnectionStrings.com](http://www.connectionstrings.com/)。

接下来，添加到以下的成员身份配置标记`Web.config`文件。 此标记注册新的提供程序名为`SecurityTutorialsSqlMembershipProvider`。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

除了注册`SecurityTutorialsSqlMembershipProvider`提供程序，上面的标记定义`SecurityTutorialsSqlMembershipProvider`作为默认提供程序 (通过`defaultProvider`属性中`<membership>`元素)。 回想一下，成员身份 framework 可以有多个已注册的提供程序。 由于`AspNetSqlMembershipProvider`注册中的第一个提供程序为`machine.config`，除非我们指示，否则用作默认的提供程序。

目前，我们的应用程序具有两个已注册的提供程序：`AspNetSqlMembershipProvider`和`SecurityTutorialsSqlMembershipProvider`。 但是，在注册之前`SecurityTutorialsSqlMembershipProvider`通过添加我们无法清除了所有以前的提供程序已注册的提供程序[`<clear />`元素](https://msdn.microsoft.com/library/t062y6yc.aspx)立即之前我们`<add>`元素。 这将清除`AspNetSqlMembershipProvider`从注册的提供程序列表中，这意味着`SecurityTutorialsSqlMembershipProvider`是唯一的已注册成员资格提供程序。 如果我们使用此方法中，那么我们不需要将标记`SecurityTutorialsSqlMembershipProvider`为默认的提供程序，因为它将是唯一的已注册成员资格提供程序。 有关详细信息使用`<clear />`，请参阅[使用`<clear />`时添加提供程序](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)。

请注意，`SecurityTutorialsSqlMembershipProvider`的`connectionStringName`设置引用刚才-添加`SecurityTutorialsConnectionString`连接字符串名称，并且其`applicationName`设置已被设置为 SecurityTutorials 的值。 此外，`requiresUniqueEmail`设置已设置为`true`。 所有其他配置选项中的值相等`AspNetSqlMembershipProvider`。 如果您愿意，则请尝试进行任何配置修改在这里。 例如，你无法通过要求提供两个非字母数字字符，而不是一个，或通过增加为而不是七个八个字符的密码长度增强密码强度。

> [!NOTE]
> 回想一下，该成员身份框架允许在单个用户存储分区跨多个应用程序。 成员资格提供程序的`applicationName`设置指示提供程序时如何使用用户存储区使用哪些应用程序。 你显式设置的值很重要`applicationName`配置设置，因为如果`applicationName`未显式设置，它将分配给在运行时的 web 应用程序的虚拟根路径。 这很好地配合，只要不会更改应用程序的虚拟根路径，但是，如果将移到不同的路径，该应用程序`applicationName`也将更改设置。 在此情况下，成员资格提供程序将开始使用不同的应用程序分区，不是以前使用过。 在移动之前创建的用户帐户将位于不同的应用程序分区，并且这些用户将不再能够登录到网站。 有关此问题的更深入讨论，请参阅[始终设置`applicationName`属性时配置 ASP.NET 2.0 成员资格和其他提供商](https://weblogs.asp.net/scottgu/443634)。

## <a name="summary"></a>摘要

此时我们具有配置的应用程序服务的数据库 (`SecurityTutorials.mdf`) 并且已配置 web 应用程序，以便成员资格框架使用`SecurityTutorialsSqlMembershipProvider`我们刚刚注册的提供程序。 此注册的提供程序类型的`SqlMembershipProvider`并具有其`connectionStringName`设置为适当的连接字符串 (`SecurityTutorialsConnectionString`) 并将其`applicationName`显式设置的值。

现在我们就可以使用我们的应用程序中的成员资格框架。 在下一教程中我们将学习如何创建新用户帐户。 以下，我们将探讨用户，执行基于用户的授权，并将与用户相关的其他信息存储在数据库中进行身份验证。

尽情享受编程 ！

### <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [始终设置`applicationName`属性时配置 ASP.NET 2.0 成员资格和其他提供程序](https://weblogs.asp.net/scottgu/443634)
- [ASP.NET 2.0 应用程序服务配置为使用 SQL Server 2000 或 SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [下载 SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [检查 ASP.NET 2.0 s 成员资格、 角色和配置文件](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>`成员资格提供程序的元素](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>`元素](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>`元素的成员身份](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [使用`<clear />`时添加提供程序](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [直接使用`SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教程中包含的主题的视频培训

- [了解 ASP.NET 成员身份](../../../videos/authentication/understanding-aspnet-memberships.md)
- [配置 SQL 以使用成员身份架构](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [更改默认成员身份架构中的成员身份设置](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>关于作者

Scott Mitchell，多个 ASP/ASP.NET 丛书的作者和创始人 4GuysFromRolla.com，具有已使用自 1998 年 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是 *[Sam 教授自己 ASP.NET 2.0 24 小时内](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 可以在达到 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或通过在其博客地址[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Alicja Maziarz。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一页](storing-additional-user-information-cs.md)
[下一页](creating-user-accounts-vb.md)
