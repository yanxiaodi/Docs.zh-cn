---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: "Web 部署在企业中的 |Microsoft 文档"
author: jrjlee
description: "本教程介绍如何迎接大量管理的企业级 web 应用程序到 devel 部署时遇到的难题..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 6210d01f65bcadf8ae4209e372d5aac68861bd7a
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="web-deployment-in-the-enterprise"></a>企业中的 web 部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教程介绍如何满足大量管理的企业级 web 应用程序到开发、 测试、 过渡和生产环境部署时遇到的难题。 本教程包括参考解决方案，以及混合的概念性和面向任务的内容，以指导你确定各种常见任务和过程。
> 
> 这些教程的意大利语翻译，请访问[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


## <a name="enterprise-deployment-challenges"></a>企业部署难题

它们看起来复杂，企业级解决方案的部署进行管理时，组织通常会遇到这些挑战：

- 你需要能够将项目部署到多个环境，如开发人员或测试环境中，暂存平台和生产服务器。 需要使用不同的配置设置为每个环境中部署的解决方案。
- 需要单步执行或自动生成和部署过程的一部分同时部署多个依赖项目。
- 你需要能够到驱动器部署从一个自动化过程。 例如，你想要使用持续集成 (CI) 进程时在签入新代码部署到测试环境的 web 应用程序。
- 你需要能够控制部署过程并从 Visual Studio 外部，设置部署变量如开发人员一般不具备正确的配置设置或为每个目标环境所需的凭据。
- 你需要部署基于架构的数据库项目并保留在后续部署上的现有数据。
- 你需要部署上临时成员资格数据库而不部署用户帐户数据。 你可能还需要更新已部署的成员资格数据库的架构，而不会丢失现有用户帐户数据。
- 你需要在将内容部署到各种目标环境时排除特定文件或文件夹。

## <a name="overview-of-approach"></a>方法的概述

本教程中，以及此系列中的其他教程使用此高级的方法来满足上文所述的挑战。

- **使用自定义 Microsoft Build Engine (MSBuild) 项目文件来控制整个生成和部署过程。**
- 这样可以生成和部署解决方案中的每个项目作为一个可编脚本的操作的一部分。
- 使用简单的特定于环境的项目文件配置特定于环境的设置。 与使用解决方案配置的 Visual Studio – 以中心的方法和发布配置文件配置不同的环境的部署，此方法允许你配置和管理从 Visual Studio 外部的部署过程。 这意味着开发人员不需要提前知道连接字符串、 服务终结点、 服务器凭据和目标环境的其他部署变量。
- Team Build Team Foundation Server (TFS) 工作流的一部分，可以调用自定义项目文件。 这允许您配置 CI 方案的自动的部署。

**使用 Internet 信息服务 (IIS) Web 部署工具 （Web 部署） 打包和部署 web 应用程序项目。**

- Web 部署提供一个框架，允许您打包并将你的 web 应用程序内容部署到的目标 IIS web 服务器，以及依赖项、 配置设置、 安全设置和任何其他要求。
- 在自定义 MSBuild 项目文件中，可以控制从整个的打包和部署过程。 您还可以操作伴随 web 部署包，如连接字符串、 服务终结点和 IIS 目标详细信息的配置设置。
- Web 部署，以及 Web 发布管道，提供大量扩展点允许你自定义你的部署。 例如，很容易从您的 web 部署包中排除不需要的文件和文件夹。

**使用 VSDBCMD.exe 实用程序来部署和更新数据库架构。**

- VSDBCMD 可以部署中生成 Visual Studio 数据库项目时，生成的数据库架构文件 (.dbschema) 的数据库。 与此相反，Web 部署中包含的数据库部署功能是更适合为从本地 SQL Server 实例部署现有数据库。
- 与用于部署数据库项目的 Visual Studio 的功能，不同 VSDBCMD 可让你部署到现有的目标数据库的差异的更新。 这样，你可以升级数据库架构时保留任何现有数据。
- 你可以在自定义 MSBuild 项目文件中执行从 VSDBCMD 命令。

## <a name="content-map"></a>内容地图

本教程包括分为四个主要方面的主题。

这些主题介绍参考解决方案 （#x 2014年; 联系人管理器解决方案） #x 2014;，并描述如何下载它并将其配置本地计算机上：

- [Contact Manager 解决方案](the-contact-manager-solution.md)
- [设置 Contact Manager 解决方案](setting-up-the-contact-manager-solution.md)

这些主题介绍 MSBuild 项目文件，描述如何创建和使用自定义项目文件和演练联系人管理器解决方案的部署过程：

- [了解项目文件](understanding-the-project-file.md)
- [了解生成过程](understanding-the-build-process.md)

这些主题描述 web 应用程序部署，包括如何生成和打包进程的工作原理、 如何与 Web 发布管道集成生成过程、 如何修改部署参数和如何将 web 包部署到目标环境：

- [生成和打包 Web 应用程序项目](building-and-packaging-web-application-projects.md)
- [为 Web 程序包部署配置参数](configuring-parameters-for-web-package-deployment.md)
- [部署 Web 程序包](deploying-web-packages.md)

- [部署数据库项目](deploying-database-projects.md)介绍不同的技术可用于部署 Visual Studio 数据库项目，以及每种方法的优缺点。 [创建并运行部署命令文件](creating-and-running-a-deployment-command-file.md)描述如何创建一个简单的命令文件，它封装你的部署逻辑，并可以为单步执行过程中部署复杂的解决方案。
- 最后，[手动安装 Web 包](manually-installing-web-packages.md)到教程结束通过显示你可以将 web 包导入到 IIS。

## <a name="key-technologies"></a>主要的技术

在本教程中的主题主要使用这些技术来管理生成和部署：

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- VSDBCMD.exe 数据库部署实用工具

## <a name="other-tutorials-in-this-series"></a>在本系列中其他教程

它在企业级 web 部署构成的五个教程系列中的一部分。 这些是序列中的其他教程：

- [部署在企业方案中的 Web 应用程序](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 本介绍性内容的系列教程提供上下文的背景。 描述该教程的方案，以及它阐释了任务和在整个系列所述的演练如何适应更广泛的应用程序生命周期管理 (ALM) 过程。
- [用于 Web 部署配置服务器环境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教程介绍如何配置 Windows server 以支持各种部署方案，包括使用 Web 部署代理服务 （远程代理） 或 Web 部署处理程序和远程数据库部署的远程 web 包部署。 它提供有关选择你自己的环境的适当的部署方法的指导，并描述了如何使用 Web Farm Framework (WFF) 在服务器场中的所有 web 服务器上复制已部署的 web 应用程序。
- [用于 Web 部署配置 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教程介绍如何配置 TFS 以支持各种部署方案，包括 CI 过程的一部分的自动的部署和手动触发的特定生成的部署。
- [高级企业级 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。 本教程介绍如何完成各种更高级的部署任务，如自定义数据库部署为多个环境、 从部署中排除文件和文件夹以及在部署过程中将 web 应用程序脱机.

>[!div class="step-by-step"]
[下一篇](the-contact-manager-solution.md)
