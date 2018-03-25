---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: "使用 Visual Studio 的 ASP.NET Web 部署： 为数据库部署做好准备 |Microsoft 文档"
author: tdykstra
description: "本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，使用的..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: caa79725ede320c4bd3e87ac246966c57175eb8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>使用 Visual Studio 的 ASP.NET Web 部署： 为数据库部署做好准备
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure App Service Web Apps 或第三方托管提供程序，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[序列中的第一个教程](introduction.md)。


## <a name="overview"></a>概述

本教程演示如何获取项目数据库部署已准备就绪。 数据库结构和一些 （并非所有） 的应用程序的两个中的数据必须将数据库部署到测试、 过渡和生产环境。

通常情况下，在开发应用程序时，你不想将部署到实时站点数据库到输入测试数据。 但是，你可能也存在一些要部署的生产数据。 在本教程中将配置 Contoso 大学项目，并且准备 SQL 脚本，以便在部署时，不包含正确的数据。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](troubleshooting.md)。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

示例应用程序使用 SQL Server Express LocalDB。 SQL Server Express 是免费版的 SQL Server。 它通常用在开发过程中由于它基于作为完整版本的 SQL Server 相同的数据库引擎。 你可以使用 SQL Server Express 进行测试，并确保应用程序将行为在生产中，但有少数例外不同 SQL Server 版本各不相同的功能相同。

LocalDB 是一种特殊的执行模式的 SQL Server Express，可用于处理数据库作为*.mdf*文件。 通常，将 LocalDB 数据库文件保存在*应用\_数据*web 项目的文件夹。 在 SQL Server Express 用户实例功能还可以使您能够使用*.mdf*文件，但用户实例功能已弃用; 因此，使用建议 LocalDB *.mdf*文件。

通常 SQL Server Express 不用于生产 web 应用程序。 LocalDB 具体而言不是建议生产使用与 web 应用程序由于它不是使用 IIS。

在 Visual Studio 2012 中，默认情况下，使用 Visual Studio 安装 LocalDB。 在 Visual Studio 2010 和早期版本中，在使用 Visual Studio; 默认情况下安装 SQL Server Express （而不是 LocalDB)就是为什么你安装为中的先决条件之一[本系列第一个教程](introduction.md)。

有关 SQL Server 版本的详细信息，包括 LocalDB，请参阅以下资源[使用 SQL Server 数据库](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver)。

## <a name="entity-framework-and-universal-providers"></a>实体框架和通用提供程序

为数据库访问 Contoso 大学应用程序需要必须部署与应用程序中，因为它不包括.NET Framework 中的以下软件：

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) （启用 ASP.NET 成员资格系统以使用 Azure SQL 数据库）
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

由于此软件包含在 NuGet 包，以便所需的程序集使用项目部署已设置该项目。 （链接将指向这些包，它可能比你在本教程中下载的初学者项目中安装的内容更新的当前版本而定。）

如果将部署到第三方托管提供程序而不是 Azure，请确保你使用实体框架 5.0 或更高版本。 早期版本的 Code First 迁移需要完全信任，并且大多数托管提供程序将在中等信任中运行你的应用程序。 有关中等信任的详细信息，请参阅[部署到 IIS 作为测试环境](deploying-to-iis.md)教程。

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>为应用程序数据库部署配置 Code First 迁移

Contoso 大学应用程序数据库进行管理的 Code First，并且将通过使用 Code First 迁移来将其部署。 通过使用 Code First 迁移的数据库部署的概述，请参阅[本系列第一个教程](introduction.md)。

当部署的应用程序数据库时，通常你不只是开发将数据库部署中它的数据的所有到生产环境，因为很多中它的数据可能存在仅用于测试目的。 例如，一个测试数据库中的学生名称是虚构的。 另一方面，你通常不能部署只是数据库中它不含数据的结构根本。 测试数据库中的数据的一些可能的真实数据，必须在那里，当用户开始使用该应用程序。 例如，你的数据库可能必须包含有效年级值或实际部门名称的表。

若要模拟此常见方案中，你将配置 Code First 迁移`Seed`将你想要在那里在生产环境中的数据插入到数据库的方法。 这`Seed`方法不应插入测试数据，因为它将 Code First 在生产环境中创建数据库后在生产中运行。

在早期版本的 Code First 迁移已发布之前，这是常见的`Seed`方法来插入测试数据此外，因为与在开发过程中的每个模型更改，数据库没有完全删除和重新创建从零开始。 使用 Code First 迁移，数据保留在数据库更改后的测试，因此包括中的测试数据`Seed`方法不是必需。 你下载的项目使用的方法中的所有数据，其中包括`Seed`初始值设定项类的方法。 在本教程中，将禁用该初始值设定项类和`enable Migrations. Then you'll update the `种子方法中的迁移配置类，以便它可以将插入你想要在生产中插入的唯一数据。

下图说明了应用程序数据库的架构：

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

有关这些教程中，将假定`Student`和`Enrollment`首先部署站点时，表应为空。 其他表包含已在应用程序投入实时必须预加载的数据。

### <a name="disable-the-initializer"></a>禁用初始值设定项

由于你将使用 Code First 迁移，则不需要使用`DropCreateDatabaseIfModelChanges`Code First 初始值设定项。 此初始值设定项的代码位于*SchoolInitializer.cs* ContosoUniversity.DAL 项目文件中的。 中的设置`appSettings`元素*Web.config*文件会导致应用程序尝试以首次访问数据库时运行此初始值设定项：

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

打开应用程序*Web.config*文件，然后删除或注释掉`add`指定 Code First 初始值设定项类的元素。 `appSettings`元素现在如下所示：

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> 若要指定一个初始值设定项类的另一种方法是执行此操作通过调用`Database.SetInitializer`中`Application_Start`中的方法*Global.asax*文件。 如果要使用该方法指定初始值设定项的项目中启用迁移，删除代码的行。


> [!NOTE]
> 如果使用的 Visual Studio 2013，添加步骤 2 和 3 之间的以下步骤: (a) 在 PMC 输入"更新包 entityframework-版本 6.1.1"若要获取 EF 的当前版本。 然后 (b) 生成项目以获取生成错误的列表，并修复它们。 删除语句用于命名空间中不再存在，右键单击，然后单击解决来添加一个 using 语句，在需要并更改 System.Data.Entity.EntityState System.Data.EntityState 的匹配项。


### <a name="enable-code-first-migrations"></a>启用 Code First 迁移

1. 请确保 ContosoUniversity 项目 (不 ContosoUniversity.DAL) 设置为启动项目。 在**解决方案资源管理器**，右键单击 ContosoUniversity 项目并选择**设为启动项目**。 Code First 迁移将查找启动项目，以便找到的数据库连接字符串。
2. 从**工具**菜单上，单击**库程序包管理器**(或**NuGet 包管理器**)，然后**程序包管理器控制台**。

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. 在顶部**程序包管理器控制台**窗口选择 ContosoUniversity.DAL 作为默认项目，然后在`PM>`提示符处输入"启用迁移"。

    ![enable-migrations 命令](preparing-databases/_static/image4.png)

    (如果收到错误显示*启用迁移*无法识别命令，输入命令*更新包 EntityFramework-重新安装*，然后重试。)

    此命令创建*迁移*ContosoUniversity.DAL 项目中，且文件夹将放置在该文件夹两个文件： *Configuration.cs*文件，可以用来配置迁移和*InitialCreate.cs*文件创建数据库的初始迁移。

    ![迁移文件夹](preparing-databases/_static/image5.png)

    你选择的 DAL 项目中**默认项目**的下拉列表**程序包管理器控制台**因为`enable-migrations`命令必须在包含 Code First 的项目中执行上下文类。 当该类是在类库项目中时，Code First 迁移查找解决方案的启动项目中的数据库连接字符串。 在 ContosoUniversity 解决方案中，web 项目已设置为启动项目。 如果你不想指定为启动项目，Visual Studio 中具有的连接字符串的项目，你可以在 PowerShell 命令中指定的启动项目。 若要查看的命令语法，请输入命令`get-help enable-migrations`。

    `enable-migrations`命令自动创建第一次迁移，因为该数据库已存在。 一种替代方法是创建数据库的迁移。 为此，请使用**服务器资源管理器**或**SQL Server 对象资源管理器**先删除 ContosoUniversity 数据库，然后启用迁移。 启用迁移后，第一次迁移手动创建通过输入命令"添加迁移 InitialCreate"。 然后可以通过输入命令"更新数据库"创建数据库。

### <a name="set-up-the-seed-method"></a>设置的 Seed 方法

对于本教程将添加固定的数据，通过将代码添加到`Seed`方法的 Code First 迁移`Configuration`类。 代码优先迁移调用`Seed`每个迁移后的方法。

由于`Seed`方法运行每个迁移后，已存在数据的表中后的初始迁移。 若要处理这种情况下，你将使用`AddOrUpdate`方法来更新具有已插入，或将它们插入如果它们尚不存在的行。 `AddOrUpdate`方法可能不是你的方案的最佳选择。 有关详细信息，请参阅[负责使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 博客上。

1. 打开*Configuration.cs*文件并将中的注释`Seed`方法替换为以下代码：

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. 对引用`List`具有下面的红色波浪线，因为你不具有`using`尚未为其命名空间语句。 右键单击其中一个的实例`List`单击**解决**，然后单击**using System.Collections.Generic**。

    ![解决与 using 语句](preparing-databases/_static/image6.png)

    此菜单选项添加到了以下代码`using`语句文件的顶部附近。

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. 按 CTRL-SHIFT-B 生成项目。

项目现在已准备好部署*ContosoUniversity*数据库。 部署应用程序首次运行它，并导航到的页访问数据库之后, Code First 将创建数据库并运行此查询`Seed`方法。

> [!NOTE]
> 将代码添加到`Seed`方法是，您可以将固定的数据插入数据库的许多方式之一。 替代方法是将代码添加到`Up`和`Down`的每个迁移类的方法。 `Up`和`Down`方法包含实现数据库更改的代码。 你将看到它们中的一些示例[部署某一数据库更新](deploying-a-database-update.md)教程。
> 
> 你也可以编写通过使用执行 SQL 语句的代码`Sql`方法。 例如，如果你正将预算列添加到将 Department 表，并想要将所有部门预算到 $1.000.00 都初始化为迁移的一部分，则可以添加到代码的以下行`Up`该迁移方法：
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>创建成员资格数据库部署的脚本

Contoso 大学应用程序使用 ASP.NET 成员资格系统和窗体身份验证用户进行身份验证和授权。 **更新信用额度**页面进行只向管理员角色的用户访问。

运行应用程序，然后单击**课程**，然后单击**更新信用额度**。

![单击更新信用额度](preparing-databases/_static/image7.png)

**登录**页将显示，因为**更新信用额度**页需要管理权限。

输入*管理员*作为用户名称和*devpwd*作为密码，然后单击**登录**。

![登录页](preparing-databases/_static/image8.png)

**更新信用额度**页将出现。

![更新信用额度页面](preparing-databases/_static/image9.png)

用户和角色的信息，请参阅*aspnet ContosoUniversity*由指定的数据库**DefaultConnection**中的连接字符串*Web.config*文件。

此数据库不受 Entity Framework Code First，因此您无法使用迁移来将其部署。 你将使用 dbDacFx 提供程序来部署数据库架构，并且你将配置要运行的脚本，将初始数据插入数据库表的发布配置文件。

> [!NOTE]
> 使用 Visual Studio 2013 引入了新的 ASP.NET 成员资格系统 （现在名为 ASP.NET 标识）。 新系统，您可以将应用程序和成员资格表保留在同一数据库中，并且你可以使用 Code First 迁移来同时部署。 示例应用程序使用不能通过使用 Code First 迁移部署早期 ASP.NET 成员资格系统。 用于部署此成员资格数据库的过程也适用于你的应用程序需要部署的 SQL Server 数据库，不通过 Entity Framework Code First 创建的任何其他方案。


此处，你通常不希望在生产环境中开发中具有相同的数据。 当你首次部署站点时，很普遍排除大部分或全部为测试创建的用户帐户。 因此，下载的项目具有两个成员资格数据库： *aspnet ContosoUniversity.mdf*与开发用户和*aspnet ContosoUniversity Prod.mdf*与生产用户。 本教程中的用户名称是在这两个数据库中相同：*管理员*和*nonadmin*。 这两个用户具有的密码*devpwd*开发数据库中和*prodpwd*生产数据库中。

你将对测试环境和生产用户对过渡和生产部署开发用户。 为此，你将在本教程，一个用于开发，一个用于生产，创建两个 SQL 脚本和更高版本的教程中，你将配置发布过程来运行它们。

> [!NOTE]
> 成员资格数据库将存储帐户密码的哈希值。 若要部署到另一台计算机中的帐户，你必须确保与源计算机上，哈希例程不生成目标服务器上的不同哈希。 它们将生成相同的哈希时使用 ASP.NET Universal Providers，只要你不要更改默认的算法。 默认的算法是 HMACSHA256 和中指定**验证**属性 **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)**  Web.config 文件中的元素。


通过使用 SQL Server Management Studio (SSMS)，或使用第三方工具，你可以手动创建数据部署脚本。 本教程的此其余部分将显示如何执行此操作在 SSMS 中，但如果你不想要安装和使用 SSMS 则可以从项目的已完成版本获取脚本并跳到在其中你存储它们在解决方案文件夹中的部分。

若要安装 SSMS，将其从安装[下载中心： Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)通过单击[ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe)或[ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe)。 如果选择了错误对于你的系统，它将无法安装，并且您可以尝试另一个。

（请注意这是一个 600 兆字节下载。 它可能需要较长的时间到安装，并且将会要求你的计算机重启。)

在 SQL Server 安装中心的第一页，单击**新的 SQL Server 独立安装或向现有安装添加功能**，并按照说明操作，接受默认选项。

### <a name="create-the-development-database-script"></a>创建开发数据库脚本

1. 运行 SSMS。
2. 在**连接到服务器**对话框框中，输入*(localdb) \v11.0*作为**服务器名称**，保留**身份验证**设置为**Windows 身份验证**，然后单击**连接**。

    ![SSMS 连接到服务器](preparing-databases/_static/image10.png)
3. 在**对象资源管理器**窗口中，展开**数据库**，右键单击**aspnet ContosoUniversity**，单击**任务**，然后单击**生成脚本**。

    ![SSMS 生成脚本](preparing-databases/_static/image11.png)
4. 在**生成和发布脚本**对话框中，单击**设置脚本编写选项**。

    你可以跳过**选择对象**步骤因为默认设置**编写整个数据库和所有数据库对象脚本**并且这是你想得到。
5. 单击 **“高级”**。

    ![SSMS 脚本编写选项](preparing-databases/_static/image12.png)
6. 在**高级脚本编写选项**对话框中，向下滚动到**的数据的脚本类型**，然后单击**仅限数据**下拉列表中的选项。
7. 更改**编写 USE DATABASE 脚本**到**False**。 使用语句对于 Azure SQL 数据库无效，且不需要在测试环境中部署到 SQL Server Express。

    ![SSMS 数据仅限脚本，没有 USE 语句](preparing-databases/_static/image13.png)
8. 单击 **“确定”**。
9. 在**生成和发布脚本**对话框中，**文件名**框指定将在其中创建脚本。 将路径更改为解决方案文件夹 （ContosoUniversity.sln 文件的文件夹） 和文件名与*aspnet 数据 dev.sql*。
10. 单击**下一步**转到**摘要**选项卡上，并依次**下一步**再次以创建脚本。

    ![创建的 SSMS 脚本](preparing-databases/_static/image14.png)
11. 单击 **“完成”**。

### <a name="create-the-production-database-script"></a>创建生产数据库脚本

因为你尚未与生产数据库运行该项目，并不尚未附加到 LocalDB 实例。 因此，你需要首先附加数据库。

1. 在 SSMS**对象资源管理器**，右键单击**数据库**单击**附加**。

    ![SSMS 附加](preparing-databases/_static/image15.png)
- 在**附加数据库**对话框中，单击**添加**然后导航到*aspnet ContosoUniversity Prod.mdf*文件中*应用\_数据*文件夹。

    ![SSMS.mdf 文件添加附加](preparing-databases/_static/image16.png)
- 单击 **“确定”**。
- 请按照你之前用来创建用于生产文件的脚本的相同过程。 命名该脚本文件*aspnet 数据 prod.sql*。

## <a name="summary"></a>摘要

这两个数据库现在已准备好部署和解决方案文件夹中有两个数据部署脚本。

![数据部署脚本](preparing-databases/_static/image17.png)

在以下教程中，你将配置项目设置会影响部署，并且您设置了自动*Web.config*文件必须部署的应用程序中不同的设置的转换。

## <a name="more-information"></a>详细信息

有关 NuGet 的详细信息，请参阅[使用 NuGet 管理项目库](https://msdn.microsoft.com/magazine/hh547106.aspx)和[NuGet 文档](http://docs.nuget.org/docs/start-here/overview)。 如果你不想使用 NuGet，你将需要了解如何分析以确定它能做什么安装时的 NuGet 包。 (例如，它可能配置*Web.config*转换，PowerShell 将脚本配置为运行在生成时，等等。)若要了解有关 NuGet 的工作原理的详细信息，请参阅[创建和发布包](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)和[配置文件和源代码转换](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)。

>[!div class="step-by-step"]
[上一页](introduction.md)
[下一页](web-config-transformations.md)
