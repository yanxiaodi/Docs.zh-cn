---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: "部署数据库项目 |Microsoft 文档"
author: jrjlee
description: "注意： 在许多企业部署方案，你需要向已部署的数据库发布增量更新的能力。 替代方法是重新创建..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 9b1f9a19c76e33b5d996cb4d562cf0c1a3e2f83b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="deploying-database-projects"></a>部署数据库项目
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> 在很多企业部署方案，你需要能够将增量更新发布到已部署的数据库。 替代方法是重新创建每个部署，这意味着你丢失现有的数据库中的任何数据上的数据库。 当使用 Visual Studio 2010 时，使用 VSDBCMD 是增量数据库发布到建议的方法。 但是下, 一版本的 Visual Studio 和 Web 发布管道 (WPP) 将包含支持直接发布增量的工具。


如果你在 Visual Studio 2010 中打开的联系人管理器示例解决方案，你将看到数据库项目包括一个包含四个文件的属性文件夹。

![](deploying-database-projects/_static/image1.png)

以及项目文件 (*ContactManager.Database.dbproj*在这种情况下)，这些文件控制的生成和部署过程的各个方面：

- *Database.sqlcmdvars*文件在部署项目时使用任何 SQLCMD 变量提供值。 每个解决方案配置 （例如，调试和发布） 可以指定不同.sqlcmdvars 文件。
- *Database.sqldeployment*文件提供了特定于部署的设置，例如是否使用在你的项目或目标服务器的排序规则中定义的排序规则是否重新创建目标数据库每个时间，或只需修改现有数据库，以便使其最新，依次类推。 每个解决方案配置时，可以指定不同.sqldeployment 文件。
- *Database.sqlpermissions*文件是一个 XML 文档，可用于定义你想要添加到目标数据库的任何权限。 所有的解决方案配置共享相同的.sqlpermissions 文件。
- *Database.sqlsettings*文件指定要在创建数据库，如使用的比较运算符的行为等的排序规则时使用的数据库级别属性。 所有的解决方案配置共享相同的.sqlsettings 文件。

值得花一段时间来在 Visual Studio 中打开这些文件并自行熟悉内容。

生成数据库项目时，生成过程将创建两个文件：

- A*数据库架构*（.dbschema 文件）。 描述了你想要以 XML 格式创建数据库的架构。
- A*部署清单*（.deploymanifest 文件）。 这包含创建和部署你的数据库所需的所有信息。 因为它引用.dbschema 文件以及其他资源，如部署说明 （.sqldeployment 文件） 和任何部署前或后部署 SQL 脚本。

这将显示这些资源之间的关系：

![](deploying-database-projects/_static/image2.png)

如你所见，.sqlsettings 文件和.sqlpermissions 文件将是生成过程的输入。 数据库项目文件中，以及这些文件用于创建数据库架构文件。 .Sqldeployment 文件和.sqlcmdvars 文件通过生成过程保持不变。 部署清单指示数据库架构、.sqldeployment 文件、.sqlcmdvars 文件和任何预部署脚本或后期部署 SQL 脚本的位置。

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>为什么使用 VSDBCMD 来部署数据库项目？

有各种不同方法来部署数据库项目。 但是，并非所有都适用于将数据库项目部署到企业环境中的远程服务器。 请考虑需要从一个数据库项目部署。 在企业部署方案中，你可能希望：

- 部署数据库项目从远程位置的功能。
- 能够对现有数据库进行增量更新。
- 能够包括部署前脚本或后期部署脚本。
- 能够定制向多个目标环境的部署。
- 部署数据库项目中的一个更大一部分的功能通常编写脚本，单步执行解决方案部署。

有三种主要方法可用于部署数据库项目：

- 部署功能可以使用 Visual Studio 2010 中的数据库项目类型。 当生成和部署 Visual Studio 2010 中的数据库项目时，在部署过程将使用部署清单来生成特定于生成配置的基于 SQL 的部署文件。 如果它不存在，或如果它已存在对数据库进行任何必要的更改，这将创建数据库。 你可以使用 SQLCMD.exe 来运行此文件在目标服务器上，或者你可以设置 Visual Studio 创建并运行该文件。 此方法的缺点是仅具有有限的部署设置控制。 通常还可能需要修改 SQL 部署文件，以提供特定于环境的变量值。 使用 Visual Studio 2010 安装，只能使用此方法从计算机和开发人员需要了解并提供所有目标环境的连接字符串和凭据。
- 你可以使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 到[将数据库部署作为 web 应用程序项目的一部分](https://msdn.microsoft.com/library/dd465343.aspx)。 但是，这种方法是很多更复杂，如果你想要部署数据库项目，而不是只需复制的目标服务器上的现有本地数据库。 你可以配置 Web 部署运行 SQL 部署脚本将生成数据库项目，但为了执行此操作，你需要创建你的 web 应用程序项目的自定义 WPP 目标文件。 这将在部署过程添加大量的复杂性。 此外，Web 部署不直接支持增量更新到现有数据库。 此方法的详细信息，请参阅[扩展为包数据库项目的 Web 发布管道部署 SQL 文件](https://go.microsoft.com/?linkid=9805121)。
- VSDBCMD 实用程序可用于部署数据库，使用的数据库架构或部署清单。 你可以从 MSBuild 目标，这样就可以作为更大、 脚本化部署过程的一部分发布数据库调用 VSDBCMD.exe。 您可以重写.sqlcmdvars 文件和大量 VSDBCMD 命令，从而允许您自你的部署不同的环境定义而无需创建多个生成配置从其他数据库属性中的变量。 VSDBCMD 提供区分功能，这意味着它将仅对齐具有你的数据库架构的目标数据库的必要更改。 VSDBCMD 还提供范围广泛的命令行选项，这让你对部署过程细化的控制。

从本概述中，你可以看到，用 MSBuild 使用 VSDBCMD 是最适合与典型的企业部署方案的方法：

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| 支持远程部署？ | 是 | 是 | 是 |
| 支持增量更新？ | 是 | No | 是 |
| 支持前/后部署脚本？ | 是 | 是 | 是 |
| 支持多环境部署？ | 有限 | 有限 | 是 |
| 支持脚本化的部署？ | 有限 | 是 | 是 |

此主题的其余部分介绍使用 MSBuild 来部署数据库项目的 VSDBCMD 的用法。

## <a name="understanding-the-deployment-process"></a>了解部署过程

VSDBCMD 实用程序可让你部署使用的数据库架构 （.dbschema 文件） 或部署清单 （.deploymanifest 文件） 的数据库。 在实践中，你几乎总是将使用部署清单中，如部署清单允许您为各种部署属性提供默认值并确定你想要运行任何部署前或后部署 SQL 脚本。 例如，此 VSDBCMD 命令用于部署**ContactManager**数据库添加到测试环境中的数据库服务器：


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


这种情况下：

- **/A** (或**/Action**) 开关可指定希望 VSDBCMD 执行。 你可以将此设置为**导入**或**部署**。 **导入**选项用于从现有数据库，生成.dbschema 文件和**部署**选项用于将.dbschema 文件部署到目标数据库。
- **/清单**(或**/ManifestFile**) 交换机标识你想要部署的.deploymanifest 文件。 如果你想要改为使用.dbschema 文件，则应使用**/模型**(或**/ModelFile**) 切换。
- **/Cs** (或**/ConnectionString**) 交换机提供对目标数据库服务器的连接字符串。 请注意这不包括数据库和 #x 2014; 的名称VSDBCMD 需要连接到服务器以创建该数据库。它不需要为单个的数据库连接。 如果.deploymanifest 文件包含连接字符串，则可以省略此开关。 如果你仍使用开关，开关值将覆盖.deploymanifest 值。
- **/P:TargetDatabase**属性提供你想要分配到目标数据库上创建的名称。 此设置的值将替代**TargetDatabase** .deploymanifest 文件中的属性。 你可以使用**/p:** *[属性名称]*.sqlcmdvars 文件中声明的语法来设置部署属性的各种和重写任何 SQLCMD 变量。
- **/Dd+** (或**/DeployToDatabase+**) 开关指示你想要创建部署并将其部署到目标环境。 如果指定**/dd-**，或省略开关，VSDBCMD 将生成部署脚本，但将不将其部署到目标环境。 此开关通常是混淆的源，并在下一部分中的更多详细信息中所述。
- **/** (或**/DeploymentScriptFile**) 开关指定你想要生成部署脚本。 此值不会影响部署过程。

VSDBCMD 的详细信息，请参阅[VSDBCMD 的命令行参考。EXE （部署和架构导入）](https://msdn.microsoft.com/library/dd193283.aspx)和[如何： 准备数据库以进行部署的命令提示符下使用 VSDBCMD。EXE](https://msdn.microsoft.com/library/dd193258.aspx)。

有关如何使用 VSDBCMD 从 MSBuild 项目文件的示例，请参阅[了解该生成过程](understanding-the-build-process.md)。 有关如何配置数据库部署设置为多个环境的示例，请参阅[为多个环境自定义数据库部署](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。

## <a name="understanding-the-deploytodatabase-switch"></a>了解 DeployToDatabase 交换机

行为**/dd**或**/DeployToDatabase**交换机取决于是否使用 VSDBCMD.dbschema 文件或.deploymanifest 文件。 如果你使用.dbschema 文件，则行为是非常简单：

- 如果指定**/dd+**或**/dd**，VSDBCMD 将生成部署脚本并部署数据库。
- 如果指定**/dd-**或省略开关，VSDBCMD 将生成部署脚本。

如果你正在使用.deploymanifest 文件，则行为将是大量更复杂。 这是因为.deploymanifest 文件包含属性名称**DeployToDatabase** ，它还确定是否部署数据库。


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


根据数据库项目的属性设置此属性的值。 如果你设置**部署操作**到**创建部署脚本 (.sql)**，则值将是**False**。 如果你设置**部署操作**到**创建部署脚本 (.sql) 并将其部署到数据库**，则值将是**True**。

> [!NOTE]
> 这些设置是与特定的生成配置和平台相关联。 例如，如果你配置设置**调试**配置，然后将发布使用**版本**配置，不会使用你的设置。


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> 在此方案中，**部署操作**应始终设置为**创建部署脚本 (.sql)**，这是因为你不希望将数据库部署的 Visual Studio 2010。 换而言之， **DeployToDatabase**属性应始终为**False**。


当**DeployToDatabase**指定属性，则**/dd**交换机仅将覆盖属性，如果属性值是**false**:

- 如果**DeployToDatabase**属性是**False**，并且你指定**/dd+**或**/dd**，VSDBCMD 将重写**DeployToDatabase**属性并将其部署数据库。
- 如果**DeployToDatabase**属性是**False**，并且你指定**/dd-**或省略开关，VSDBCMD 不会将部署数据库。
- 如果**DeployToDatabase**属性是**True**，VSDBCMD 将忽略该交换机并将数据库部署。
- 部署脚本将在每个情况下，而不考虑是否要部署的数据库以及生成。

## <a name="conclusion"></a>结束语

本主题提供有关 Visual Studio 2010 中的数据库项目的生成和部署过程的概述。 它还描述了如何使用 VSDBCMD.exe 用 MSBuild 来支持企业级数据库部署。

这在实践中的工作方式的详细信息，请参阅[为多个环境自定义数据库部署](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。

## <a name="further-reading"></a>其他阅读材料

有关如何通过创建单独的部署配置文件为每个环境自定义数据库部署的信息，请参阅[为多个环境自定义数据库部署](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。 有关如何配置通过运行后期部署脚本的数据库角色成员身份的指南，请参阅[到测试环境中部署数据库角色成员](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。 有关管理一些独特的挑战该成员身份的指导数据库施加，请参阅[将成员资格数据库部署到企业环境](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

MSDN 上的以下主题提供更广泛的指导和 Visual Studio 数据库项目的背景信息和数据库部署过程：

- [Visual Studio 2010 SQL Server 数据库项目](https://msdn.microsoft.com/library/ff678491.aspx)
- [管理数据库的行为更改](https://msdn.microsoft.com/library/aa833404.aspx)
- [如何： 准备数据库以进行部署的命令提示符下使用 VSDBCMD。EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [数据库生成和部署的概述](https://msdn.microsoft.com/library/aa833165.aspx)

>[!div class="step-by-step"]
[上一页](deploying-web-packages.md)
[下一页](creating-and-running-a-deployment-command-file.md)
