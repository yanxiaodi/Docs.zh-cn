---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "创建并运行部署命令文件 |Microsoft 文档"
author: jrjlee
description: "本主题介绍如何构建命令文件，以便可以运行使用 Microsoft Build Engine (MSBuild) 项目文件作为单步重新部署..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="creating-and-running-a-deployment-command-file"></a>创建并运行部署命令文件
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主题介绍如何生成一个，可以运行作为一个单步执行、 可重复的过程中使用 Microsoft Build Engine (MSBuild) 项目文件的部署的命令文件。


本主题窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器](the-contact-manager-solution.md)来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序解决方案 （&) #x 2014;Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解该生成过程](understanding-the-build-process.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="process-overview"></a>过程概述

在本主题中，你将学习如何创建和运行使用这些项目文件来执行可重复部署到你的目标环境的命令文件。 实质上，命令文件只需包含的 MSBuild 命令的：

- 指示要执行环境不可知的 MSBuild *Publish.proj*文件。
- 告知*Publish.proj*哪些文件包含特定于环境的项目设置和在哪里可以找到它的文件。

## <a name="create-an-msbuild-command"></a>创建的 MSBuild 命令

中所述[了解该生成过程](understanding-the-build-process.md)，特定于环境的项目文件 （&) #x 2014; 例如， *Env Dev.proj*& #x 2014; 设计要导入环境不可知*Publish.proj*在生成时的文件。 在一起，这两个文件提供一组完整的说明，以告诉 MSBuild 如何生成和部署你的解决方案。

*Publish.proj*文件使用**导入**元素导入特定于环境的项目文件。


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


在这种情况下，当你使用 MSBuild.exe 生成和部署联系人管理器解决方案，你需要：

- 在上运行 MSBuild.exe *Publish.proj*文件。
- 通过提供一个名为的命令行参数来指定特定于环境的项目文件的位置**TargetEnvPropsFile**。

若要执行此操作，MSBuild 命令应类似如下：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


从这里，它将是简单的步骤将移至可重复、 单步执行部署。 你需要做是将 MSBuild 命令添加到一个.cmd 文件。 在联系人管理器解决方案中，发布文件夹包含名为的文件*发布 Dev.cmd*正好可以实现此执行。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl**开关将指示 MSBuild 将创建一个名为的日志文件*msbuild.log*在其中调用 MSBuild.exe 的工作目录中。


若要部署或重新部署联系人管理器解决方案，你需要做运行*发布 Dev.cmd*文件。 在您运行该文件，MSBuild 将：

- 生成解决方案中的所有项目。
- 生成 web 应用程序项目的可部署 web 包。
- 生成数据库项目的.dbschema 和.deploymanifest 文件。
- 将 web 包部署到 web 服务器。
- 将数据库部署到数据库服务器。

## <a name="run-the-deployment"></a>运行部署

当你已为你的目标环境中创建命令文件时，你应能够通过只需运行该文件来完成整个部署。

**若要将联系人管理器解决方案部署到你的测试环境**

1. 你开发人员在工作站上，打开 Windows 资源管理器，然后浏览到的位置*发布 Dev.cmd*文件。
2. 双击该文件以运行它。
3. 如果**打开的文件 – 安全警告**显示对话框，请单击**运行**。
4. 如果你的配置设置和测试服务器设置是否正确，命令提示符窗口将显示**生成成功**消息时 MSBuild 已完成处理的项目文件。

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. 如果这是首次已将解决方案部署到此环境，你将需要添加到测试 web 服务器计算机帐户**db\_datawriter**和**db\_datareader**上的角色**ContactManager**数据库。 中介绍了此过程[配置用于 Web 部署发布的数据库服务器](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。

    > [!NOTE]
    > 只需创建数据库时分配这些权限。 默认情况下，生成过程将不会重新创建每个部署和 #x 2014年上的数据库; 相反，它将比较最新的架构的现有数据库并进行所需更改。 因此，只需将这些数据库角色映射第一次部署解决方案。
6. 打开 Internet Explorer 并浏览到联系人管理器应用程序的 URL (例如， `http://testweb1:85/ContactManager/`)。
7. 验证应用程序按预期方式工作并且你能够添加联系人。

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>结束语

创建命令文件包含 MSBuild 说明可为你提供快速轻松地构建和部署到特定目标环境的多项目解决方案中。 如果需要反复将你的解决方案部署到多个目标环境，你可以创建多个命令文件。 在每个命令文件中，MSBuild 命令将生成相同的通用项目文件中，但它将指定不同的特定于环境的项目文件。 例如，若要将发布到开发人员或测试环境的命令文件可能包含此 MSBuild 命令：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


要将发布到过渡环境的命令文件可能包含此 MSBuild 命令：


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> 有关如何自定义服务器环境的特定于环境的项目文件的指南，请参阅[配置为目标环境的部署属性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


你还可以通过重写属性或在 MSBuild 命令中设置各种其他开关自定义生成过程中的为每个环境。 有关详细信息，请参阅[MSBuild 命令行参考](https://msdn.microsoft.com/library/ms164311.aspx)。

>[!div class="step-by-step"]
[上一页](deploying-database-projects.md)
[下一页](manually-installing-web-packages.md)
