---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: "了解生成过程 |Microsoft 文档"
author: jrjlee
description: "本主题提供的企业范围内生成和部署过程的演练。 本主题中介绍的方法使用自定义 Microsoft 生成 Engin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 3efcefc40dc135ff42f55911036f8b38b5aa13b1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="understanding-the-build-process"></a>了解生成过程
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题提供的企业范围内生成和部署过程的演练。 本主题中介绍的方法使用自定义 Microsoft Build Engine (MSBuild) 项目文件来提供细粒度控制过程的各个方面。 在项目文件中，自定义 MSBuild 目标用于运行 Internet Information Services (IIS) Web 部署工具 (MSDeploy.exe) 类似的部署实用程序和数据库部署实用工具 VSDBCMD.exe。
> 
> > [!NOTE]
> > 前一个主题，[了解项目文件](understanding-the-project-file.md)、 所述的 MSBuild 项目文件的关键组件和引入拆分项目文件，以支持部署到多个目标环境的概念。 如果你尚不熟悉这些概念，你应查看[了解项目文件](understanding-the-project-file.md)通读本主题工作之前。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="build-and-deployment-overview"></a>生成和部署概述

在[联系人管理器解决方案](the-contact-manager-solution.md)，三个文件控制的生成和部署过程：

- A*通用项目文件*(*Publish.proj*)。 这包含不会更改目标环境之间的生成和部署说明。
- *特定于环境的项目文件*(*Env Dev.proj*)。 这包含特定于特定的目标环境的生成和部署设置。 例如，可以使用*Env Dev.proj*文件提供用于开发人员或测试环境的设置并创建一个名为的备用文件*Env Stage.proj*来提供设置过渡环境。
- A*命令文件*(*发布 Dev.cmd*)。 这包含指定你的项目文件的命令想要执行的 MSBuild.exe。 你可以创建命令文件对于每个目标环境，其中的每个文件都包含的 MSBuild.exe 命令时，指定不同的特定于环境的项目文件。 这使开发人员只需通过运行相应的命令文件，将部署到不同的环境。

在示例解决方案中，你可以找到这些发布解决方案文件夹中的三个文件。

![](understanding-the-build-process/_static/image1.png)

在这些文件中更详细地讨论之前，让我们看看时使用此方法的总体的生成过程的工作原理。 在高级别中，生成和部署过程如下所示：

![](understanding-the-build-process/_static/image2.png)

第一件事是两个项目文件 （&） #x 2014; 一个包含通用生成和部署说明，以及一个包含特定于环境的设置 （&） #x 2014; 将合并到单个项目文件。 MSBuild 合作通过项目文件中的说明。 它将生成每个解决方案，每个项目使用项目文件中的项目。 然后，它调用到其他工具，如 Web Deploy (MSDeploy.exe) 并且将你的 web 内容和数据库部署到目标环境在 VSDBCMD 实用程序。

从开始到结束，生成和部署过程执行这些任务：

1. 它将删除输出目录中，在全新的版本的准备过程中的内容。
2. 生成解决方案中的每个项目：

    1. 面向 web 项目和 #x 2014; 在这种情况下，ASP.NET MVC web 应用程序和 WCF web 服务和 #x 2014年; 生成过程创建 web 部署包的每个项目。
    2. 对于数据库项目，生成过程创建的每个项目的部署清单 （.deploymanifest 文件）。
3. 它使用 VSDBCMD.exe 实用程序来部署每个数据库项目中使用各种属性从项目文件 （#x 2014年; 目标连接字符串和数据库名称） #x 2014年; 以及.deploymanifest 文件的解决方案。
4. 它使用 MSDeploy.exe 实用程序来部署解决方案，使用项目文件中的各种属性来控制部署过程中每个 web 项目。

示例解决方案可用于跟踪此过程的更多详细信息。

> [!NOTE]
> 有关如何自定义服务器环境的特定于环境的项目文件的指南，请参阅[配置为目标环境的部署属性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="invoking-the-build-and-deployment-process"></a>调用的生成和部署过程

若要部署到开发人员测试环境的联系人管理器解决方案，开发人员运行*发布 Dev.cmd*命令文件。 这将调用 MSBuild.exe，指定*Publish.proj*作为要执行的项目文件和*Env Dev.proj*作为参数值。


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl**切换 (短，无法用于**/fileLogger**) 将生成输出记录到名为的文件*msbuild.log*当前目录中。 有关详细信息，请参阅[MSBuild 命令行参考](https://msdn.microsoft.com/library/ms164311.aspx)。


此时，MSBuild 开始运行，加载*Publish.proj*文件，并开始处理其中的说明进行操作。 第一个指令告知导入项目的 MSBuild 文件**TargetEnvPropsFile**参数指定。


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile**参数指定*Env Dev.proj*文件，因此 MSBuild 将的内容合并*Env Dev.proj*文件到*Publish.proj*文件。

MSBuild 遇到在合并的项目文件中的下一个元素是属性组。 在文件中出现的顺序处理属性。 MSBuild 创建提供满足任何指定的条件的键 / 值对每个属性。 更高版本的文件中定义的属性将覆盖具有相同名称在文件中前面定义的任何属性。 例如，考虑**OutputRoot**属性。


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


当 MSBuild 处理第一个**OutputRoot**元素，提供类似名称的参数未提供，它将的值设置**OutputRoot**属性**...\Publish\Out**。当它遇到第二个**OutputRoot**元素，如果条件计算结果为**true**，它将覆盖的值**OutputRoot**具有的值的属性**OutDir**参数。

MSBuild 遇到下一个元素是一个包含名为的单个项组**ProjectsToBuild**。


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild 的生成名为的项列表来处理此指令**ProjectsToBuild**。 在这种情况下，项列表包含的单个值 （&） #x 2014年; 的路径和文件名的解决方案文件。

此时，剩余的元素是目标。 目标属性和项和 #x 2014年从以不同方式处理; 实质上，目标不会处理除非显式将它们指定用户或项目文件中的另一构造由调用。 回想一下，打开**项目**标记包含**DefaultTargets**属性。


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


这会指示 MSBuild 来调用**FullPublish**目标，如果目标不指定何时调用 MSBuild.exe。 **FullPublish**目标不包含任何任务; 而是只需指定依赖项的列表。


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


此依赖关系指示 MSBuild 该按顺序执行**FullPublish**目标，它必须调用此列表中提供的顺序的目标：

1. 它必须调用**清理**目标。
2. 它必须调用**BuildProjects**目标。
3. 它必须调用**GatherPackagesForPublishing**目标。
4. 它必须调用**PublishDbPackages**目标。
5. 它必须调用**PublishWebPackages**目标。

### <a name="the-clean-target"></a>Clean 目标

**清理**目标基本上删除输出目录及其所有内容，在准备进行全新的版本。


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


请注意，目标包括**ItemGroup**元素。 当你定义的属性或中的项**目标**元素，你要创建*动态*属性和项。 换而言之，属性或项未处理执行目标时才可以。 输出目录可能不存在，或包含的任何文件，直到生成过程开始时，因此你不能生成 **\_FilesToDelete**为静态项列表; 你需要等待执行之前。 在这种情况下，作为在目标内的动态项生成列表。

> [!NOTE]
> 在这种情况下，因为**清理**目标是要执行的第一个，因此没有真正需要使用动态项组。 但是，当您可能想要按不同的顺序在某一时刻执行的目标是在这种情况下，使用动态属性和项的好办法。  
> 您还应避免将声明将永远不会使用的项的目标。 如果你将仅由特定的目标的项，请考虑将它们放置在要删除任何不必要的开销的生成过程的目标。


动态到某个位置，项**清理**目标将非常简单，并使用内置**消息**，**删除**，和**RemoveDir**到任务：

1. 将一条消息发送到记录器。
2. 生成要删除文件的列表。
3. 删除这些文件。
4. 删除输出目录。

### <a name="the-buildprojects-target"></a>BuildProjects 目标

**BuildProjects**目标基本上生成的所有项目中的示例解决方案。


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


此目标中，在前面的主题中，某些详细信息中所述[了解项目文件](understanding-the-project-file.md)，来说明如何任务和目标引用属性和项。 此时，你主要感兴趣**MSBuild**任务。 此任务可用于生成多个项目。 任务不会创建 MSBuild.exe; 的新实例它使用当前正在运行的实例来生成每个项目。 在此示例中的关键点是相关的部署属性：

- **DeployOnBuild**属性指示 MSBuild 项目设置中运行任何部署说明，完成每个项目生成时。
- **DeployTarget**属性标识你想要调用后生成项目的目标。 在这种情况下，**包**目标生成项目输出到一个可部署 web 包中。

> [!NOTE]
> **包**目标调用 Web 发布管道 (WPP)，它提供 MSBuild 和 Web 部署之间的集成。 如果你想要看一看的内置目标 WPP 提供，查看*Microsoft.Web.Publishing.targets*在 %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 文件夹中的文件。


### <a name="the-gatherpackagesforpublishing-target"></a>GatherPackagesForPublishing 目标

如果你先研究**GatherPackagesForPublishing**目标，你会注意到，它实际上并不包含任何任务。 相反，它包含的单个项组的定义三个动态的项。


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


这些项是指创建时的部署包**BuildProjects**目标已执行。 你无法定义，这些项以静态方式在项目文件中，因为项目所引用的文件不存在直到**BuildProjects**执行目标。 相反，项必须定义动态目标不会被调用直到内后**BuildProjects**执行目标。

在此目标 （&） #x 2014年中不使用项; 此目标中，只需生成的项和每个项值关联的元数据。 这些元素处理后， **PublishPackages**项将包含两个值的路径*ContactManager.Mvc.deploy.cmd*文件和路径*ContactManager.Service.deploy.cmd*文件。 Web 部署作为每个项目，web 包的一部分创建这些文件，并且这些是你必须调用的文件以便部署包的部署目标服务器上。 如果你打开这些文件之一，则基本上会看到使用各种生成特定的参数值的 MSDeploy.exe 命令。

**DbPublishPackages**项将包含单个值的路径*ContactManager.Database.deploymanifest*文件。

> [!NOTE]
> 在生成数据库项目，和它将使用 MSBuild 项目文件相同的架构时，会生成一个.deploymanifest 文件。 它包含所有必需部署数据库，包括数据库架构 (.dbschema) 的位置和任何预先部署脚本和后期部署脚本的详细信息的信息。 有关详细信息，请参阅[概述的数据库生成和部署](https://msdn.microsoft.com/library/aa833165.aspx)。


你将了解有关如何创建和使用在部署包和数据库部署清单的详细信息[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)和[部署数据库项目](deploying-database-projects.md)。

### <a name="the-publishdbpackages-target"></a>PublishDbPackages 目标

简要来讲， **PublishDbPackages**目标调用 VSDBCMD 实用程序来部署**ContactManager**到目标环境的数据库。 配置数据库部署涉及大量的决策和细微差异，并且你将了解有关这个中[部署数据库项目](deploying-database-projects.md)和[将数据库部署自定义为多个环境](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). 在本主题中，我们将重点此目标的实际函数。

首先，请注意，开始标记包含**输出**属性。


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


这是一个示例的*目标批处理*。 在 MSBuild 项目文件中，批处理是一种技术来循环访问集合。 值**输出**属性， **"%(DbPublishPackages.Identity)"**，是指**标识**元数据属性**DbPublishPackages**项列表。 这一表示法，**Outputs=%***(ItemList.ItemMetadataName)*，将被转换为：

- 拆分中的项**DbPublishPackages**为包含相同的项目的多个批**标识**元数据值。
- 执行一次每批的目标。

> [!NOTE]
> **标识**是之一[内置的元数据值](https://msdn.microsoft.com/library/ms164313.aspx)，分配给在创建的每个项。 它引用的值**包括**属性中**项**元素和 #x 2014年; 换而言之的路径和文件名的项。


在这种情况下，应该永远不会有多个项具有相同的路径和文件名，因为我们实质上正在使用的一个批次大小。 对于每个数据库包，目标被执行一次。

你可以看到中的类似表示法 **\_Cmd**属性，用于生成具有相应的开关的 VSDBCMD 命令。


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


在这种情况下， **%(DbPublishPackages.DatabaseConnectionString)**， **%(DbPublishPackages.TargetDatabase)**，和**%(DbPublishPackages.FullPath)**所有引用元数据值的**DbPublishPackages**项集合。  **\_Cmd**属性由**Exec**任务，调用该命令。


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


由于这一表示法， **Exec**任务将创建基于的独特组合的批次**DatabaseConnectionString**， **TargetDatabase**，和**FullPath**元数据值，并且任务将执行一次每个批处理。 这是一个示例的*任务批处理*。 但是，因为目标级别批处理已划分为单个项的批次，我们项集合**Exec**任务将运行一次，并且每个迭代的目标的一次。 换而言之，此任务时，将调用一次为解决方案中每个数据库包 VSDBCMD 实用程序。

> [!NOTE]
> 目标和任务批处理的详细信息，请参阅 MSBuild[批处理](https://msdn.microsoft.com/library/ms171473.aspx)，[目标批处理中的项元数据](https://msdn.microsoft.com/library/ms228229.aspx)，和[任务批处理中的项元数据](https://msdn.microsoft.com/library/ms171474.aspx)。


### <a name="the-publishwebpackages-target"></a>PublishWebPackages 目标

通过此点已调用**BuildProjects**目标，生成的每个项目的 web 部署包中的示例解决方案。 伴随每个包会*deploy.cmd*文件，其中包含将包部署到目标环境所需的 MSDeploy.exe 命令，和一个*SetParameters.xml*文件，它指定目标环境的必要的详细信息。 您已调用**GatherPackagesForPublishing**目标，生成一个项集合，其中包含*deploy.cmd*你感兴趣的文件。 从根本上来说， **PublishWebPackages**目标执行这些功能：

- 它操作*SetParameters.xml*每个包以便包含目标环境的正确详细信息的文件使用**XmlPoke**任务。
- 它将调用*deploy.cmd*文件对于每个包，使用相应的开关。

就像**PublishDbPackages**目标， **PublishWebPackages**目标使用目标批处理以确保目标执行一次，每个 web 包。


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


在目标， **Exec**任务用于运行*deploy.cmd*为每个 web 包的文件。


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


有关配置 web 包的部署的详细信息，请参阅[生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)。

## <a name="conclusion"></a>结束语

本主题提供如何使用拆分项目文件来控制从开始到完成的联系人管理器示例解决方案的生成和部署过程的演练。 使用此方法只需通过运行特定于环境的命令文件允许你运行复杂，在单个、 可重复执行步骤中，企业级部署。

## <a name="further-reading"></a>其他阅读材料

项目文件和 WPP 的更深入介绍，请参阅[内 Microsoft 生成引擎： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。

>[!div class="step-by-step"]
[上一页](understanding-the-project-file.md)
[下一页](building-and-packaging-web-application-projects.md)
