---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: "打包过程的故障排除 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何使用在 M EnablePackageProcessLoggingAndAssert 属性可以收集有关打包过程的详细的信息..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 977077357eb5774193a40c55fabee9733dd5ab2f
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="troubleshooting-the-packaging-process"></a>打包过程的故障排除
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何使用可以收集有关打包过程的详细的信息**EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) 中的属性。
> 
> 当你将设置**EnablePackageProcessLoggingAndAssert**属性**true**，MSBuild 将：
> 
> - 将有关打包过程的其他信息添加到生成日志。
> - 例如，记录在某些情况下的错误，如果打包列表中找到重复的文件。
> - 创建中的日志目录*ProjectName*\_包文件夹，并使用它来记录有关要打包的文件信息。
> 
> 如果在打包过程失败，或你的 web 部署包不包含预期的文件，你可以使用此信息进行故障排除的进程和定点其中事情的进展状况错误。
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert**属性仅适用如果生成你的项目使用**调试**配置。 在其他配置，将忽略此属性。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器解决方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; 来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>了解 EnablePackageProcessLoggingAndAssert 属性

[生成和打包 Web 应用程序项目](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)描述如何 Web 发布管道 (WPP) 提供了一套扩展 MSBuild 的功能，使其能够与 Internet Information Services (IIS) Web 集成 MSBuild 目标部署工具 （Web 部署）。 当打包 web 应用程序项目时，在调用 WPP 目标。

大量这些 WPP 目标包括记录的其他信息的条件逻辑时**EnablePackageProcessLoggingAndAssert**属性设置为**true**。 例如，如果你查看**包**目标，你可以看到它创建的其他日志目录和文件的列表写入一个文本文件中，如果**EnablePackageProcessLoggingAndAssert**等于**true**。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> 在中定义的 WPP 目标*Microsoft.Web.Publishing.targets*在 %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 文件夹中的文件。 你可以打开此文件，并查看在 Visual Studio 2010 或任何 XML 编辑器中的目标。 请注意不要修改文件的内容。


## <a name="enabling-the-additional-logging"></a>启用其他日志记录

你可以提供的值**EnablePackageProcessLoggingAndAssert**以多种方式，具体取决于如何生成你的项目的属性。

如果你从命令行的项目生成时，你可以提供的值**EnablePackageProcessLoggingAndAssert**作为命令行自变量的属性：


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


如果你使用自定义项目文件生成你的项目，你可以包括**EnablePackageProcessLoggingAndAssert**中的值**属性**属性**MSBuild**任务：


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


如果你使用 Team Foundation Server (TFS) 生成定义生成你的项目，则可以提供的值**EnablePackageProcessLoggingAndAssert**中的属性**MSBuild 参数**行：![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> 有关创建和配置生成定义的详细信息，请参阅[创建生成定义，支持部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。


或者，如果你想要包含在每次生成中的包，你可以修改为 web 应用程序项目设置的项目文件**EnablePackageProcessLoggingAndAssert**属性**true**。 应将属性添加到第一个**PropertyGroup**在.csproj 或.vbproj 文件中的元素。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>查看日志文件

当生成并打包 web 应用程序项目**EnablePackageProcessLoggingAndAssert**设置为**true**，MSBuild 会创建名为登录的其他文件夹*ProjectName*\_包文件夹。 日志文件夹包含各种文件：

![](troubleshooting-the-packaging-process/_static/image2.png)

你看到的文件的列表将因你的项目和你的生成过程中的内容。 但是，这些文件通常用于记录的打包，各个阶段的过程 WPP 正在收集的文件的列表：

- *PreExcludePipelineCollectFilesPhaseFileList.txt*文件列出了 MSBuild 收集的有关打包排除指定任何文件被删除之前的文件。
- *AfterExcludeFilesFilesList.txt*后将删除指定排除任何文件，文件将包含的修改后的文件列表。

    > [!NOTE]
    > 有关从打包过程中排除文件和文件夹的详细信息，请参阅[排除文件和文件夹从部署](excluding-files-and-folders-from-deployment.md)。
- *AfterTransformWebConfig.txt*文件列出了收集后任何打包的文件*Web.config*转换已执行。 在此列表中，任何配置特定*Web.config*转换文件，如*Web.Debug.config*和*Web.Release.config*，从列表中的文件排除打包。 单个转换*Web.config*包含在它们的位置。
- *PostAutoParameterizationWebConfigConnectionStrings.txt*文件后的连接字符串中包含的文件列表*Web.config*拥有已参数化的文件。 这是允许你在部署程序包时，将连接字符串替换为你的目标环境的正确设置的过程。
- *Prepackage.txt*文件包含的文件在包中包括的完成预生成列表。

> [!NOTE]
> 额外的日志文件的名称通常与 WPP 目标相对应。 你可以通过检查查看这些目标*Microsoft.Web.Publishing.targets*在 %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 文件夹中的文件。


如果 web 包的内容不是所期望的内容，查看这些文件非常有用的方式来标识在过程操作中的哪个点发生了错误。

## <a name="conclusion"></a>结束语

本主题描述如何使用**EnablePackageProcessLoggingAndAssert** MSBuild 进行故障排除在打包过程中的属性。 它解释你可以在其中提供到生成过程中，属性值的不同方法，它描述了加密时，将属性设置为记录的其他信息**true**。

## <a name="further-reading"></a>其他阅读材料

使用自定义 MSBuild 项目文件来控制部署过程的详细信息，请参阅[了解项目文件](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。 WPP 和它如何管理打包过程的详细信息，请参阅[生成和打包 Web 应用程序项目](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。 有关如何从 web 部署包中排除特定文件和文件夹的指南，请参阅[排除文件和文件夹从部署](excluding-files-and-folders-from-deployment.md)。

>[!div class="step-by-step"]
[上一篇](running-windows-powershell-scripts-from-msbuild-project-files.md)
