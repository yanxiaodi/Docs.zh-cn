---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: "高级企业级 Web 部署 |Microsoft 文档"
author: jrjlee
description: "本教程将演示如何执行是必需的还是大量的企业部署方案中所需的各种任务。 有关意大利语 translati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: c3cb7f63cf7c0246a0c4da6038a65a6ac43a7b59
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
<a name="advanced-enterprise-web-deployment"></a>高级企业级 Web 部署
====================
通过[Jason Lee](https://github.com/jrjlee)

[下载 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本教程将演示如何执行是必需的还是大量的企业部署方案中所需的各种任务。
> 
> 这些教程的意大利语翻译，请访问[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。


此窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序解决方案 （&) #x 2014;Communication Foundation (WCF) 服务和数据库项目。

这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。 在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。

## <a name="scenario-overview"></a>方案概述

这些教程的高级方案所述[企业 Web 部署： 方案概述](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。 我们建议您开始本教程之前查看此主题。

## <a name="how-to-use-this-tutorial"></a>如何使用本教程

- 每个在本教程中的主题是独立的并解决特定质询或企业部署方案中出现问题。 你无需这些主题中的任何特定顺序完成。 但是，本教程介绍某些高级的任务。 在这种情况下，你应该熟悉的概念和技术的[企业中的 Web 部署](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)教程介绍如何才能获得此内容的最大利益。
- 本教程包括以下主题：
- [执行"假设"部署](performing-a-what-if-deployment.md)。 在大量方案，你将想要确定在目标环境或任何现有内容上的建议部署的影响，实际进行任何更改前。 本主题介绍如何运行"假设"部署生成日志文件和数据库更新脚本，就像在不实际进行任何更改的情况下，具有到目标环境中，部署内容。 分析这些资源可帮助你确认之前实时部署任何潜在问题。
- [自定义数据库部署为多个环境](customizing-database-deployments-for-multiple-environments.md)。 在数据库项目部署到多个目标时，通常需要自定义每个目标环境的部署属性。 例如，在测试环境中你将通常重新创建数据库上每个部署，而在过渡环境或生产环境中将是大量更有可能进行增量更新，以保留数据。 本主题介绍如何才能将这些属性更改为你的部署逻辑通过创建每个目标环境的特定于环境的部署配置 (.sqldeployment) 文件。
- 将数据库角色成员身份部署到测试环境。 当你重新创建数据库上每个部署和 #x 2014年; 例如，作为持续集成 (CI) 生成的一部分并部署到的测试环境 （&） #x 2014; 通常需要配置每次的数据库角色成员身份。 例如，你通常将需要向其授予权限与 web 应用程序关联的应用程序池标识。 本主题介绍如何通过将部署后 SQL 脚本添加到你的部署逻辑自动执行此过程。
- [将成员资格数据库部署到企业环境](deploying-membership-databases-to-enterprise-environments.md)。 ASP.NET 成员资格数据库具有各种特征，可以使部署过程变得复杂。 例如，如果仅有架构的部署，将使数据库处于不可操作状态的状态。 在大多数情况下，最好是直接在每个目标环境中创建的成员资格数据库。 但是，如果需要部署成员资格数据库，本主题介绍一些可用于满足本身存在的问题的方法。
- [从部署中排除文件和文件夹](excluding-files-and-folders-from-deployment.md)。 在某些情况下，你将需要定制到特定目标环境你 web 包的内容。 例如，你可能想要部署到测试环境，以支持客户端调试，但部署到过渡或生产环境时使用的库的缩减的版本时包括的 JavaScript 库的完整版本。 本主题介绍如何你可以排除特定文件和文件夹从包创建进程。
- [拍摄 Web 应用程序脱机与 Web 部署](taking-web-applications-offline-with-web-deploy.md)。 当你将解决方案部署到过渡或生产环境时，通常需要执行部署过程的持续时间内的 web 应用程序脱机。 本主题介绍如何添加*应用\_offline.htm*到 web 应用程序在开始部署过程的文件并将其删除的末尾。 虽然*应用\_offline.htm*文件已到位，浏览到 web 应用程序的任何用户自动重定向到*应用\_offline.htm*文件。
- [MSBuild 中运行 Windows PowerShell 脚本](running-windows-powershell-scripts-from-msbuild-project-files.md)。 许多部署方案需要更复杂的部署后操作，如向注册表添加自定义事件源或配置 SQL Server 实例之间的复制。 这些操作通常都是通过 Windows PowerShell 脚本完成的。 本主题介绍如何从 Microsoft Build Engine (MSBuild) 的项目文件生成和部署过程的一部分运行 Windows PowerShell 脚本。
- [打包过程的故障排除](troubleshooting-the-packaging-process.md)。 Web 发布管道 (WPP) 定义名为 MSBuild 属性**EnablePackageProcessLoggingAndAssert**可用于生成 web 应用程序项目的打包过程有关的详细信息。 本主题描述属性的用途以及如何使用它。

## <a name="key-technologies"></a>主要的技术

本教程重点介绍如何使用这些产品和技术以支持自动的生成和 web 部署：

- Visual Studio 2010 和 Team Foundation Server (TFS) 2010
- MSBuild 和 TFS Team Build
- Internet Information Services (IIS) 7.5
- IIS Web 部署工具 （Web 部署） 2.1
- VSDBCMD.exe 数据库部署实用工具

## <a name="other-tutorials-in-this-series"></a>在本系列中其他教程

它在企业级 web 部署构成的五个教程系列中的一部分。 这些是序列中的其他教程：

- [部署在企业方案中的 Web 应用程序](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。 本介绍性内容的系列教程提供上下文的背景。 描述该教程的方案，以及它阐释了任务和在整个系列所述的演练如何适应更广泛的应用程序生命周期管理 (ALM) 过程。
- [Web 部署在企业中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 本教程提供 MSBuild 项目文件的概念介绍、 WPP、 Web 部署，和其他相关的技术。 它还说明了如何使用这些工具在一起来管理复杂部署进程。
- [用于 Web 部署配置服务器环境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。 本教程介绍如何配置 Windows server 以支持各种部署方案，包括使用 Web 部署代理服务 （远程代理） 或 Web 部署处理程序和远程数据库部署的远程 web 包部署。 它提供有关选择你自己的环境的适当的部署方法的指导，并描述了如何使用 Web Farm Framework (WFF) 在服务器场中的所有 web 服务器上复制已部署的 web 应用程序。
- [用于 Web 部署配置 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。 本教程介绍如何配置 TFS 以支持各种部署方案，包括 CI 过程的一部分的自动的部署和手动触发的特定生成的部署。

>[!div class="step-by-step"]
[下一篇](performing-a-what-if-deployment.md)
