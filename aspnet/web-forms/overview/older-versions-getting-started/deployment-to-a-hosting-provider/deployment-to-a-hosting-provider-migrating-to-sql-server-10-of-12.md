---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 迁移到 SQL Server-10 12 |Microsoft 文档"
author: tdykstra
description: "这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Stu 包含 SQL Server Compact 数据库..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: b97834e3e287645151bf927996fde63d93ae8356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>部署具有 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 的 ASP.NET Web 应用程序： 迁移到 SQL Server-10 12
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 这一系列的教程演示如何部署 （发布） ASP.NET web 应用程序项目，它通过使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 中包含 SQL Server Compact 数据库。 如果你安装 Web 发布更新，也可以使用 Visual Studio 2010。 有关序列的简介，请参阅[序列中的第一个教程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 显示部署功能后，将 Visual Studio 2012 RC 版本中推出，演示如何部署 SQL Server Compact 以外的 SQL Server 版本并演示如何将部署到 Azure App Service Web Apps 的教程，请参阅[的 ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概述

本教程演示如何从 SQL Server Compact 迁移到 SQL Server。 你可能想要执行此操作的一个原因是为了充分利用 SQL Server Compact 不支持，如存储的过程、 触发器、 视图或复制的 SQL Server 功能。 有关 SQL Server Compact 和 SQL Server 之间的差异的详细信息，请参阅[部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教程。

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express，而不是完整的 SQL Server 进行开发

一旦你已决定要升级到 SQL Server，你可能想要在你的开发和测试环境中使用 SQL Server 或 SQL Server Express。 除了工具支持在和中数据库引擎功能的差异，还有一些提供程序实现中 SQL Server Compact 和其他版本的 SQL Server 之间的差异。 这些差异可能导致相同的代码，以生成不同的结果。 因此，如果你决定要保留 SQL Server Compact 与开发数据库，应彻底测试你的站点中 SQL Server 或 SQL Server Express 之前每个部署到生产环境的测试环境中。

不同于 SQL Server Compact，SQL Server Express 是实质上是相同的数据库引擎，并使用相同的.NET 提供程序作为完整的 SQL Server。 当你测试与 SQL Server Express 时，则可以确信获得相同的结果将与 SQL Server。 您可以使用 SQL Server Express 可以使用的 SQL Server 使用相同的数据库工具的大多数 (值得注意的例外正在[SQL Server 事件探查器](https://msdn.microsoft.com/library/ms181091.aspx))，并且它支持的 SQL Server 存储的过程、 视图、 触发器等其他功能和复制。 （你通常需要在生产网站，但是使用完整的 SQL Server。 SQL Server Express 可以运行在共享宿主环境中，但它不是为此，并且许多宿主提供程序不支持它。）

如果你正在使用 Visual Studio 2012，你通常选择 SQL Server Express LocalDB 适用于你开发环境，因为它是默认情况下，使用 Visual Studio 安装的内容。 但是，LocalDB 不工作不在 IIS 中，因此你必须使用 SQL Server 或 SQL Server Express 为你的测试环境。

### <a name="combining-databases-versus-keeping-them-separate"></a>组合而不保留它们单独的数据库

Contoso 大学应用程序具有两个 SQL Server Compact 数据库： 成员资格数据库 (*aspnet.sdf*) 和应用程序数据库 (*School.sdf*)。 在迁移时，可以将这些数据库迁移到两个单独的数据库或单个数据库。 你可能想要将它们合并为了简化你的应用程序数据库和成员资格数据库之间的数据库联接。 托管计划还可能会提供相应原因，才能将它们合并。 例如，托管提供商可能会收费多个数据库的详细信息，或可能不甚至允许多个数据库。 这是使用 Cytanium Lite 承载用于本教程中，这样仅单个 SQL Server 数据库的帐户的情况。

在本教程中，你将两个将数据库迁移这种方式：

- 将迁移到在开发环境中的两个 LocalDB 数据库。
- 将迁移到测试环境中的两个 SQL Server Express 数据库。
- 迁移到一个组合完整 SQL Server 数据库的生产环境中。

提示： 如果你收到如下错误消息，或当你完成本教程的内容不起作用，请务必检查[故障排除页](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="installing-sql-server-express"></a>安装 SQL Server Express

默认情况下，使用 Visual Studio 2010，会自动安装 SQL Server Express，但默认情况下它不与安装 Visual Studio 2012。 若要安装 SQL Server 2012 Express，请单击以下链接

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

选择*ENU / x64 / SQLEXPR\_x64\_ENU.exe*或*ENU/x86/SQLEXPR\_x86\_ENU.exe*，并在安装向导中接受默认值设置。 有关安装选项的详细信息，请参阅[从安装向导 （安装程序） 中安装 SQL Server 2012](https://msdn.microsoft.com/library/ms143219.aspx)。

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>为测试环境中创建 SQL Server Express 数据库

下一步是创建的 ASP.NET 成员资格和 School 数据库。

从**视图**菜单中选择**服务器资源管理器**(**数据库资源管理器**在 Visual Web Developer)，然后右键单击**数据连接**和选择**创建新的 SQL Server 数据库**。

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

在**创建新的 SQL Server 数据库**对话框框中，输入"。 \SQLExpress"中**服务器名称**框和"aspnet-测试"**新的数据库名称**框，然后单击**确定**。

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

请按照相同的过程来创建名为"学校 Test"的新 SQL Server Express School 数据库。

（要追加"Test"到这些数据库名称因为更高版本，你将创建开发环境中，每个数据库的其他实例，你需要能够区分这两组的数据库。）

**服务器资源管理器**现在将显示两个新数据库。

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>创建新的数据库授予脚本

当在 IIS 中在开发计算机上运行应用程序时，应用程序使用默认应用程序池的凭据访问数据库。 但是，默认情况下，应用程序池标识没有打开数据库的权限。 因此，你需要运行一个脚本来授予该权限。 在本部分中，你将创建你将运行更高版本，以确保它在 IIS 中运行时，该应用程序可以打开数据库的脚本。

在此解决方案的*SolutionFiles*中创建的文件夹[将部署到生产环境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教程中，创建一个名为的新 SQL 文件*Grant.sql*。 将以下 SQL 命令复制到文件，然后保存并关闭文件：

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> 此脚本用于处理与 SQL Server 2008 和 Windows 7 中的 IIS 设置，因为它们在本教程中指定。 如果在使用不同版本的 SQL Server 或的 Windows 中，或者如果你设置 IIS 在你的计算机上以不同的方式，可能需要对此脚本的更改。 有关 SQL Server 脚本的详细信息，请参阅[SQL Server 联机丛书](https://go.microsoft.com/fwlink/?LinkId=132511)。


> [!NOTE] 
> 
> **安全说明**此脚本将向提供 db\_程序在运行时，这是必须在生产环境中访问数据库的用户的所有者权限。 在某些情况下你可能想要指定具有完整的数据库架构更新权限仅对于部署，并指定运行时具有的权限仅读取和写入数据的不同用户的用户。 有关详细信息，请参阅**为 Code First 迁移查看自动 Web.config 更改**中[作为测试环境部署到 IIS](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)。


## <a name="configuring-database-deployment-for-the-test-environment"></a>配置测试环境的数据库部署

接下来，你将配置 Visual Studio，以便它将执行以下任务，以便每个数据库：

- 生成目标数据库中创建源数据库的结构 （表、 列、 约束等） 的 SQL 脚本。
- 生成将源数据库的数据插入到目标数据库中的表的 SQL 脚本。
- 运行生成的脚本和你创建目标数据库中授予脚本。

打开**项目属性**窗口，然后选择**打包/发布 SQL**选项卡。

请确保**活动 （发行版）**或**版本**中选择**配置**下拉列表。

单击**启用此页**。

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**打包/发布 SQL**选项卡通常被禁止，因为它指定旧部署方法。 对于大多数情况下，你应配置中的数据库部署**发布 Web**向导。 从 SQL Server Compact 迁移到 SQL Server 或 SQL Server Express 是一种特殊情况，此方法是一个不错的选择。

单击**导入从 Web.config**。

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio 查找连接字符串*Web.config*文件中，查找另一个用于成员资格数据库，另一个用于 School 数据库，并将行对应于在每个连接字符串添加**数据库条目**表。 它找到的连接字符串对于现有的 SQL Server Compact 数据库，并且在下一步就是要配置如何以及在何处部署这些数据库。

输入中的数据库部署设置**数据库项详细信息**下面部分**数据库条目**表。 中所示的设置**数据库项详细信息**部分适用于者为准行中**数据库条目**选择表，如下面的插图中所示。

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>成员资格数据库的的配置部署设置

选择**DefaultConnection 部署**行中**数据库条目**以便配置设置应用于成员资格数据库的表。

在**目标数据库的连接字符串**，输入指向新的 SQL Server Express 的成员资格数据库的连接字符串。 你可以从需要的连接字符串**服务器资源管理器**。 在**服务器资源管理器**，展开**数据连接**和选择**aspnetTest**数据库，然后从**属性**窗口复制**连接字符串**值。

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

相同的连接字符串将在此处重现：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

复制并粘贴到此连接字符串**目标数据库的连接字符串**中**打包/发布 SQL**选项卡。

请确保**抽取数据和/或从现有数据库的架构**选择。 这是什么因素会导致 SQL 脚本以自动生成并运行目标数据库中。

**源数据库的连接字符串**从提取值*Web.config*文件并指向开发 SQL Server Compact 数据库。 这是将用于生成将在目标数据库中更高版本运行的脚本的源数据库。 由于你想要部署的数据库的生产版本，将"aspnet Dev.sdf"变为"aspnet Prod.sdf"。

更改**数据库脚本选项**从**仅限架构**到**架构和数据**，因为你想要将数据 （用户帐户和角色） 复制以及数据库结构。

若要配置部署运行之前创建的授予脚本，你必须将其添加到**数据库脚本**部分。 单击**添加脚本**，然后在**添加 SQL 脚本**对话框框中，导航到存储授予脚本的文件夹 （这是包含解决方案文件的文件夹）。 选择名为的文件*Grant.sql*，然后单击**打开**。

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

有关设置**DefaultConnection 部署**行中**数据库条目**现在类似下图：

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School 数据库的的配置部署设置

接下来，选择**SchoolContext 部署**行中**数据库条目**以便配置部署设置的 School 数据库的表。

你可以使用你之前用来获取新的 SQL Server Express 数据库的连接字符串的相同方法。 复制到此连接字符串**目标数据库的连接字符串**中**打包/发布 SQL**选项卡。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

请确保**抽取数据和/或从现有数据库的架构**选择。

**源数据库的连接字符串**从提取值*Web.config*文件并指向开发 SQL Server Compact 数据库。 将"学校 Dev.sdf"更改为"学校-Prod.sdf"若要部署的数据库的生产版本。 (你将永远不会在应用程序中创建 School Prod.sdf 文件\_数据文件夹，因此你会将测试环境中的该文件复制到应用程序\_ContosoUniversity 项目文件夹更高版本中的数据文件夹。)

更改**数据库脚本选项**到**架构和数据**。

你还想要运行脚本来授予读取和写入应用程序池标识为此数据库的权限，因此添加*Grant.sql*用于成员资格数据库脚本文件。

完成后，设置**SchoolContext 部署**行中**数据库条目**类似下图：

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

保存对**打包/发布 SQL**选项卡。

复制*学校 Prod.sdf*文件从*c:\inetpub\wwwroot\ContosoUniversity\App\_数据*文件夹*应用\_数据*文件夹中ContosoUniversity 项目中。

### <a name="specifying-transacted-mode-for-the-grant-script"></a>为授予脚本指定事务的模式

在部署过程生成部署的数据库架构和数据的脚本。 默认情况下，这些脚本在事务中运行。 但是，默认情况下 （如授予脚本中） 的自定义脚本不运行在事务中。 如果在部署过程将混合事务模式，在部署过程中运行的脚本时可能会超时错误。 在此部分中，若要配置自定义的脚本来运行在事务中编辑项目文件。

在**解决方案资源管理器**，右键单击**ContosoUniversity**项目，然后选择**卸载项目**。

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

然后再次右键单击项目并选择**编辑 ContosoUniversity.csproj**。

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Visual Studio 编辑器中显示的项目文件的 XML 内容。 请注意，有几条`PropertyGroup`元素。 (在图中，内容`PropertyGroup`元素已被省略。)

![项目文件编辑器窗口](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

第一个，未包含任何`Condition`属性，是适用于而不考虑应用设置生成配置。 一个`PropertyGroup`元素仅适用于调试生成配置 (请注意`Condition`属性)、 一个仅适用于发布生成配置，并且一个仅适用于测试生成配置。 在`PropertyGroup`适用于发布生成配置的元素，你将看到`PublishDatabaseSettings`输入包含的设置的元素**打包/发布 SQL**选项卡。没有`Object`对应于每个授权脚本的元素的指定的 （请注意这两个实例"Grant.sql"）。 默认情况下，`Transacted`属性`Source`元素为每个授予脚本`False`。

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

更改的值`Transacted`属性`Source`元素`True`。

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

保存并关闭项目文件中，然后右键单击中的项目**解决方案资源管理器**和选择**重新加载项目**。

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>为连接字符串设置 Web.Config 转换

连接字符串为新的 SQL Express 数据库输入**打包/发布 SQL**选项卡上通过 Web 部署仅用于在部署过程中更新目标数据库。 你仍必须设置*Web.config*转换，以便部署中的连接字符串*Web.config*文件指向新的 SQL Server Express 数据库。 (当你使用**打包/发布 SQL**选项卡上，你无法在发布配置文件中配置连接字符串。)

打开*Web.Test.config*和替换`connectionStrings`具有元素`connectionStrings`下面的示例中的元素。 （请确保仅复制 connectionStrings 元素，而不是如下所示提供上下文的周围的代码。）

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

此代码会导致`connectionString`和`providerName`每个属性`add`要被替换元素在部署*Web.config*文件。 这些连接字符串不是你在中输入相同**打包/发布 SQL**选项卡。设置"MultipleActiveResultSets = True"具有已添加到它们，因为它具有所需的实体框架和通用的提供程序。

## <a name="installing-sql-server-compact"></a>安装 SQL Server Compact

SqlServerCompact NuGet 包提供 SQL Server Compact 数据库 Contoso 大学应用程序引擎集。 但现在它不是应用程序，但 Web 部署，必须能够读取 SQL Server Compact 数据库中，若要创建在 SQL Server 数据库中运行脚本。 若要启用 Web 部署来读取 SQL Server Compact 数据库，安装 SQL Server Compact 在开发计算机上使用以下链接： [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)。

## <a name="deploying-to-the-test-environment"></a>将部署到测试环境

若要发布到测试环境，你必须创建发布配置文件配置为使用**打包/发布 SQL**数据库而不发布配置文件数据库设置发布的选项卡。

首先，删除现有的测试配置文件。

在**解决方案资源管理器**，右键单击 ContosoUniversity 项目中，然后单击**发布**。

选择**配置文件**选项卡。

单击**管理配置文件**。

选择**测试**，单击**删除**，然后单击**关闭**。

关闭**发布 Web**向导以保存此更改。

接下来，创建新的测试配置文件，并使用它来发布该项目。

在**解决方案资源管理器**，右键单击 ContosoUniversity 项目中，然后单击**发布**。

选择**配置文件**选项卡。

选择**&lt;新...&gt;** 从下拉列表，并输入"Test"作为配置文件名称。

在**服务 URL**框中，输入*localhost*。

在**站点/应用程序**框中，输入*默认网站/ContosoUniversity*。

在**目标 URL**框中，输入`http://localhost/ContosoUniversity/`。

单击 **“下一步”**。

**设置**选项卡对话框将警告你，**打包/发布 SQL**已配置选项卡上，并且它使你能够重写它们通过单击启用新的数据库发布改进。 为此部署，你不想重写**打包/发布 SQL**选项卡上设置，因此只需单击**下一步**。

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

在上的一条消息**预览**选项卡指示**未选择的数据库发布**，但这仅意味着发布配置文件中未配置了数据库发布。

单击“发布” 。

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio 部署应用程序，并在测试环境中打开浏览器向该站点的主页页。 运行教师页后，可以看到，它会显示你此前看到的相同数据。 运行**添加学生**页上，添加一个新的学生，，然后查看在新的学生**学生**页。 这将验证你可以更新数据库。 选择**更新信用额度**（将需要登录） 的页以验证部署成员资格数据库并有权访问它。

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>为生产环境中创建 SQL Server 数据库

现在，你已将部署到测试环境中，你已准备好设置部署到生产环境。 在开始就像为测试环境中，通过创建将部署到的数据库。 从概览，您还记得，Cytanium Lite 托管计划将仅允许单个 SQL Server 数据库，因此你将设置只有一个数据库，不是两个。 所有表和数据从成员资格和学校 SQL Server Compact 数据库将部署到生产中的一个 SQL Server 数据库。

转到 Cytanium 控件面板[http://panel.cytanium.com](http://panel.cytanium.com)。将鼠标悬停**数据库**，然后单击**SQL Server 2008**。

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

在**SQL Server 2008**页上，单击**Create Database**。

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

将数据库"School"，然后单击**保存**。 （页上会自动添加前缀"contosou"，因此，有效的名称将是"contosouSchool"。）

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

在同一页上，单击**Create User**。 上 Cytanium 的服务器，而不是使用 Windows 集成的安全性，并让打开你的数据库的应用程序池标识，你将创建有权打开你的数据库用户。 你将向转在生产环境中的连接字符串添加用户的凭据*Web.config*文件。 在此步骤中，你将创建这些凭据。

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

填写必填字段**SQL 用户属性**页：

- 输入"ContosoUniversityUser"作为名称。
- 输入密码。
- 选择**contosouSchool**作为默认数据库。
- 选择**contosouSchool**复选框。

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>配置数据库部署生产环境

现在你已准备好设置中的数据库部署设置**打包/发布 SQL**选项卡上，如你此前为测试环境。

打开**项目属性**窗口中，选择**打包/发布 SQL**选项卡上，并确保**活动 （发行版）**或**版本**是在所选**配置**下拉列表。

在配置每个数据库的部署设置时，你的生产和测试环境所执行的操作的主要区别是你如何配置连接字符串中。 为测试环境中输入不同的目标数据库连接字符串，但对于生产环境目标连接字符串将为这两个数据库相同。 这是因为这两个数据库部署到生产中的一个数据库。

### <a name="configuring-deployment-settings-for-the-membership-database"></a>成员资格数据库的的配置部署设置

若要配置适用于成员资格数据库的设置，请选择**DefaultConnection 部署**行中**数据库条目**表。

在**目标数据库的连接字符串**，输入指向刚创建的新的生产 SQL Server 数据库的连接字符串。 你可以从你的欢迎电子邮件获取连接字符串。 电子邮件的相关部分包含以下示例连接字符串：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

将三个变量之后，你需要的连接字符串如下所如下例所示：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

复制并粘贴到此连接字符串**目标数据库的连接字符串**中**打包/发布 SQL**选项卡。

请确保**抽取数据和/或从现有数据库的架构**仍然选择，且**数据库脚本选项**仍**架构和数据**。

在**数据库脚本**框中，清除 Grant.sql 脚本旁边的复选框。

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School 数据库的的配置部署设置

接下来，选择**SchoolContext 部署**行中**数据库条目**以便配置 School 数据库设置的表。

将复制到相同的连接字符串**目标数据库的连接字符串**复制到此字段中的成员资格数据库。

请确保**抽取数据和/或从现有数据库的架构**仍然选择，且**数据库脚本选项**仍**架构和数据**。

在**数据库脚本**框中，清除 Grant.sql 脚本旁边的复选框。

保存对**打包/发布 SQL**选项卡。

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>设置 Web.Config 转换到生产数据库的连接字符串

接下来，你将设置*Web.config*转换，以便部署中的连接字符串*Web.config*文件以指向新的生产数据库。 在输入的连接字符串**打包/发布 SQL**用于 Web 部署使用的选项卡是与该应用程序需要使用除添加相同`MultipleResultSets`选项。

打开*Web.Production.config*和替换`connectionStrings`具有元素`connectionStrings`如以下示例所示的元素。 (仅复制`connectionStrings`元素中，不提供以显示的上下文的周围的标记。)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

有时，您看到告诉你始终加密连接字符串中的建议*Web.config*文件。 这可能适合如果你在部署到你自己的公司网络上的服务器。 时将部署到共享宿主环境，不过，要信任的托管提供商的安全做法，但不必要的或实际加密的连接字符串。

## <a name="deploying-to-the-production-environment"></a>将部署到生产环境

现在你已准备好部署到生产环境。 Web 部署将读取你的项目中的 SQL Server Compact 数据库*应用\_数据*文件夹并重新创建的所有表和生产 SQL Server 数据库中的数据。 若要发布使用**打包/发布 Web**选项卡上设置，你必须创建新的发布配置文件用于生产。

首先，删除现有的生产配置文件，如之前所做的测试配置文件。

在**解决方案资源管理器**，右键单击 ContosoUniversity 项目中，然后单击**发布**。

选择**配置文件**选项卡。

单击**管理配置文件**。

选择**生产**，单击**删除**，然后单击**关闭**。

关闭**发布 Web**向导以保存此更改。

接下来，创建新的生产配置文件，并使用它来发布该项目。

在**解决方案资源管理器**，右键单击 ContosoUniversity 项目中，然后单击**发布**。

选择**配置文件**选项卡。

单击**导入**，然后选择前面下载的.publishsettings 文件。

上**连接**选项卡上，更改**目标 URL**为正确的临时 URL，这在此示例中是 http://contosouniversity.com.vserver01.cytanium.com。

将配置文件重命名为生产。 (选择**配置文件**选项卡，单击**管理配置文件**要这样做)。

关闭**发布 Web**向导保存所做的更改。

在实际应用中在其中数据库已更新到生产中，你将执行两个附加步骤现在发布之前：

1. 上载*应用\_offline.htm*中, 所示[将部署到生产环境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教程。
2. 使用**文件管理器**Cytanium 控制面板复制功能*aspnet Prod.sdf*和*学校 Prod.sdf*文件从生产站点到*应用\_数据*ContosoUniversity 项目文件夹中的。 这可确保你要部署到新的 SQL Server 数据库的数据包括由你的生产网站所做的最新更新。

在**Web 单键发布**工具栏上，请确保**生产**配置文件已选择，，然后单击**发布**。

如果你已上载*应用\_offline.htm*在发布前，你必须使用**文件管理器**实用工具删除 Cytanium 控制面板中*应用\_脱机。*在测试前 htm。 你还可以在同一时间删除*.sdf*文件从*应用\_数据*文件夹。

你现在可以打开浏览器并转到您的个人网站 URL，测试应用程序部署到测试环境之后未相同的方式。

## <a name="switching-to-sql-server-express-localdb-in-development"></a>切换到在开发过程中的 SQL Server Express LocalDB

如已概述中所述，则通常最好在测试和生产中使用的开发中使用相同的数据库引擎。 （记住在开发过程中使用 SQL Server Express 的优点是，数据库的工作方式将你的开发、 测试和生产环境中）。在本节中你将设置 ContosoUniversity 项目时要使用 SQL Server Express LocalDB 从 Visual Studio 运行应用程序。

执行此迁移的最简单方法是让 Code First 和成员资格系统为你创建这两个新的开发数据库。 使用此方法将迁移需要三个步骤：

1. 更改连接字符串，以指定新的 SQL Express LocalDB 数据库。
2. 运行网站管理工具以创建管理员用户。 这将创建成员资格数据库。
3. 使用 Code First 迁移更新数据库命令创建并设定种子的应用程序数据库。

### <a name="updating-connection-strings-in-the-webconfig-file"></a>更新 Web.config 文件中的连接字符串

打开*Web.config*文件并将`connectionStrings`元素替换为以下代码：

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>创建成员资格数据库

在**解决方案资源管理器**，选择 ContosoUniversity 项目，然后单击**ASP.NET 配置**中**项目**菜单。

选择安全选项卡。

单击**创建或管理角色**，然后创建**管理员**角色。

返回到安全选项卡。

单击**创建用户**，然后选择**管理员**复选框，并创建名为管理员的用户

关闭**网站管理工具**。

### <a name="creating-the-school-database"></a>创建 School 数据库

打开程序包管理器控制台窗口。

在**默认项目**下拉列表中，选择 ContosoUniversity.DAL 项目。

输入以下命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First 迁移应用初始迁移，创建数据库，然后将应用 AddBirthDate 迁移，然后运行 Seed 方法。

通过按控件 F5 运行该站点。 如所做的测试和生产环境，运行**添加学生**页上，添加一个新的学生，，然后查看在新的学生**学生**页。 这将验证 School 数据库已创建并初始化和你已阅读并向其中写入访问。

选择**更新信用额度**页上，然后登录以验证程序已部署成员资格数据库并有权访问它。 如果你未迁移你的用户帐户，创建管理员帐户，然后选择**更新信用额度**页后，可以验证其是否工作。

## <a name="cleaning-up-sql-server-compact-files"></a>清理 SQL Server Compact 文件

你不再需要的文件和已包含至 SQL Server Compact 的 NuGet 包。 如果你想 （此步骤不需要），你可以清理不需要的文件和引用。

在**解决方案资源管理器**，删除*.sdf*文件从*应用\_数据*文件夹和*amd64*和*x86*文件夹从*bin*文件夹。

在**解决方案资源管理器**，右键单击该解决方案 （不是一种项目），，然后单击**管理解决方案的 NuGet 包**。

在左窗格中**管理 NuGet 包**对话框中，选择**安装包**。

选择**EntityFramework.SqlServerCompact**打包并单击**管理**。

在**选择项目**对话框中，选择这两个项目。 若要卸载这两个项目中的包，请清除这两个复选框，然后单击**确定**。

在询问你是否想要卸载依赖的包还对话框中，单击否。 以下方法之一是你需要保留的实体框架包。

请按照相同的过程来卸载**SqlServerCompact**包。 (必须按此顺序卸载包，因为**EntityFramework.SqlServerCompact**程序包依赖于**SqlServerCompact**包。)

你现在成功迁移到 SQL Server Express 和完整的 SQL Server。 在下一个教程，你将使另一个数据库的行为更改，并且你将了解如何将数据库更改部署时你测试和生产数据库使用 SQL Server Express 和完整的 SQL Server。

>[!div class="step-by-step"]
[上一页](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[下一页](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
