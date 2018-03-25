---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署 SQL Server Compact 数据库-2 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5296bc1ca3fd0b24123bd79a550a7e2cffc34a44
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 部署 SQL Server Compact 数据库-2 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

本教程演示如何设置两个 SQL Server Compact 数据库和数据库引擎进行部署。

为数据库访问 Contoso 大学应用程序需要必须部署与应用程序中，因为它不包括.NET Framework 中的以下软件：

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) （数据库引擎）。
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) （启用 ASP.NET 成员资格系统以使用 SQL Server Compact）
- [实体框架 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)（代码进行迁移的第一个）。

数据库结构和一些 （并非所有） 的应用程序的两个中的数据还必须部署数据库。 通常情况下，在开发应用程序时，你不想将部署到实时站点数据库到输入测试数据。 但是，你还可以输入一些生产数据，您想要部署。 在本教程中将配置 Contoso 大学项目，以便在部署时，所需的软件和正确的数据将包括在内。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="sql-server-compact-versus-sql-server-express"></a>与 SQL Server Express 的 SQL Server Compact

示例应用程序使用 SQL Server Compact 4.0。 此数据库引擎是网站; 的相对较新选项早期版本的 SQL Server Compact 不完成 web 宿主环境中的工作。 SQL Server Compact 提供与使用 SQL Server Express 进行开发和部署到完整的 SQL Server 的更常见的方案相比的几个好处。 具体取决于您选择托管提供商的情况下，SQL Server Compact 可能更便宜，若要部署，因为某些提供商根据额外以支持完整的 SQL Server 数据库。 因为可以将数据库引擎自身部署为 web 应用程序的一部分，没有 SQL Server Compact 额外收费。

但是，你还应注意其限制。 SQL Server Compact 不支持存储的过程、 触发器、 视图或复制。 (SQL Server compact 不支持的 SQL Server 功能的完整列表，请参阅[差异之间 SQL Server Compact 和 SQL Server](https://msdn.microsoft.com/library/bb896140.aspx)。)此外，一些可用于处理架构和 SQL Server Express 和 SQL Server 数据库中的数据的工具不起作用 SQL Server Compact。 例如，你不能使用 SQL Server Management Studio 或 SQL Server Data Tools 在 Visual Studio 中使用 SQL Server Compact 数据库。 必须用于使用 SQL Server Compact 数据库的其他选项：

- 在 Visual Studio 中，为 SQL Server Compact 提供有限的数据库操作功能，可以使用服务器资源管理器。
- 你可以使用的数据库操作功能[WebMatrix](https://www.microsoft.com/web/webmatrix/)，它具有比服务器资源管理器功能更多。
- 你可以使用相对齐全的第三方或打开源工具，例如[SQL Server Compact 工具箱](https://github.com/ErikEJ/SqlCeToolbox)和[SQL Compact 数据和架构脚本实用工具](https://github.com/ErikEJ/SqlCeToolbox)。
- 你可以编写并运行你自己的 DDL （数据定义语言） 脚本来操作数据库架构。

你可以开始使用 SQL Server Compact，然后再升级你的需求不断发展更高版本。 后面的这一系列教程演示如何从 SQL Server Compact 中到 SQL Server Express 和 SQL Server 迁移。 但是，如果你要创建新的应用程序，并且期望在不久的将来需要 SQL Server，则可能最好 SQL Server 或 SQL Server Express 开头。

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>为部署配置 SQL Server Compact 数据库引擎

通过安装以下 NuGet 包添加了 Contoso 大学应用程序中的数据访问所需的软件：

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) （ASP.NET 通用提供程序）
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

链接是指向这些包，它可能比你在本教程中下载的初学者项目中安装的内容更新的当前版本。 对于部署到宿主提供程序，请确保你使用实体框架 5.0 或更高版本。 早期版本的 Code First 迁移需要完全信任，并且许多托管提供商在你的应用程序将在中等信任中运行。 有关中等信任的详细信息，请参阅[作为测试环境部署到 IIS](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教程。

NuGet 包安装通常将负责部署与应用程序的此软件所需的所有内容。 在某些情况下，这涉及到任务，例如更改 Web.config 文件并添加在生成解决方案时运行的 PowerShell 脚本。 **如果你想要不使用 NuGet 添加对任何这些功能 （如 SQL Server Compact 和实体框架） 的支持，请确保你了解 NuGet 包安装，你可以手动执行同样的工作的功能。**

是一个例外，NuGet 不带负责所必须进行以确保成功部署的所有内容。 SqlServerCompact NuGet 包添加到你将复制到本机程序集的项目的生成后脚本*x86*和*amd64*项目下的子文件夹*bin*文件夹中，但该脚本不在项目中包括这些文件夹。 因此，Web 部署将不将它们复制到目标 web 站点除非你手动将其包含在项目中。 （此行为会使从默认部署配置; 另一个选项，你不会使用这些教程中，是更改控制此行为的设置。 你可以更改的设置是**仅运行该应用程序所需的文件**下**项部署**上**打包/发布 Web**选项卡**项目属性**窗口。 更改此设置不通常建议，因为这会导致许多其他文件部署到生产环境不是那里需要。 有关替代项的详细信息，请参阅[配置项目属性](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)教程。)

生成项目，然后在**解决方案资源管理器**单击**显示所有文件**如果尚未这样做。 你可能还需要单击**刷新**。

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

展开**bin**文件夹以查看**amd64**和**x86**文件夹，然后选择这些文件夹，右键单击，并选择**包括在项目**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

文件夹图标将更改以显示该文件夹，已包括在项目中。

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>配置应用程序数据库部署的 Code First 迁移

当部署的应用程序数据库时，通常你不只是开发将数据库部署中它的数据的所有到生产环境，因为很多中它的数据可能存在仅用于测试目的。 例如，一个测试数据库中的学生名称是虚构的。 另一方面，你通常不能部署只是数据库中它不含数据的结构根本。 测试数据库中的数据的一些可能的真实数据，必须在那里，当用户开始使用该应用程序。 例如，你的数据库可能必须包含有效年级值或实际部门名称的表。

若要模拟此常见方案中，你将配置代码第一个迁移 Seed 方法，用于在你想要在那里在生产环境中的数据插入数据库。 此种子方法不会插入测试数据，因为它将 Code First 在生产环境中创建数据库后在生产中运行。

在早期版本的 Code First 迁移已发布之前，它带来了常见种子方法，此外，插入测试数据因为与在开发过程中的每个模型更改，数据库没有完全删除和重新创建从零开始。 使用 Code First 迁移，测试数据保留在数据库更改后，因此不需要包括 Seed 方法中的测试数据。 你下载的项目使用初始值设定项类的 Seed 方法中包括所有数据的预迁移方法。 在本教程中，可以将禁用初始值设定项类，并启用迁移。 然后你将更新在迁移配置类的 Seed 方法，以便它可以将插入你想要在生产中插入的数据。

下图说明了应用程序数据库的架构：

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

有关这些教程中，将假定`Student`和`Enrollment`首先部署站点时，表应为空。 其他表包含已在应用程序投入实时必须预加载的数据。

由于你将使用 Code First 迁移，你不再需要使用**DropCreateDatabaseIfModelChanges** Code First 初始值设定项。 此初始值设定项的代码处于 ContosoUniversity.DAL 项目中的 SchoolInitializer.cs 文件。 中的设置**appSettings**元素的 Web.config 文件会导致应用程序尝试以首次访问数据库时运行此初始值设定项：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

打开应用程序 Web.config 文件，移除指定的 appSettings 元素中的 Code First 初始值设定项类的元素。 现在，该 appSettings 元素如下所示：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> 若要指定一个初始值设定项类的另一种方法是执行此操作通过调用`Database.SetInitializer`中`Application_Start`中的方法*Global.asax*文件。 如果要使用该方法指定初始值设定项的项目中启用迁移，删除代码的行。


接下来，启用 Code First 迁移。

第一步是确保 ContosoUniversity 项目设置为启动项目。 在**解决方案资源管理器**，右键单击 ContosoUniversity 项目并选择**设为启动项目**。 Code First 迁移将查找启动项目，以便找到的数据库连接字符串。

从**工具**菜单上，单击**库程序包管理器**然后**程序包管理器控制台**。

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

在顶部**程序包管理器控制台**窗口选择 ContosoUniversity.DAL 作为默认项目，然后在`PM>`提示符处输入"启用迁移"。

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

此命令创建*Configuration.cs*在新的文件*迁移*ContosoUniversity.DAL 项目文件夹中的。

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

你选择 DAL 项目，因为必须在包含的第一个代码的上下文类的项目中执行"enable-migrations"命令。 当该类是在类库项目中时，Code First 迁移查找解决方案的启动项目中的数据库连接字符串。 在 ContosoUniversity 解决方案中，web 项目已设置为启动项目。 （如果你确实不想要指定为启动项目，Visual Studio 中具有的连接字符串的项目，你可以指定启动项目中的 PowerShell 命令。 若要查看 enable-migrations 命令的命令语法，你可以输入命令"获取帮助 enable-migrations"。）

打开 Configuration.cs 文件并将中的注释`Seed`方法替换为以下代码：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

对引用`List`具有下面的红色波浪线，因为你不具有`using`尚未为其命名空间语句。 右键单击其中一个的实例`List`单击**解决**，然后单击**using System.Collections.Generic**。

![解决与 using 语句](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

此菜单选项添加到了以下代码`using`语句文件的顶部附近。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> 将代码添加到`Seed`方法是，您可以将固定的数据插入数据库的许多方式之一。 替代方法是将代码添加到`Up`和`Down`的每个迁移类的方法。 `Up`和`Down`方法包含实现数据库更改的代码。 你将看到它们中的一些示例[部署某一数据库更新](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)教程。
> 
> 你也可以编写通过使用执行 SQL 语句的代码`Sql`方法。 例如，如果你正将预算列添加到将 Department 表，并想要将所有部门预算到 $1.000.00 都初始化为迁移的一部分，则可以添加到代码的以下行`Up`该迁移方法：
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> 此示例显示本教程使用`AddOrUpdate`中的方法`Seed`方法的 Code First 迁移`Configuration`类。 代码优先迁移调用`Seed`每个迁移后，因此该方法更新具有已插入，或将其插入如果它们尚不存在的行。 `AddOrUpdate`方法可能不是你的方案的最佳选择。 有关详细信息，请参阅[负责使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 博客上。


按 CTRL-SHIFT-B 生成项目。

下一步是创建`DbMigration`类初始迁移。 你希望此迁移，以创建新的数据库，因此你必须删除已存在的数据库。 SQL Server Compact 数据库包含在*.sdf*文件中*应用\_数据*文件夹。 在**解决方案资源管理器**，展开*应用\_数据*在 ContosoUniversity 项目中以查看两个 SQL Server Compact 数据库，这由*.sdf*文件。

右键单击*School.sdf*文件并单击**删除**。

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

在**程序包管理器控制台**窗口中，输入命令"Initial"以创建初始迁移并将其命名为"初始"。

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First 迁移创建中的另一个类文件*迁移*文件夹，并且此类包含创建数据库架构的代码。

在**程序包管理器控制台**，输入命令"更新数据库"创建数据库并运行**种子**方法。

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(如果你收到的错误消息指示表已存在，无法创建，则可能是因为你运行应用程序删除了数据库后，在你执行之前`update-database`。 In that case，删除*School.sdf*再次文件，然后重试`update-database`命令。)

运行该应用程序。 现在学生页为空，但教师页包含教师。 这是你将获得的内容在生产中部署应用程序之后。

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

项目现在已准备好部署*学校*数据库。

## <a name="creating-a-membership-database-for-deployment"></a>为部署创建的成员资格数据库

Contoso 大学应用程序使用 ASP.NET 成员资格系统和窗体身份验证用户进行身份验证和授权。 只有管理员能够访问其页面之一。 若要查看此页上，运行应用程序并选择**更新信用额度**下的弹出菜单中从**课程**。 应用程序将显示**Log In**页上，因为只有管理员有权使用**更新信用额度**页。

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

登录为"admin"使用密码"Pa$ w0rd"（请注意数字零代替"w0rd"中的字母"o"）。 你登录后**更新信用额度**显示页。

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

当你首次部署站点时，很普遍排除大部分或全部为测试创建的用户帐户。 在这种情况下，你将部署一个管理员帐户和任何用户帐户。 而不是手动删除测试帐户，你将创建具有仅在生产环境中需要一个管理员用户帐户的新成员资格数据库。

> [!NOTE]
> 成员资格数据库将存储帐户密码的哈希值。 若要部署到另一台计算机中的帐户，你必须确保与源计算机上，哈希例程不生成目标服务器上的不同哈希。 它们将生成相同的哈希时使用 ASP.NET Universal Providers，只要你不要更改默认的算法。 默认的算法是 HMACSHA256 和中指定**验证**属性 **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)**  Web.config 文件中的元素。


成员资格数据库不会保留通过 Code First 迁移，并且没有设定种子测试帐户的数据库 （如没有的 School 数据库） 没有自动初始值设定项。 因此，要保持可用的测试数据将使测试数据库的副本之前创建一个新。

在**解决方案资源管理器**，重命名*aspnet.sdf*文件中*应用\_数据*文件夹*aspnet Dev.sdf*。 (不生成副本，只需将其重命名-你将在稍后创建新的数据库。)

在**解决方案资源管理器**，请确保选择 web 项目 （ContosoUniversity、 不 ContosoUniversity.DAL）。 然后在**项目**菜单上，选择**ASP.NET 配置**运行**网站管理工具**(WAT)。

选择**安全**选项卡。

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

单击**创建或管理角色**并添加**管理员**角色。

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

向后定位到**安全**选项卡上，单击**Create User**，并以管理员身份添加用户"admin"。 在单击之前**Create User**按钮上**Create User**页上，确保你选择**管理员**复选框。 本教程中使用的密码已"Pa$ w0rd"，并且您可以输入任何电子邮件地址。

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

关闭浏览器。 在**解决方案资源管理器**，单击刷新按钮以查看新*aspnet.sdf*文件。

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

右键单击**aspnet.sdf**和选择**包括在项目**。

## <a name="distinguishing-development-from-production-databases"></a>从生产数据库的可分辨开发

在此部分中，你将重命名数据库，以便开发版本是学校 Dev.sdf，而且 aspnet Dev.sdf 和生产版本是学校 Prod.sdf 和 aspnet Prod.sdf。 这不是必需的但这样做因此有助于使你获取的数据库的测试和生产版本混淆。

在**解决方案资源管理器**，单击**刷新**展开应用\_数据文件夹，请参阅前面创建的 School 数据库，右键单击它并选择**包括在项目中**.

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

重命名*aspnet.sdf*到*aspnet Prod.sdf*。

重命名*School.sdf*到*学校 Dev.sdf*。

当你不想使用的 Visual Studio 中运行应用程序*-Prod*版本的数据库文件中，你想要使用*-开发人员*版本。 因此你必须更改 Web.config 文件中的连接字符串，以便它们指向*-开发人员*版本与数据库。 （你尚未创建的 School Prod.sdf 文件，但这是确定，因为 Code First 将创建该数据库有运行你的应用程序的生产第一个时间。）

打开应用程序 Web.config 文件，并找到连接字符串：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

将"aspnet.sdf"更改为"aspnet-Dev.sdf"，并将"School.sdf"更改为"学校 Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

SQL Server Compact 数据库引擎和这两个数据库现已准备好可部署。 在以下教程你将自动设置*Web.config*文件必须是不同的开发、 测试和生产环境中的设置的转换。 （必须更改的设置包括连接字符串，但你将设置这些更改更高版本时创建的发布配置文件。）

## <a name="more-information"></a>详细信息

有关 NuGet 的详细信息，请参阅[使用 NuGet 管理项目库](https://msdn.microsoft.com/magazine/hh547106.aspx)和[NuGet 文档](http://docs.nuget.org/docs/start-here/overview)。 如果你不想使用 NuGet，你将需要了解如何分析以确定它能做什么安装时的 NuGet 包。 (例如，它可能配置*Web.config*转换，PowerShell 将脚本配置为运行在生成时，等等。)若要了解有关 NuGet 的工作原理的详细信息，请参阅尤其[创建和发布包](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)和[配置文件和源代码转换](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-introduction-1-of-12.md)
[下一页](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
