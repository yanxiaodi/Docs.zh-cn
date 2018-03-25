---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: 自定义数据库部署为多个环境 |Microsoft 文档
author: jrjlee
description: 本主题介绍如何部署过程的一部分定制到特定目标环境数据库的属性。 注意： 本主题假定 th...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: f3ca344c2466d9d538f55cd8ff0a5bf5b7bac808
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="customizing-database-deployments-for-multiple-environments"></a>多个环境的的自定义数据库部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何部署过程的一部分定制到特定目标环境数据库的属性。
> 
> > [!NOTE]
> > 本主题假定你要部署使用 MSBuild.exe 和 VSDBCMD.exe 的 Visual Studio 2010 数据库项目。 为什么你可能选择这种方法的详细信息，请参阅[企业中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)和[部署数据库项目](../web-deployment-in-the-enterprise/deploying-database-projects.md)。
> 
> 
> 在数据库项目部署到多个目标时，通常需要自定义每个目标环境的数据库部署属性。 例如，在测试环境中你将通常重新创建数据库上每个部署，而在过渡环境或生产环境中将是大量更有可能进行增量更新，以保留数据。
> 
> 在 Visual Studio 2010 数据库项目中，部署设置包含在一个部署配置 (.sqldeployment) 文件。 本主题将演示如何创建特定于环境的部署配置文件，指定你想要使用作为 VSDBCMD 参数。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="task-overview"></a>任务概述

本主题假定：

- 中所述使用解决方案部署的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。
- 中所述从项目文件以部署数据库项目时，调用 VSDBCMD[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

若要创建支持改变目标环境之间的数据库部署属性的部署系统，你将需要：

- 创建每个目标环境的部署配置 (.sqldeployment) 文件。
- 创建用于部署配置文件指定为命令行开关的 VSDBCMD 命令。
- 参数化 VSDBCMD 命令在 Microsoft Build Engine (MSBuild) 项目文件中，以便适合于目标环境的 VSDBCMD 选项。

本主题将演示如何执行每个这些过程。

## <a name="creating-environment-specific-deployment-configuration-files"></a>创建特定于环境的部署配置文件

默认情况下，数据库项目包含一个名为的单个部署配置文件*Database.sqldeployment*。 如果你在 Visual Studio 2010 中打开此文件，你可以看到向你提供的不同的部署选项：

- **部署比较排序规则**。 这样就可以选择是否要使用你的项目的数据库排序规则 (*源*排序规则) 或你的目标服务器的数据库排序规则 (*目标*排序规则)。 在大多数情况下，你将想要使用源的排序规则，当你将部署到一个开发或测试环境。 当你将部署到过渡或生产环境时，通常将想要保留不变以避免任何互操作性问题的目标排序规则。
- **部署数据库属性**。 这样就可以选择是否要应用的数据库属性中中, 定义*Database.sqlsettings*文件。 当您首次部署数据库时，你应该部署数据库属性。 如果你在更新现有数据库，属性应已就位，并且需要不应将它们重新部署。
- **始终重新创建数据库**。 这样，你选择是否要重新创建目标数据库，每次部署或您的架构进行增量更改，以使目标数据库的最新。 如果重新创建该数据库，你将丢失现有的数据库中的任何数据。 在这种情况下，你应通常设置为**false**部署到过渡环境或生产环境。
- **如果可能发生数据丢失则阻止增量部署**。 这允许你选择部署是否应停止是否对数据库架构的更改会导致数据丢失。 你通常将其设置为**true**以部署到生产环境中，以便为你提供机会介入和保护任何重要数据。 如果设置了**始终重新创建数据库**到**false**，此设置将产生任何影响。
- **在单用户模式下执行部署**。 这通常不开发或测试环境中的问题。 但是，你应通常设置为**true**部署到过渡环境或生产环境。 这可以防止用户进行部署时，对数据库进行更改。
- **备份数据库之前部署**。 你通常将其设置为**true**当你将部署到生产环境中，为防止数据丢失预防措施。 你可能还想要将其设置为**true**时你将部署到过渡环境，如果临时数据库中包含大量数据。
- **生成的对象在目标数据库中但未在数据库项目中的 DROP 语句**。 在大多数情况下，这是对数据库进行增量更改的整数和最重要的部分。 如果设置了**始终重新创建数据库**到**false**，此设置将产生任何影响。
- **不使用 ALTER ASSEMBLY 语句更新 CLR 类型**。 此设置确定如何 SQL Server 应更新公共语言运行时 (CLR) 类型，程序集的较新版本。 此属性应设置为**false**在大多数情况下。

此表显示不同的目标环境的典型的部署设置。 但是，你的设置可能不同，具体取决于你的确切需求。

|  | Developer/Test | 过渡/集成 | 生产 |
| --- | --- | --- | --- |
| **部署比较排序规则** | 源 | 目标 | 目标 |
| **部署数据库属性** | True | 仅在第一次 | 仅在第一次 |
| **始终重新创建数据库** | True | False | False |
| **如果可能发生数据丢失则，阻止增量部署** | False | 可能 | True |
| **在单用户模式下执行部署脚本** | False | True | True |
| **部署前备份数据库** | False | 可能 | True |
| **生成 DROP 语句的目标数据库中的对象但不是在数据库项目** | False | True | True |
| **不使用 ALTER ASSEMBLY 语句更新 CLR 类型** | False | False | False |
  

> [!NOTE]
> 数据库部署属性和环境注意事项的详细信息，请参阅[数据库项目设置概述](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)，[如何： 部署详细信息的配置属性](https://msdn.microsoft.com/library/dd172125.aspx)， [生成并将数据库部署到独立的开发环境中](https://msdn.microsoft.com/library/dd193409.aspx)，和[生成并将数据库部署到过渡或生产环境](https://msdn.microsoft.com/library/dd193413.aspx)。


若要支持部署到多个目标数据库项目，应创建每个目标环境的部署配置的文件。

**若要创建特定于环境的配置文件**

1. 在 Visual Studio 2010 中，在**解决方案资源管理器**窗口中，右键单击您的数据库项目，并依次**属性**。
2. 在数据库项目的属性页上，在**部署**选项卡上，在**部署配置文件**行中，单击**新建**。

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. 在**新部署配置文件**对话框框中，为文件提供有意义的名称 (例如， **TestEnvironment.sqldeployment**)，然后单击**保存**。
4. 上 *[文件名]***.sqldeployment** 页上，设置部署属性以匹配你的目标环境的要求，然后保存该文件。

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. 注意，新的文件会添加到您的数据库项目中的属性文件夹。

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>在 VSDBCMD 中指定的部署配置文件

当使用 Visual Studio 2010 中 （如调试和发布） 的解决方案配置时，你可以将部署配置文件关联的每个配置。 生成一个特定的配置时，生成过程将生成指向配置特定部署配置文件配置特定部署清单文件。 但是，这些教程中所述的部署的主要目标之一是方法的使用户能够控制部署过程，而使用 Visual Studio 2010 和解决方案配置。 在此方法中，将解决方案配置无论是相同的目标部署环境。 若要定制您的数据库部署到特定目标环境，可以使用 VSDBCMD 命令行选项指定你的部署配置文件。

若要在你 VSDBCMD 中指定一个部署配置文件，使用**p: / DeploymentConfigurationFile**切换，并提供你的文件的完整路径。 这将覆盖部署清单标识的部署配置文件。 例如，你可以使用此 VSDBCMD 命令部署**ContactManager**数据库添加到测试环境：


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> 请注意，生成过程可能会重命名.sqldeployment 文件时将文件复制到输出目录。


如果在部署前或后部署 SQL 脚本中使用的 SQL 命令变量，可以使用类似的方法与部署关联的特定于环境的.sqlcmdvars 文件。 在这种情况下，使用**p: / SqlCommandVariablesFile**开关来标识.sqlcmdvars 文件。

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>从 MSBuild 项目文件运行 VSDBCMD 命令

你可以通过调用 MSBuild 项目文件中的 VSDBCMD 命令**Exec** MSBuild 目标内的任务。 在最简单的形式，它将如下所示：


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- 在实践中，若要使你的项目文件易于读取和重复使用，你需要创建用于存储的各种命令行参数的属性。 这样会使用户能够提供特定于环境的项目文件中的属性值，或重写从 MSBuild 命令行的默认值更容易。 如果你使用拆分项目文件方法中所述[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，应相应地划分你构建说明和两个文件之间的属性：
- 特定于环境的设置，例如部署配置文件名、 数据库连接字符串和目标数据库名称，应该在特定于环境的项目文件中。
- 运行 VSDBCMD 命令，以及任何通用的属性，例如 VSDBCMD 可执行文件的位置的 MSBuild 目标应放在通用项目文件中。

你还应确保生成数据库项目，然后调用 VSDBCMD 以便.deploymanifest 文件已创建，并可供使用。 你可以看到这种方法主题中的完整示例[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，这将指导你完成中的项目文件[联系人管理器示例解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)。

## <a name="conclusion"></a>结束语

本主题介绍如何部署数据库项目使用 MSBuild 和 VSDBCMD 时定制到不同的目标环境的数据库属性。 当你需要将数据库项目部署为更大，企业级解决方案的一部分时，此方法非常有用。 这些解决方案通常部署到多个目标，如沙盒开发或测试环境、 过渡或集成平台，和生产或实时环境。 每个这些目标环境通常需要一组唯一的数据库部署属性。

## <a name="further-reading"></a>其他阅读材料

有关部署使用 VSDBCMD.exe 的数据库项目的详细信息，请参阅[部署数据库项目](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 使用自定义 MSBuild 项目文件来控制部署过程的详细信息，请参阅[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

MSDN 上的这些文章提供有关数据库部署的更多常规指导：

- [数据库项目设置的概述](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [如何： 配置的部署详细信息属性](https://msdn.microsoft.com/library/dd172125.aspx)
- [生成并部署到独立的开发环境中的数据库](https://msdn.microsoft.com/library/dd193409.aspx)
- [生成并将数据库部署到过渡或生产环境](https://msdn.microsoft.com/library/dd193413.aspx)

>[!div class="step-by-step"]
[上一页](performing-a-what-if-deployment.md)
[下一页](deploying-database-role-memberships-to-test-environments.md)
