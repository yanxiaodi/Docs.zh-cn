---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: 了解项目文件 |Microsoft 文档
author: jrjlee
description: Microsoft Build Engine (MSBuild) 项目文件位于生成和部署过程的核心。 本主题开头的 MSBuild 的概念性概述...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 09c3793e9cdddb7c42cf966f2d079245f441540c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="understanding-the-project-file"></a>了解项目文件
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft Build Engine (MSBuild) 项目文件位于生成和部署过程的核心。 本主题开头的 MSBuild 和项目文件的概念性概述。 它描述工作时使用项目文件，它通过举例说明如何使用项目文件来部署实际应用程序工作将遇到的关键组件。
> 
> 你将学习：
> 
> - 如何 MSBuild 使用 MSBuild 项目文件以生成项目。
> - 如何与部署技术，如 Internet 信息服务 (IIS) Web 部署工具 （Web 部署），MSBuild 相集成。
> - 如何了解项目文件的关键组件。
> - 如何使用项目文件生成和部署复杂应用程序。


## <a name="msbuild-and-the-project-file"></a>MSBuild 和项目文件

当你创建和生成 Visual Studio 中的解决方案时，Visual Studio 将使用 MSBuild 生成解决方案中的每个项目。 每个 Visual Studio 项目包括 MSBuild 项目文件，以反映的类型的项目 （&） #x 2014年文件扩展名; 例如，C# 项目 (.csproj)、 Visual Basic.NET 项目 (.vbproj) 或数据库项目 (.dbproj)。 若要生成项目时，MSBuild 必须处理与项目关联的项目文件。 项目文件是包含所有信息和说明 MSBuild 需要以便生成项目，如要包括平台要求、 版本控制信息、 web 服务器或数据库服务器设置的内容的 XML 文档和必须执行的任务。

MSBuild 项目文件基于[MSBuild XML 架构](https://msdn.microsoft.com/library/5dy88c2e.aspx)，且因此生成过程已完全打开，透明。 此外，你无需安装 Visual Studio，以便使用 MSBuild 引擎和 #x 2014年; MSBuild.exe 可执行文件是.NET Framework 的一部分，你可以从命令提示符中运行它。 作为开发人员，您可以对你自己 MSBuild 项目文件中，使用 MSBuild XML 架构中，以便对设置高级且细化控制如何生成和部署你的项目。 这些自定义项目文件中的 Visual Studio 会自动生成的项目文件的方式完全相同的工作。

> [!NOTE]
> 此外可以使用 Team Foundation Server (TFS) 中的团队生成服务使用 MSBuild 项目文件。 例如，可以使用持续集成 (CI) 方案中的项目文件以在签入新代码时自动部署到测试环境。 有关详细信息，请参阅[自动 Web 部署配置 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。


### <a name="project-file-naming-conventions"></a>项目文件命名约定

在创建您自己的项目文件时，你可以使用你喜欢的任何文件扩展名。 但是，若要使你的解决方案以了解其他人更轻松，你应使用这些常见约定：

- 在创建生成项目的项目文件时，请使用.proj 扩展。
- 在创建导入到其他项目文件的可重用项目文件时，请使用.targets 文件扩展名。 文件扩展名.targets 通常不生成任何内容本身，而是只需包含可以导入.proj 文件的说明。

### <a name="integration-with-deployment-technologies"></a>与部署技术的集成

如果你已使用了 web 应用程序项目在 Visual Studio 2010 中，如 ASP.NET web 应用程序和 ASP.NET MVC web 应用程序，则会知道这些项目包括用于打包和部署 web 应用程序到目标环境的内置支持。 **属性**页这些项目包括**打包/发布 Web**和**打包/发布 SQL**可用于配置的选项卡如何中的各组成部分你打包和部署应用程序。 下面的示例演示**打包/发布 Web**选项卡：

![](understanding-the-project-file/_static/image1.png)

后面这些功能的基础技术称为 Web 发布管道 (WPP) 作为。 WPP 实质上是将 MSBuild 和[Web 部署](https://go.microsoft.com/?linkid=9805122)在一起以 web 应用程序提供完成生成、 包和部署过程。

好消息是你可以充分利用 WPP 提供创建 web 项目的自定义项目文件时的集成点。 你可以在项目文件中，可用于生成项目，创建 web 部署包，并通过单个项目文件和 MSBuild 的单次调用在远程服务器上安装这些程序包包括部署说明。 你还可以作为生成过程的一部分调用任何其他可执行文件。 例如，你可以运行 VSDBCMD.exe 命令行工具来部署架构文件中的数据库。 在本主题的过程中，你将看到，你可以如何充分利用这些功能，以满足你的企业部署方案的要求。

> [!NOTE]
> Web 应用程序部署过程的工作原理的详细信息，请参阅[ASP.NET Web 应用程序项目部署概述](https://msdn.microsoft.com/library/dd394698.aspx)。


## <a name="the-anatomy-of-a-project-file"></a>项目文件的剖析

查看更多详细信息中的生成过程之前，值得花一些时间来熟悉 MSBuild 项目文件的基本结构。 本部分概述了你查看、 编辑或创建的项目文件时遇到的更常用元素。 具体而言，你将学习：

- 如何使用*属性*管理生成过程的变量。
- 如何使用*项*识别生成过程的输入，如代码文件。
- 如何使用*目标*和*任务*向 MSBuild，提供执行指令使用*属性*和*项*中其他位置定义项目文件中。

这将显示 MSBuild 项目文件中的主要元素之间的关系：

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Project 元素

[项目](https://msdn.microsoft.com/library/bcxfsh87.aspx)元素是每个项目文件的根元素。 除了标识项目文件中，XML 架构**项目**元素可以包含特性来指定生成过程的入口点。 例如，在[联系人管理器示例解决方案](the-contact-manager-solution.md)、 *Publish.proj*文件指定生成应由调用名为的目标启动**FullPublish**。


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>属性和条件

项目文件通常需要提供大量不同的为了成功地生成和部署你的项目的信息片段。 这部分信息可能包括服务器名称、 连接字符串、 凭据、 生成配置、 源和目标文件路径和你想要包括以支持自定义的任何其他信息。 在项目文件中，属性必须定义内[PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx)元素。 MSBuild 属性包含的键 / 值对。 在**PropertyGroup**元素中，元素名称定义项的属性和元素的内容定义的属性值。 例如，你可以定义属性名为**ServerName**和**ConnectionString**来存储静态服务器名称和连接字符串。


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


若要检索的属性值，你可以使用格式 **$(***PropertyName***)***。*例如，若要检索的值 **ServerName** 属性，你需要键入：


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> 你将看到示例说明了如何以及何时使用本主题中后面的属性值。


在项目文件中嵌入为静态属性的信息并不总是理想的方法来管理生成过程。 在许多方案，你将想要从其他源获得的信息或使用户能够从命令提示符的信息。 MSBuild，可作为命令行参数指定任何属性值。 例如，用户可以提供的值**ServerName**时他或她运行从命令行的 MSBuild.exe。


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> 有关参数和可以使用 MSBuild.exe 的开关的详细信息，请参阅[MSBuild 命令行参考](https://msdn.microsoft.com/library/ms164311.aspx)。


可以使用相同的属性语法以获取环境变量和内置的项目属性的值。 很多常用的属性由定义，并且它们通过将相关参数名称包含使用项目文件。 例如，若要检索的当前项目平台 （&） #x 2014; 例如， **x86**或**AnyCpu**& #x 2014; 可以包括**$(Platform)**中的属性引用你的项目文件。 有关详细信息，请参阅[用于生成命令和属性的宏](https://msdn.microsoft.com/library/c02as0cs.aspx)，[常用 MSBuild 项目属性](https://msdn.microsoft.com/library/bb629394.aspx)，和[保留属性](https://msdn.microsoft.com/library/ms164309.aspx)。

属性通常用于中结合*条件*。 大多数 MSBuild 元素支持**条件**属性，这样就可以指定在其 MSBuild 应该评估元素的条件。 例如，考虑此属性定义：


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


当 MSBuild 处理此属性定义时，它首先会检查以查看是否**$(OutputRoot)**属性值为可用。 如果属性值为空 （&） #x 2014; 换而言之，用户未为其提供值的此属性 （&） #x 2014年; 条件计算结果为**true**和属性值设置为**...\Publish\Out**。如果用户已为此属性提供一个值，条件计算结果为**false**和不使用静态属性值。

可以在其中指定条件的不同方法的详细信息，请参阅[MSBuild 条件](https://msdn.microsoft.com/library/7szfhaft.aspx)。

### <a name="items-and-item-groups"></a>项目和项组

项目文件的重要的角色之一是定义生成过程的输入。 通常，这些输入是文件和 #x 2014年; 代码文件、 配置文件、 命令文件和你需要处理或将复制为任何其他文件的一部分的生成过程。 在 MSBuild 项目架构中，这些输入由表示[项](https://msdn.microsoft.com/library/ms164283.aspx)元素。 在项目文件中，项必须定义内[ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx)元素。 就像**属性**元素，您可以命名**项**元素你的喜好。 但是，你必须指定**包括**属性用于标识的文件或项表示的通配符。


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


通过指定多个**项**具有相同名称的元素，要有效地创建资源的一个命名的列表。 若要查看此操作中一种好方法是要了解在 Visual Studio 创建的项目文件之一。 例如， *ContactManager.Mvc.csproj*中的示例解决方案文件均包含大量的项组，每个都有多个具有相同的名称**项**元素。


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


项目文件这种方式，指示 MSBuild 构造的需要处理在相同的方式和 #x 2014; 中的文件列表**引用**列表包括程序集必须到位成功生成、 **编译**列表包括必须进行编译的代码文件和**内容**列表包括必须复制不变的资源。 我们将了解如何生成过程引用和使用本主题中后面的这些项。

此外可以包含项元素[ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx)子元素。 这些是用户定义的键 / 值对，并且实质上是表示特定于该项目的属性。 例如，大量的**编译**项目文件中的项元素包括**DependentUpon**子元素。


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> 除了用户创建的项目元数据，所有项都分配上创建的各种公共元数据。 有关详细信息，请参阅[常见项元数据](https://msdn.microsoft.com/library/ms164313.aspx)。


你可以创建**ItemGroup**根级别中的元素**项目**元素或在特定**目标**元素。 **ItemGroup**元素还支持**条件**特性，这样就可以定制根据条件类似的项目配置或平台生成过程的输入。

### <a name="targets-and-tasks"></a>目标和任务

在 MSBuild 架构中，[任务](https://msdn.microsoft.com/library/77f2hx1s.aspx)元素表示程序各个生成指令 （或任务）。 MSBuild 包括许多预定义的任务。 例如:

- **复制**任务将文件复制到新位置。
- **Csc**任务时，将调用 Visual C# 编译器。
- **Vbc**任务时，将调用 Visual Basic 编译器。
- **Exec**任务运行指定的程序。
- **消息**任务会将消息写入一个记录器。

> [!NOTE]
> 有关完整的现成可用的任务的详细信息，请参阅[MSBuild 任务参考](https://msdn.microsoft.com/library/7z253716.aspx)。 有关详细信息的任务，包括如何创建你自己的自定义任务，请参阅[MSBuild 任务](https://msdn.microsoft.com/library/ms171466.aspx)。


任务必须始终包含在[目标](https://msdn.microsoft.com/library/t50z2hka.aspx)元素。 A**目标**元素是一组按顺序执行的一个或多个任务和项目文件可以包含多个目标。 当你想要运行任务或一组任务时，请调用包含它们的目标。 例如，假设有记录一条消息的简单项目文件。


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


你可以通过调用从命令行中，目标**/t**开关指定目标。


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


或者，可以添加**DefaultTargets**属性设为**项目**元素中，并指定你想要调用的目标。


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


在这种情况下，你不需要指定目标从命令行。 你可以只需指定项目文件中，并且会调用 MSBuild **FullPublish**为你的目标。


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


目标和任务可以包括**条件**属性。 在这种情况下，你可以选择忽略整个目标或单个任务，如果符合特定的条件。

通常情况下，当你创建有用的任务和目标，你将需要引用的属性和已在项目文件中其他位置定义的项：

- 若要使用的属性值，键入**$(***PropertyName***)**，其中*PropertyName*是的名称**属性**元素或的名称参数。
- 若要使用的项，请键入**@(***ItemName***)**，其中*ItemName*是的名称**项**元素。

> [!NOTE]
> 请记住，是否具有相同名称创建多个项，你正在生成列表。 与此相反，如果具有相同名称创建多个属性，你提供的最后一个属性值将使用相同的名称 （&） #x 2014年覆盖任何以前的属性; 一个属性可以仅包含单个值。


例如，在*Publish.proj*文件中的示例解决方案中，看一看**BuildProjects**目标。


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


在此示例中，你可以观察这些要点：

- 如果**BuildingInTeamBuild**指定参数，并且的值为**true**，将执行任何中此目标的任务。
- 目标包含的单个实例[MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx)任务。 此任务，可以生成其他 MSBuild 项目。
- **ProjectsToBuild**传递给任务的项。 此项可表示的项目或解决方案文件，所有定义的列表**ProjectsToBuild**项项组中的元素。 在这种情况下， **ProjectsToBuild**项引用的单个解决方案文件。

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- 传递到的属性值**MSBuild**任务包括参数名为**OutputRoot**和**配置**。 如果它们不与参数值，如果提供或静态属性值设置。

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

你还可以看到， **MSBuild**任务时，将调用名为的目标**生成**。 这是一个广泛应用于 Visual Studio 项目文件，并可供你在自定义项目文件，如的几个内置目标**生成**，**清理**，**重新生成**，和**发布**。 你将了解有关使用目标和任务控制生成过程中，以及有关**MSBuild**任务具体而言，本主题中后面。

> [!NOTE]
> 在目标上的详细信息，请参阅[MSBuild 目标](https://msdn.microsoft.com/library/ms171462.aspx)。


## <a name="splitting-project-files-to-support-multiple-environments"></a>拆分项目文件，以支持多个环境

假设你想要能够将解决方案部署到多个环境，如测试服务器、 过渡平台和生产环境。 配置可能有所不同大体上这些环境和 #x 2014年; 不只是在服务器名称、 连接字符串以及等等，但也可能在凭据、 安全设置和其他因素的很多方面。 如果你需要定期执行此操作，不真正有利，每次切换目标环境时编辑项目文件中的多个属性。 也不是一个理想的解决方案需要属性值提供到生成过程的无限列表。

幸运的是一种替代方法。 MSBuild，可以跨多个项目文件拆分生成配置。 若要查看此工作原理，在示例解决方案中，请注意，有两个自定义项目文件：

- *Publish.proj*，其中包含属性，项目，和所共有的所有环境的目标。
- *Env Dev.proj*，其中包含特定于开发人员环境的属性。

现在请注意， *Publish.proj*文件包括[导入](https://msdn.microsoft.com/library/92x05xfs.aspx)元素，紧跟在打开下方的**项目**标记。


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**导入**元素用来将另一个 MSBuild 项目文件的内容导入到当前的 MSBuild 项目文件。 在这种情况下， **TargetEnvPropsFile**参数提供了你想要导入的项目文件的文件名。 运行 MSBuild 时，你可以为此参数提供一个值。


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


这有效地将两个文件的内容合并到单个项目文件。 使用此方法，可以创建包含通用生成配置的一个项目文件和包含特定于环境的属性的多个附属项目文件。 因此，只需使用不同的参数值中运行命令，你将你的解决方案部署到不同的环境。

![](understanding-the-project-file/_static/image3.png)

拆分项目文件以这种方式是遵循一个好办法。 它允许开发人员通过运行单个命令，同时可避免重复通用生成属性跨多个项目文件将部署到多个环境。

> [!NOTE]
> 有关如何自定义服务器环境的特定于环境的项目文件的指南，请参阅[配置为目标环境的部署属性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="conclusion"></a>结束语

本主题提供对 MSBuild 项目文件的常规介绍，并且说明了如何创建您自己的自定义项目文件来控制生成过程。 它还引入了将项目文件拆分为通用生成说明和特定于环境的生成属性，以便可以方便地生成并部署到多个目标的项目的概念。

下一主题[了解该生成过程](understanding-the-build-process.md)，提供更深刻地了解如何通过逐步引导您完成现实级别的复杂性，部署的解决方案使用项目文件添加到控件生成和部署。

## <a name="further-reading"></a>其他阅读材料

项目文件和 WPP 的更深入介绍，请参阅[内 Microsoft 生成引擎： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。

>[!div class="step-by-step"]
[上一页](setting-up-the-contact-manager-solution.md)
[下一页](understanding-the-build-process.md)
